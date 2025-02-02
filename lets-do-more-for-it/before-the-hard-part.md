# 難しい部分の前に

このガイドブックのこの章を読んでいるすべての人は、a-Shellが既に持っている機能に不満を持っているはずであり、この章ではこのアプリができるさまざまな可能性について話します。

この道は大きな挑戦でいっぱいです。混乱したエラーに対処するために何度も概念を学んだり、インターネットで検索したりすることがあります。もし今準備ができているなら、ハッキングの旅を始めましょう！

### プログラムはどのように動作するのか？

これはこのトピックの中で最も重要な問題の1つです。a-Shellで動作するすべてのプログラムは、次の2つに分類されます。

* ネイティブのバイナリコード、`ios` および `arm64` 用。これらは通常のアプリが動作するのと同じように動作し、理論的には何でもできます。これらの種類のコードをコンパイルするには、XcodeがインストールされたMacコンピューターが必要であり、配布するには開発者アカウントでアプリを再署名する必要があります。更新はApp Storeにプッシュする必要があり、すべてのユーザーは新しい機能を追加したり、削除したりする能力はありません。
* WebAssemblyコード、`wasm` と呼ばれるもの。これらはより単純な作業を行うことができ、任意のコンピューターでコンパイルできます（a-Shell自体でも可能）。ユーザーが追加または削除でき、`wasm` パッケージを管理するための簡単なツール `pkg` が提供されています。

プロジェクトをネイティブコードにコンパイルするか、WebAssemblyコードにコンパイルするかは、プロジェクトが複雑かどうか、およびそれが大多数のユーザーに歓迎されるかどうかによります。

### この章では何が話される予定ですか？

この部分は進行中であり、最終的に確定していません。

* a-Shell独自のツールチェーンを使用してC/C++プロジェクトをWebAssemblyファイルにコンパイルする方法
* 実際のコンピューター環境のツールチェーンを使用してプロジェクトをWebAssemblyファイルにクロスコンパイルする方法（`make`、`cmake`、またはその他のツールを使用）
* a-Shellの拡張リポジトリに新しいパッケージを提出する方法
* a-Shell自体に新しいコマンドを追加し、プロジェクトをコンパイルする方法
* GoとWebAssembly
* Rustの可能性
* Node.js? JSCompiler?
* いくつかの面白いアイデア

