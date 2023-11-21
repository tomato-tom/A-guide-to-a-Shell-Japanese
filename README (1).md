# 初心者向けガイド

おそらく、App Store で `a-Shell` を見つけたことでしょう。`a-Shell` はiOS/iPadOS用のターミナルエミュレータで、`python.rich` モジュールのインポートから `vim` プラグインの管理まで、さまざまなUnixコマンドを実行できます。`ffmpeg`、`python`、`lua`、`tex`、`perl`、`clang`、`wasm`、`jsc`などを使用でき、`vim`またはnano風の`pico`を使用してテキストを編集できます。さらに、`JupyterLab` を実行することもできます。また、将来的には `code-server` も調査されるかもしれません。

### できること

#### 基本的なコマンドとネットワークコマンド

予想されるように、もちろん `ls`、`cd`、`cp`などの基本的なコマンドが使用できます。多くの重要なネットワークコマンドも提供されています。

```sh
$ ping google.com -c 4
PING google.com (198.18.0.85): 56 data bytes
64 bytes from 198.18.0.85: icmp_seq=0 ttl=64 time=0.406 ms
64 bytes from 198.18.0.85: icmp_seq=1 ttl=64 time=0.454 ms
64 bytes from 198.18.0.85: icmp_seq=2 ttl=64 time=0.532 ms
64 bytes from 198.18.0.85: icmp_seq=3 ttl=64 time=0.396 ms
--- google.com ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.396/0.447/0.532/0.054 ms

$ nslookup
> apple.com
Server:         198.18.0.2
Address:        198.18.0.2#53
Non-authoritative answer:
Name:   apple.com
Address: 198.18.0.37
```

`man` コマンドも提供されているため、基本的なコマンドのマニュアルを簡単に読むことができます。

<figure><img src=".gitbook/assets/68F0411E-EB70-4EEA-8E82-3119770F7787.jpeg" alt=""><figcaption><p>make のマニュアル</p></figcaption></figure>

#### パッケージを追加

`pkg` と呼ばれるツールを使用して、いくつかの追加コマンドをインストールできます。次のようにして `pkg install` を使用してさらなるコマンドを取得できます：

```sh
$ pkg install zip
```

`pkg list` を使用してすでにインストールされているすべてのパッケージを一覧表示し、`pkg search <パッケージ名>` を使用してパッケージが利用可能かどうかを検索できます。すべての利用可能なパッケージを表示するには、`pkg search` を使用します。パッケージを削除するには、`pkg remove <パッケージ名>` を使用します。

