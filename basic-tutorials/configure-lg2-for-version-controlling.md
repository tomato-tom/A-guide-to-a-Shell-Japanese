# `lg2`を使ったバージョン管理の設定

長い間、多くのユーザーが使用可能なバージョン管理ツールを探してきました。iOS/iPadOS向けのそのようなツールに関しては、多くのユーザーがWorking CopyやGit Torchなどの特定の目的のためのツールを見るかもしれません。ただし、`git push`なしで「無料」のGitクライアントを受け入れることができるでしょうか？また、一部のアプリには組み込みのGitサポートがありますが、SpckEditor以外はすべて有料です。iSHでは本当にGitを使用できますが、そのプロセスは遅く、不安定です。幸いにも、a-Shellには組み込みのGitサポートがあります。この記事では、a-Shellで`lg2`を使用してバージョン管理をする方法に焦点を当てています。

### Gitコマンドは？

一部の新しいユーザーは、リポジトリに`git`パッケージがあることに気付いているかもしれません。しかし、これは単に`git`コマンドを直接使用できるようにするための`lg2`へのリンクを追加するだけです。これは便利かもしれませんが、`git`と`lg2`の間には違いがあることに注意してください。FSF（Free Software Foundation）のポリシーにより、オリジナルの`git`は含まれていませんが、ほとんどの場合には`lg2`で十分です。`git`パッケージをインストールしている場合でも、違いのために多くのGitディレクトリを管理するためのツールは動作しないことにも注意してください。

さて、`git`コマンドをインストールしてみましょう。

```
$ pkg install git
Downloading git
The git command cannot be included with
a-Shell. Would you like to create a
script at $HOME/Documents/bin/git that
wraps lg2 with the following contents?
#!/bin/sh
lg2 "$@"
Keep in mind that git and lg2 options
are not 100% compatible, and they also
do not use the same configuration files
or environment variables.
Create $HOME/Documents/bin/git? (y/n [n]) y
The $HOME/Documents/bin directory does not exist.
Creating it first.
Creating $HOME/Documents/bin/git
Creation complete
Done
```

これで、`git`または`lg2`のいずれかを使用してGitリポジトリを管理できます。

### SSHの設定

GitHubや他のGitリポジトリにリンクするためにSSHキーが必要な場合があります。まずSSHキーを生成しましょう。

```
$ ssh-keygen -t ed25519 -c "<user name>"
```

一部のユーザーは `rsa` を好むことがあります。ただし、時々RSAキーはGitHubや他のウェブサイトでSHA-1の問題が原因で動作しないことがあります。この問題に遭遇した場合は、`ed25519`または`ecdsa`を試してみてください。鍵のパスと名前を選択し、チュートリアルに従ってパスワードを設定するかどうかを決定し、次に公開鍵をGitサーバーにアップロードします。GitHubや他の場所にアップロードする方法についての有益な情報は、検索して得ることができます。それではテストしてみましょう：

```
$ ssh -T git@github.com
Hi <your name>! You've successfully authenticated, but GitHub does not provide shell access.
```

これで問題ありません。次にユーザー名とメールアドレスを設定します。

{% hint style="warning" %}
この部分に問題があります！
{% endhint %}

```
$ lg2 config --global user.name "<your name>"
$ lg2 config --global user.email "<your email>"
```

各コマンドが機能しない場合、以下のようにキーとパスワードを毎回入力されないようにするために、次のように追加できます。

```
$ lg2 config --global user.identityFile "~/Documents/.ssh/<private key filename>"
$ lg2 config --global user.password "<your password>"
```

これらのコマンドが機能しない場合、次のように手動でグローバル設定ファイルを作成できます：

```
vim ~/Documents/.gitconfig
```

次に、ファイルの本文に以下を追加します：

```
[user]
       name = <your name>
       email = <your email>
```

設定の可能性と必要な構文についての詳細な情報については、[Git Book](https://git-scm.com/docs/git-config#_configuration_file)を参照してください。ファイルにSSHキーのパスフレーズなどの機密情報を含める場合は、`chmod`を使用して適切にファイルのアクセス権を設定してリスクを制限する必要があります。

### クローンおよびその他の操作

任意のリポジトリを自然にクローンできます：

```
$ lg2 clone https://github.com/holzschu/a-shell.git
```

その後、現在のディレクトリに `a-shell.git` が表示されます。逆に、通常の`git`コマンドを使用した通常のコンピューターでは、ディレクトリは `a-shell` となります。URLの `.git` を削除して目立たなくすることもできます。実際には、`lg2 push origin`を含むすべての基本的なコマンドが正常に動作しますが、コミットグラフの描画など一部は動作しません。バージョン管理の旅をお楽しみください！

### a-ShellはSubversion、CVS、またはその他の代替をサポートしていますか？

いいえ。サポートが必要な場合は、イシューをオープンしてください。
