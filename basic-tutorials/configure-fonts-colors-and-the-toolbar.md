# フォント、色、およびツールバーの設定

`config`コマンドを使用して、ユーザーはフォント、テキストおよび背景の色、およびツールバーを構成できます。

### 簡単に言うと

```
usage: config [-s font size][-n font name][-b background color][-f foreground 
color][-c cursor color][-k shape][-dgprth]
```
* `-s font size`: テキストのサイズを変更
* `-n font name`: フォントを変更
* `-b background color`: 背景色を変更
* `-f foreground color`: テキストの色を変更
* `-c cursor color`: カーソルの色を変更
* `-k shape`: カーソルの形状を変更（`shape`はビーム、ブロック、またはアンダーラインにできます）

上記のコマンドでは、`default`を使用して以前に保存された状態に戻すことができ、`factory`を使用して初期の状態に戻すことができます。

* `-d`: 変更を保存せずに以前の状態に戻る
* `-g`: 変更をすべての開いているウィンドウに適用する
* `-p`: 変更を保存して永続的に適用する
* `-r`: 初期の状態に戻る：白い背景と黒いテキスト
* `-t`: ツールバーの構成ファイルを生成
* `-h`: ヘルプテキストを表示

### フォント

まず、自分のコンソールフォントを手動で用意する必要があります。[https://www.nerdfonts.com](https://www.nerdfonts.com)からのNerdフォントを使用することをお勧めします。これにはさまざまなアイコンが含まれています。フォントファイルをデバイスにダウンロードして、iFontsなどのアプリを使用してインストールしてください。

この例では、`UbuntuMono Nerd Font Mono`を使用します。a-Shellに戻り、次のように実行します：

```
$ config -n ‘UbuntuMono Nerd Font Mono’
```

フォントの名前がわからない場合は、`config -n`を使用してフォントメニューを開き、選択してください。

<figure><img src="../.gitbook/assets/4D2C7F31-8B19-4CEC-B5D1-B0940275F88C.jpeg" alt=""><figcaption><p>フォントメニュー</p></figcaption></figure>

### 色

黒い背景と白い/緑のテキストを持つターミナルが欲しいかもしれません。次のように実行します：

```
$ config -b black -f white -c white
```

好きな色を使用してください。

その後、すべての設定を保存します：

```
$ config -gp
```

時折、以下のように見えることがあります：

<figure><img src="../.gitbook/assets/68779072-3337-4915-A0A0-164394ED4052.png" alt=""><figcaption><p>ステータスバーとキーボードはまだ白い</p></figcaption></figure>

待ってください、なぜまだ背景と同じくらい暗い場所がありますか？これらの2つの場所は、デバイスがダークモードのときだけ暗いです。これらを明るくまたは暗く保ちたい場合は、設定アプリで構成できます。設定を開始し、「a-Shell」を左側のメニューで見つけます。今、a-Shellの設定が表示されます：

<figure><img src="../.gitbook/assets/4E304434-1F28-4CED-AD7B-F85D4060DAAD.jpeg" alt=""><figcaption><p>a-Shellの設定メニュー</p></figcaption></figure>

「ツールバーカラー」をクリックすると、4つのオプションが表示されます：`システム設定`、`画面の色による`、`ダークモード`、および`ライトモード`。好みのものを選択してください。

### ツールバー

画面下部のツールバーのボタンもカスタマイズ可能です。まず、ツールバー構成ファイルを生成します：

```
$ config -t
I have created a toolbar configuration file: ~/Documents/.toolbarDefinition
You can now edit it to add or remove buttons to the toolbar.
Changes will take effect when the app restarts.
```

その後、`.toolbarDefinition`をVimまたはPicoで編集できます。ファイルの構成を見てみましょう：

```
# ボタンアイコン 　　　　　アクション 　　　パラメータ
arrow.right.to.line.alt insertString    \u{0009}
chevron.up.square       systemAction    control
escape                  insertString    \u{001B}
doc.on.clipboard        systemAction    paste
separator
arrow.up                systemAction    up
arrow.down              systemAction    down
arrow.left              systemAction    left
arrow.right             systemAction    right
```

ファイルは行に分かれており、各行がボタンを定義しています。それぞれがツールバーの左端と右端を管理する2つの部分があり、`separator` によって区切られています。

各行には3つのパーツがあります：`icon`、`action`、および `parameter`。アイコンはSF Symbolsのシンボルまたは文字列のシンボルであることができます。SF Symbolsの紹介については、[https://developer.apple.com/sf-symbols/](https://developer.apple.com/sf-symbols/) を参照してください。アクションは `insertString`、`systemAction`、または `insertCommand` で、パラメータは具体的に何を行うかを定義します。

`insertString` はボタンが押されたときに文字列を挿入します。このとき、パラメータは挿入される文字列です。`\n` や `\u{0009}` のような特殊文字がサポートされており、Escapeなどのキーを追加するのは難しくありません。それに対して、`systemAction` のパラメータは `up`、`down`、`left`、`right`、`control`、`cut`、`copy`、または `paste` のいずれかであり、`insertCommand` のパラメータは短いコマンドです。このとき、コマンドの出力はカーソル位置に挿入されます。

iOS/iPadOS 16では、Settingsアプリで「iOS/iPadOSツールバーを使用」が有効になっている場合、ボタンは括弧でグループ化されることがあります。これにより便利に整理でき、表示されるタイミングも設定できます。

以下は生成されたファイルに含まれるいくつかの例です：

```markdown
# ボタンの例:
#
# delete.backward       insertString    \u{007F}
# return                insertString    \u{000D}
# switch.2              insertString    vim .toolbarDefinition\n
# calendar.badge.clock  insertCommand   date "+%Y_%m_%d"

# グループの例（iPadおよびiOSスタイルのツールバーのみ）。サブメニューには最大15のコマンドがあります。
# [
#     scissors                      systemAction    cut
#     arrow.up.doc.on.clipboard     systemAction    copy
#     doc.on.clipboard              systemAction    paste
# ] filemenu.and.cursorarrow

# これは実行中のコマンドがない場合にのみ表示されます:
# [="none"
#     ls            insertString    ls -a ~/Documents/
#     uname         insertString    uname -a
#     ping 🍎       insertString    ping www.apple.com
#     date          insertString    date
# ]

# これはVimでMarkdownファイルを編集している場合に表示されます:
# [="vim .*\.md"
#     key               insertString    \u{001B}:q!\n
#     bold              insertString    :s/\\%V.*\\%V./**&**\n
#     italic            insertString    :s/\\%V.*\\%V./*&*\n
#     strikethrough     insertString    :s/\\%V.*\\%V./\\~\\~&\\~\\~\n
# ] contextualmenu.and.cursorarrow
