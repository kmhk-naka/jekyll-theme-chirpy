---
title: 【Python】OpenCVでキャプチャしたフレームを保存する
categories: [Blogging, Tutorial]
tags: [OpenCV, Python, argparse]
date: 2021-01-20 19:51:00 +0900
---

# はじめに
Object Detectionをやりたくて，そのための訓練データを集めるためにWebカメラで撮影することにしました．
その際に作成したコードを公開しようと思います．

本稿では，OpenCVが利用可能な状態を前提としていて，環境構築については説明しませんのでご了承ください．

# 実行環境

- Ubuntu 20.04 LTS
- Python 3.8.5
- opencv-python 4.5.1

# ライブラリの読み込み
このスクリプトで必要になるライブラリをインポートします．

`argparse` については次のセクションで簡単に説明します．

```python
import argparse
import time
import uuid
from pathlib import Path

import cv2
```

# ArgumentParserを使ったオプションの処理
`argparse` はコマンドラインから実行する際にオプションをつけたいときに便利なライブラリになります．

`ArgumentParser` を使うと次のコマンドのように，パラメータを渡して処理することができます．

```terminal
$ python script.py 100 aaa bbb --path xxx/yyy/zzz
```

[公式ドキュメントはこちら](https://docs.python.org/3/library/argparse.html)になります．  
細かな点についてはドキュメントを参照してください．

ここでは，今回設定したオプションについて説明します．

まずはじめに `ArgumentParser` オブジェクトを生成します．

```python
parser = argparse.ArgumentParser()
```

## 引数を追加する
引数を設定するには `add_argument()` メソッド ([ドキュメント](https://docs.python.org/3/library/argparse.html#the-add-argument-method)) を使用します．  
今回追加した引数は次の4つになります．

| `num` | 撮影する画像数を指定します． |
| `labels` | 撮影した画像のラベル名を可変長引数として指定します． |
| `-a` , `--auto` | 撮影方法を手動か自動かを切り替えます．<br>自動の場合は撮影間隔を秒単位で指定します． |
| `-p` , `--path` | 撮影した画像の保存先パスを指定します． |

引数名を指定する際に， `-a` や `--auto` のようにprefixに `-` を付加するとオプション引数となり， `num` のようにすると位置引数となります．
これらの違いは，実行時に引数が省略可能かどうかです．

また，`default` でデフォルトの値や `type` で型の設定ができます．<br>
さらに，`help` には引数の説明を記述できます．

```python
parser.add_argument('num', help='num of images', type=int)
parser.add_argument('labels', nargs='*')
parser.add_argument('-a', '--auto', type=float, default=None)
parser.add_argument('-p', '--path', type=str, default='images')
```

## 引数を解析する
次に渡された引数を解析します．  
引数の解析には `parse_args()` メソッド ([ドキュメント](https://docs.python.org/3/library/argparse.html#the-parse-args-method)) を使用します．

引数を取得する際の注意点として，**省略形の引数名ではエラーになる**という点があります．  
たとえば， `args.path` を `args.p` とするとエラーが出ます．

```python
args = parser.parse_args()

IMAGE_PATH = Path(args.path)

labels = args.labels
number_imgs = args.num
```

# 手動で撮影する関数を作成
それでは，画像をキャプチャして保存する処理を見ていきます．

おおまかな流れとしては次のようになります．

1. `VideoCapture` オブジェクトを生成してカメラを起動します．
   - カメラがすでに使用されていて起動できなかった場合は処理を終了します．
2. 引数で渡されたラベルごとに `for` ループを回します．
   - ラベルごとにディレクトリを作成します．
3. 無限ループでフレームを取得し表示します．
   - スペースキーを押すとフレームを画像として保存します．
   - `Q` キーを押すと処理を終了します．
   - 引数で指定した画像数に達すると無限ループを抜けます．
4. 最後に `VideoCapture` オブジェクトを解放します．

スペースキーの照合に `32` を指定していますが，これはプラットフォームに依る可能性があるようなので注意してください([参考](https://stackoverflow.com/questions/14494101/using-other-keys-for-the-waitkey-function-of-opencv))．

```python
def manual_capture():
    cap = cv2.VideoCapture(0)

    if not cap.isOpened():
        sys.exit()

    for label in labels:
        (IMAGE_PATH / label).mkdir(mode=0o775, parents=True, exist_ok=True)
        print(f'Collecting images for {label}')

        image_num = 0

        while True:
            ret, frame = cap.read()
            cv2.imshow('frame', frame)

            k = cv2.waitKey(1) & 0xFF
            if k == 32:  # Space key
                image_num += 1
                image_name = IMAGE_PATH / label / f'{label}.{uuid.uuid1()}.jpg'
                cv2.imwrite(str(image_name), frame)
            elif k & 0xFF == ord('q'):
                cap.release()
                return
            elif image_num == number_imgs:
                break

    cap.release()
```

# 自動で撮影する関数を作成
次に，一定時間間隔で撮影を行う関数について見ていきます．

基本的な流れは手動の場合と同じになります．

1. VideoCapture オブジェクトを生成してカメラを起動します．
    - カメラがすでに使用されていて起動できなかった場合は処理を終了します．
2. 引数で渡されたラベルごとに for ループを回します．
    - ラベルごとにディレクトリを作成します．
3. 最初にフレームを表示してから3秒のスリープを入れています．
4. 引数で渡された画像数回 for ループを回します．
    - 引数で渡されたインターバルで撮影を繰り返します．
    - Q キーを押すと処理を終了します．
5. 最後に VideoCapture オブジェクトを解放します．

```python
def auto_capture(interval: float):
    cap = cv2.VideoCapture(0)

    if not cap.isOpened():
        sys.exit()

    for label in labels:
        (IMAGE_PATH / label).mkdir(mode=0o775, parents=True, exist_ok=True)
        print(f'Collecting images for {label}')

        ret, frame = cap.read()
        cv2.imshow('frame', frame)
        time.sleep(3)

        for image_num in range(number_imgs):
            ret, frame = cap.read()
            image_name = IMAGE_PATH / label / f'{label}.{uuid.uuid1()}.jpg'

            cv2.imwrite(str(image_name), frame)
            cv2.imshow('frame', frame)
            time.sleep(interval)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                cap.release()
                return

    cap.release()
```

# 手動撮影か自動撮影かを引数から判別する
`--auto` オプションが指定されているかどうかで自動か手動かを判別して該当の関数を呼び出します．

```python
if __name__ == "__main__":
    if args.auto is not None:
        auto_capture(args.auto)
    else:
        manual_capture()
```

# さいごに
スクリプト全体のソースコードを載せておきます．

```python
import argparse
import sys
import time
import uuid
from pathlib import Path

import cv2


parser = argparse.ArgumentParser()
parser.add_argument('num', help='num of images', type=int)
parser.add_argument('labels', nargs='*')
parser.add_argument('-a', '--auto', type=float, default=None)
parser.add_argument('-p', '--path', type=str, default='images')

args = parser.parse_args()

IMAGE_PATH = Path(args.path)

labels = args.labels
number_imgs = args.num


def manual_capture():
    cap = cv2.VideoCapture(0)

    if not cap.isOpened():
        sys.exit()

    for label in labels:
        (IMAGE_PATH / label).mkdir(mode=0o775, parents=True, exist_ok=True)
        print(f'Collecting images for {label}')

        image_num = 0

        while True:
            ret, frame = cap.read()
            cv2.imshow('frame', frame)

            k = cv2.waitKey(1) & 0xFF
            if k == 32:  # Space key
                image_num += 1
                image_name = IMAGE_PATH / label / f'{label}.{uuid.uuid1()}.jpg'
                cv2.imwrite(str(image_name), frame)
            elif k & 0xFF == ord('q'):
                cap.release()
                return
            elif image_num == number_imgs:
                break

    cap.release()


def auto_capture(interval: float):
    cap = cv2.VideoCapture(0)

    if not cap.isOpened():
        sys.exit()

    for label in labels:

        (IMAGE_PATH / label).mkdir(mode=0o775, parents=True, exist_ok=True)
        print(f'Collecting images for {label}')

        ret, frame = cap.read()
        cv2.imshow('frame', frame)
        time.sleep(3)

        for image_num in range(number_imgs):
            ret, frame = cap.read()
            image_name = IMAGE_PATH / label / f'{label}.{uuid.uuid1()}.jpg'

            cv2.imwrite(str(image_name), frame)
            cv2.imshow('frame', frame)
            time.sleep(interval)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                cap.release()
                return

    cap.release()


if __name__ == "__main__":
    if args.auto is not None:
        auto_capture(args.auto)
    else:
        manual_capture()
```