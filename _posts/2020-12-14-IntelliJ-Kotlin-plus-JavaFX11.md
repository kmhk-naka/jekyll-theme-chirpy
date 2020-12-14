---
title: IntelliJでKotlinとJavaFX 11を使った開発環境の構築
categories: [Blogging, Tutorial]
tags: [Kotlin, IntelliJ, JavaFX 11]
date: 2020-12-14 15:15:00 +0900
---

# Java SDK 11からJavaFXが切り離された
JavaFXはJDK 11からスタンドアローンなモジュールとして配布されるようになりました．
そのため，JDKとは別にインストールする必要があります．

インストールについては以下のドキュメントを参照してください．

- [Getting Started with JavaFX](https://openjfx.io/openjfx-docs/#install-javafx)


# 環境情報
- Xubuntu 20.04
- IntelljJ IDEA 2020.03 (Community Edition)
- OpenJDK 11
- JavaFX 11

# JavaFX ライブラリの追加
IntelliJにJavaFX ライブラリを認識させるために，PATHの指定をする必要があります．

1. メインメニューから**File > Project Structure...**`(Ctrl+Shift+Alt+S)`を選択します．
2. **Global Libraries**[^global]セクションを開き，**+** をクリックして，Javaを選択します．
3. 以下の画像のように`javafx.***.jar`[^path_to_fx]をすべて追加します．
4. 作成したライブラリを右クリックして，`Add to Modules...`を選択しモジュールに追加します．

![Add JavaFX Libraries](/assets/img/posts/2020-12-14-IntelliJ-Kotlin-plus-JavaFX11/GlobalLibraries.png)

[^global]: **Libraries**ではなく**Global Libraries**に追加しておくと，今後利用する際にまた設定する手間が省けます．
[^path_to_fx]: PATHは各自のインストール先に合わせてください．

# Gradleプラグインの設定
[openjfx/javafx-gradle-plugin](https://github.com/openjfx/javafx-gradle-plugin#javafx-gradle-plugin)に従って`build.gradle`を設定します．

今回，JavaFX SDKはローカルのものを利用します．

```gradle
plugins {
    id("org.openjfx.javafxplugin") version "0.0.9"
}

// 省略

javafx {
    sdk = "/path/to/javafx-sdk"
    modules("javafx.controls", "javafx.fxml")
}
```

# 実行時のVMオプションの設定
実行時のVMオプションにもJavaFX ライブラリを認識させる必要があります．

1. メインメニューから**Run > Edit Configurations...**を選択します．
2. 左側のリストから**Application > Main**を選択します．(無い場合は **+** をクリックして新規作成してください)
3. **Modify options**から**Add VM options**を選択します．
4. VMオプション欄に以下のオプションを指定します．その際，`/path/to/javafx`の部分は各自のPATHに置き換えてください．

```
--module-path /path/to/javafx --add-modules javafx.controls,javafx.fxml
```

![VM Options](/assets/img/posts/2020-12-14-IntelliJ-Kotlin-plus-JavaFX11/VMOption.png)

# JavaFX アプリの実行
最後に，ウィンドウを生成するアプリケーションを実行して確認します．

以下のコードを実行し，ウィンドウが生成されたら設定は問題なく完了しています．

```kotlin
import javafx.application.Application
import javafx.scene.Scene
import javafx.scene.layout.FlowPane
import javafx.stage.Stage

class Main : Application() {
    override fun start(stage: Stage?) {
        val fp = FlowPane()
        val scene = Scene(fp)

        stage?.scene = scene

        stage?.title = "JavaFX Application"
        stage?.x = 100.0
        stage?.y = 100.0
        stage?.width = 700.0
        stage?.height = 400.0

        stage?.show()
    }
}

fun main(args: Array<String>) {
    Application.launch(Main::class.java, *args)
}
```

<br>

***