変数 `$PKG_SERVER` はパッケージを取得するアドレスを定義します。変数が設定されていない場合、デフォルトのリポジトリ [https://github.c\
om/holzschu/a-Shell-commands](https://github.com/holzschu/a-Shell-commands) が使用されます。変数を設定して使用するリポジトリを指定できます：

```sh
$ export PKG_SERVER=https://github.com/holzschu/a-Shell-commands 
```

パッケージを取得または検索できない場合は、`$PKG_SERVER` に何か問題があるかもしれません。デフォルトのリポジトリに切り替えるには、それをアンセットしてみてください：

```sh
$ unsetenv PKG_SERVER
```

#### テキストファイルの編集

これまでに3つのテキストエディタが提供されています: `vim`、`pico`、および `ed`。

Vim ユーザーは、Vim プラグインがうまく動作することに喜ぶかもしれませんが、`vim-plug` のようなプラグインマネージャには多くの問題があります。そのため、Vim 8 の組み込みパッケージマネージャを使用することが推奨されています。詳細については、[configure-your-vim.md](basic-tutorials/configure-your-vim.md "mention") を参照してください。

<figure><img src=".gitbook/assets/89BA884C-9395-4E53-9284-97E69E3CE2A9.jpeg" alt=""><figcaption><p>Vim インターフェース</p></figcaption></figure>

Vim に慣れておらず、よりシンプルなテキストエディタをお探しの場合は、`pico` が適しています。GPLの下で提供されているGNU Nanoは、FSFのポリシーによりa-Shellに含めることができません。そのため、同様のエクスペリエンスを提供するために `pico` が含まれています。

<figure><img src=".gitbook/assets/D884DB64-276A-46D6-8ED6-789FBD167C1C.jpeg" alt=""><figcaption><p>Pico インターフェース</p></figcaption></figure>

また、おもちゃのような `ed` も含まれています。`ed` は行エディタで、行ごとに編集コマンドを入力できます。以下の例では、`r`、`,p`、`1`、`2`、`3`、`4`、および `q` は `ed` 内のコマンドであり、他のものは `ed` によって出力されたものです。

```
$ ed
r test.cpp
131
,p
#include<iostream>
using namespace std;
int main(){
        cout << "Hello, world!" << endl;
        int a;
        cin >> a;
        cout << a;
        return 0;
}
1
#include<iostream>
2
using namespace std;
3
4
int main(){
q
$
```

#### リモート SSH/SFTP

SSH接続が利用可能です。`ssh`、`scp`、`sftp` をこれまで通りに使用してください。SSHキーを生成するには `ssh-keygen` を使用します。`mosh` と `sshd` はまだサポートされていません。

#### Python 3

Pythonを簡単に実行できます。

```sh
$ python
>>> print("Hello, world!")
Hello, world!
```

`pip`を使用してモジュールをインストールすることもできます。現時点では、`clang`はPythonモジュールを適切に処理できないため、それらは純粋なPythonで記述する必要があります。

```sh
$ pip install requests
Defaulting to user installation because normal site-packages is not writeable
Collecting selenium
  Downloading selenium-4.8.3-py3-none-any.whl (6.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.5/6.5 MB 2.9 MB/s eta 0:00:00
Requirement already satisfied: urllib3[socks]~=1.26 in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from selenium) (1.26.13)
Collecting trio~=0.17
  Downloading trio-0.22.0-py3-none-any.whl (384 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 384.9/384.9 kB 4.8 MB/s eta 0:00:00
Collecting trio-websocket~=0.9
  Downloading trio_websocket-0.10.2-py3-none-any.whl (17 kB)
Requirement already satisfied: certifi>=2021.10.8 in /private/var/containers/Bundle/Application/C3889491-0
CAD-4C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from selenium) (2022.9.24)
Requirement already satisfied: attrs>=19.2.0 in /private/var/containers/Bundle/Application/C3889491-0CAD-4
C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (22.1.0
)
Collecting sortedcontainers
  Downloading sortedcontainers-2.4.0-py2.py3-none-any.whl (29 kB)
Collecting async-generator>=1.9
  Downloading async_generator-1.10-py3-none-any.whl (18 kB)
Requirement already satisfied: idna in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B01-
39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (3.4)
Collecting outcome
  Downloading outcome-1.2.0-py2.py3-none-any.whl (9.7 kB)
Requirement already satisfied: sniffio in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B
01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (1.3.0)
Collecting exceptiongroup
  Downloading exceptiongroup-1.1.1-py3-none-any.whl (14 kB)
Collecting wsproto>=0.14
  Downloading wsproto-1.2.0-py3-none-any.whl (24 kB)
Collecting PySocks!=1.5.7,<2.0,>=1.5.6
  Downloading PySocks-1.7.1-py3-none-any.whl (16 kB)
Collecting h11<1,>=0.9.0
  Downloading h11-0.14.0-py3-none-any.whl (58 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 58.3/58.3 kB 2.2 MB/s eta 0:00:00
Installing collected packages: sortedcontainers, PySocks, outcome, h11, exceptiongroup, async-generator, w
sproto, trio, trio-websocket, selenium
Successfully installed PySocks-1.7.1 async-generator-1.10 exceptiongroup-1.1.1 h11-0.14.0 outcome-1.2.0 se
lenium-4.8.3 sortedcontainers-2.4.0 trio-0.22.0 trio-websocket-0.10.2 wsproto-1.2.0
```

#### Lua and Perl

他のスクリプト言語であるLuaも機能します。

```sh
$ lua
Lua 5.4.4  Copyright (C) 1994-2022 Lua.org, PUC-Rio
> print("Hello, world!")
Hello, world!
```

Perlも機能します。

```sh
$ perl test.pl
Hello, world!
```

#### JavaScript

WebKitのJS環境が含まれています。通常のJavaScriptコードを実行するには`jsc`を使用できます。

```sh
$ echo 'console.log("Hello, world!");' > test.js
$ jsc test.js
Hello, world!
```

`node.js`を含めるのは難しいことです。`jsc`で実行できるようにする可能性は存在しますが、現時点では誰も試みていません。この問題については、章「さらに進んでみましょう」で話します。

#### C/C++ and WebAssembly

WebAssemblyのおかげで、C/C++で書かれたプロジェクトをコンパイルすることができます。`clang`を使用してCコードをWebAssemblyにコンパイルし、`wasm`を使用してそれらを簡単に実行できます。実際、`pkg`でインストールされたほとんどのコマンドはWebAssemblyで配布されています。
