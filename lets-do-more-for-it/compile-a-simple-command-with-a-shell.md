# a-Shellでシンプルなコマンドをコンパイルする

この記事では、a-Shell独自のツールチェーンを使用して書かれた小さなファイルまたはプロジェクトをコンパイルすることに焦点を当てています。Appleの制約により、これらはネイティブコードではなくWebAssemblyにのみコンパイルできるため、`emacs`や`fish`などを期待しないでください。

### clang、clang++、wasmとの出会い

単一のプログラムファイルをコンパイルから始めましょう。以下は2つの例です：

```c
// test.c
#include<stdio.h>

int main(){
    printf("Hello, world!\n");
    return 0;
}
```

```cpp
// test.cpp
#include<iostream>
using namespace std;

int main(){
    cout << "Hello, world!" << endl;
    return 0;
}
```

コンパイルするには、それぞれ `clang` と `clang++` を使用します。出力ファイルの名前を設定するために `-o` を使用します（これらは `.wasm` で終わってもしなくてもかまいません）。

```bash
$ clang test.c -o testc
$ clang++ test.cpp -o testcpp.wasm
```

次に、コンパイルされたファイルを `wasm` で実行します。`wasm` を呼び出して実行するか、それをバイナリコードのように直接実行できます。

```bash
$ ./testc
Hello, world!
$ wasm testcpp.wasm
Hello, world!
```

時々 `wasm: Error:` というメッセージが表示されることがあります。その場合はすべての開いているウィンドウを閉じてから再試行してみてください。

### makeを使ってみよう

大規模なプロジェクトの場合、すべてのファイルに対してコンパイルコマンドを一行ずつ入力するのは難しい仕事です。通常、`make`を使用して自動的に行います。`make`はプロジェクトのmakefileを探し、それが動作するように指示されたとおりに実行します。ソースコードからプロジェクトをコンパイルしてインストールする有名な方法を知っているかもしれません：

```bash
$ ./configure
$ make
$ make install
```

上記の例では、`./configure`はプラットフォームに応じてmakefileを生成し、`make`はmakefileに従ってプロジェクトをコンパイルし、`make install`はそれをコンピュータにインストールします。ただし、`./configure`は本当にa-ShellとWebAssemblyの違いを知らない場合がありますので、誤った作業をしてしまうことがあります。

それでは、シンプルなプロジェクト `unrar` を見てみましょう。`configure`のようなスクリプトがないほどシンプルなので、まずソースコード全体を取得し、以下は簡略化されたmakefileの一部です：

```makefile
# Linux using GCC
CXX=c++
CXXFLAGS=-O2 -Wno-logical-op-parentheses -Wno-switch -Wno-dangling-else
LIBFLAGS=-fPIC
DEFINES=-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -DRAR_SMP
STRIP=strip
AR=ar
LDFLAGS=-pthread
DESTDIR=/usr
```

```makefile
COMPILE=$(CXX) $(CPPFLAGS) $(CXXFLAGS) $(DEFINES)
LINK=$(CXX)

WHAT=UNRAR

UNRAR_OBJ=filestr.o recvol.o rs.o scantree.o qopen.o
LIB_OBJ=filestr.o scantree.o dll.o qopen.o

OBJECTS=rar.o strlist.o strfn.o pathfn.o smallfn.o global.o file.o filefn.o filcreat.o \
	archive.o arcread.o unicode.o system.o crypt.o crc.o rawread.o encname.o \
	resource.o match.o timefn.o rdwrfn.o consio.o options.o errhnd.o rarvm.o secpassword.o \
	rijndael.o getbits.o sha1.o sha256.o blake2s.o hash.o extinfo.o extract.o volume.o \
	list.o find.o unpack.o headers.o threadpool.o rs16.o cmddata.o ui.o

.cpp.o:
	$(COMPILE) -D$(WHAT) -c $<
```

これを私たちのツールチェーンに合わせるためにmakefileを修正してみましょう。その前に知る必要があることは次のとおりです。

- `CXX` は使用されているコンパイラを意味します。a-Shellの場合は `clang++` になります。
- `CXXFLAGS` および `DEFINES` はコンパイラのオプションを意味します。
- `-O2` は `clang++` と連動する O2 最適化を意味します。
- `-Wno-logical-op-parentheses -Wno-switch -Wno-dangling-else` は不要です。
- `-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -DRAR_SMP` は大きなファイルを処理するための32ビットシステム向けです。64ビットシステムでは無駄でも無害です。
- `/usr` はa-Shellではアクセスできません。実際、 `~/Library` が `/usr` として機能するので、その代わりに使用されます。
- `-fPIC` は動的リンクライブラリ用の位置に依存しないコードを意味します。
- `-pthread` は複数のスレッドを意味し、WebAssemblyではまだ提供されていないため、a-Shellでは動作しません。

次に、makefileを以下のように修正できます：

```makefile
# a-Shell using clang++
CXX=clang++
CXXFLAGS=-O2
LIBFLAGS=-fPIC
DEFINES=-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -DRAR_SMP
STRIP=strip
AR=ar
LDFLAGS=
DESTDIR=~/Library
...
```

{% hint style="info" %}
実際、このプロジェクトは、WASI（WebAssembly System Interface）が提供するAPIの不足により動作しません。a-Shell独自のツールチェーンで動作するソースコードを持つプロジェクトを探しています。もし知っているものがあれば、教えていただけますか。
{% endhint %}
