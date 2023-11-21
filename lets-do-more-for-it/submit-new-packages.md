# パッケージの提出

時折、プロジェクトを `pkg` リポジトリにリリースして、誰もがそれを利用できるようにしたいことがあります。現時点では、ほとんどのパッケージがWebAssemblyで配布されていますが、実際にはa-Shellで正常に動作するほぼすべてのものがリリース可能です（ネイティブバイナリコードを除く）。この記事では、`pkg` リポジトリにリリースの準備ができたプロジェクトに焦点を当てています。

### パッケージの構造

公式のリポジトリは [https://github.com/holzschu/a-Shell-commands](https://github.com/holzschu/a-Shell-commands) に保存されています。パッケージを準備するには、インストールスクリプト、manページ、アンインストールスクリプトの3つの部分が必要です。例として `expand` を見てみましょう。

以下はそのインストールスクリプトです：

```bash
#! /bin/sh
# パッケージのデフォルトインストールファイル：

packagename=${0##*/}

# ダウンロードコマンド：
curl -L https://github.com/holzschu/a-Shell-commands/releases/download/0.1/$packagename -o ~/Documents/bin/$packagename --create-dirs --silent
chmod +x ~/Documents/bin/$packagename
# manページのダウンロード
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/man/man1/$packagename.1 -o ~/Library/man/man1/$packagename.1 --create-dirs --silent
# manデータベースのリフレッシュ
mandocdb ~/Library/man
# アンインストール情報のダウンロード：
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/uninstall/$packagename -o ~/Documents/.pkg/$packagename --create-dirs --silent
```

インストールスクリプトは、バイナリファイル（スクリプトの場合もあります）を `~/Documents/bin/` または `~/Library/bin/` にダウンロードします。これは `$PATH` に保存されており、その後manページを取得し、manデータベースを更新し、最後にアンインストールスクリプトをダウンロードします。バイナリファイルはリポジトリ（実際はどこでも）に保存でき、単一のコマンドに対しては `curl` で解析できます。以下はそのアンインストールスクリプトです：

```bash
#! /bin/sh

# パッケージのデフォルトアンインストールファイル：
packagename=${0##*/}

# コマンドの削除
rm ~/Documents/bin/$packagename
# manページの削除
rm ~/Library/man/man1/$packagename.1
# manデータベースのリフレッシュ
mandocdb ~/Library/man
```

アンインストールスクリプトは、パッケージに関するすべてをクリアします：バイナリファイルとmanページ。より複雑なプロジェクトの場合、パッケージに関連するアプリケーションデータを削除することを忘れないでください。

これでわかりました：`pkg install` が実行するためにはインストールスクリプトが必要で、アンインストールスクリプトも同様に必要です（ただし `pkg uninstall` に対応）。通常、manページも必要ですが、必ずしも必要ではないので要件によります。インストールスクリプトはリポジトリに保存されており、パッケージの他の部分がどこに保存されているかを示しています。

### 例

{% hint style="info" %}
この部分はまだ進行中です。
{% endhint %}

`cowsay` はPerlで書かれた古くて有名なプロジェクトです。これをインストールするには、まずGitHubのミラーリポジトリをクローンすることができます：

```
$ lg2 clone https://github.com/schacon/cowsay
```

リポジトリには `install.sh` があります。実際にはa-Shellと `dash` では機能しませんが、これは本質的ではありません。それを再書きしてみましょう。
