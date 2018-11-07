# Hello, React Hooks!

[Hooks](https://reactjs.org/docs/hooks-intro.html)について紹介します。

Hooksとは、Classコンポーネントの力を借りずにFunction Componentでstateを利用できるようにする機能のことです。

2018年11月時点ではまだ[RFC](https://github.com/reactjs/rfcs/blob/hooks-rfc/text/0000-react-hooks.md)で議論中の機能になります。

このHooksを実際に触ってみたのでその手順を以下に記載します。

扱うソースコードは公式のマニュアルをそのままパクりました。

まずは、Reactアプリケーションを用意します。お約束のcreate-react-appでさくっと作ります。

```bash
$ npx create-react-app demo-hooks-intro
$ cd demo-hooks-intro
$ yarn start
```

ブラウザが起動して、`http://localhost:3000/` にアクセスします。

Reactのお約束の画面が表示されれば正常にReactアプリケーションが作成できたことになります。

ここで、`^C`(control プラス c)でサーバを一旦止めます。

次に、アプリケーションにインストールされているReactのバージョンを確認します。

2018年11月08日の時点では、以下のように`16.6.1`となっていました。

```bash
$ grep ^react@ yarn.lock
react@16.6.1:
```

このバージョンでは、Hooksは利用できないのでここでバージョンを上げておきましょう。

で、どのバージョンに上げるのかというと、一番新しいバージョンにあげておけば間違いないと思いますので、一度、バージョン一覧を確認しておきます。
(公式サイトには、`React v16.7.0-alpha`とありましたが、今後のために、多分これ以降ならなんでも良いでしょうということにします。)

使用するのは、[`yarn info`](https://yarnpkg.com/lang/ja/docs/cli/info/)コマンドです。

```bash
$ yarn info react
```

はい、色々出てきましたね。

バージョン情報だけに絞りたいときは、versionsを更に追加すると良いです。

```bash
$ yarn info react versions
```

と、入力して再度実行してみましょう。

```
  [
   .
   .
   .
   .
  '16.6.0-alpha.f47a958',
  '16.6.0',
  '16.6.1',
  '16.7.0-alpha.0' ]
```

すると、バージョンの一覧だけが出力されました。この一覧は昇順にソートされているので一番下のバージョンが最新のバージョンということになります。

僕が実行した時点では、'16.7.0-alpha.0'というのが最新なのでこれをインストールすることにします。

```bash
$ yarn add react@16.7.0-alpha.0
yarn add v1.12.1
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
success Saved lockfile.
success Saved 2 new dependencies.
info Direct dependencies
├─ react-dom@16.6.1
└─ react@16.7.0-alpha.0
info All dependencies
├─ react-dom@16.6.1
└─ react@16.7.0-alpha.0
✨  Done in 15.42s.
```

では次に、`yarn add`で先程確認した最新のreactをインストールします。

`yarn add react`と入力して`@`を入力してバージョンを指定します。

エンターキーを押下します。

さくっと入りましたね。

`git diff`コマンドで差分を見てみましょう。

```diff
diff --git a/package.json b/package.json
index 486f492..10313fc 100644
--- a/package.json
+++ b/package.json
@@ -3,7 +3,7 @@
   "version": "0.1.0",
   "private": true,
   "dependencies": {
-    "react": "^16.6.1",
+    "react": "16.7.0-alpha.0",
     "react-dom": "^16.6.1",
     "react-scripts": "2.1.1"
   },
diff --git a/yarn.lock b/yarn.lock
index 5916afc..78a085a 100644
--- a/yarn.lock
+++ b/yarn.lock
@@ -6801,9 +6801,10 @@ react-dev-utils@^6.1.1:
     strip-ansi "4.0.0"
     text-table "0.2.0"

-react-dom@16.6.1:
+react-dom@^16.6.1:
   version "16.6.1"
   resolved "https://registry.yarnpkg.com/react-dom/-/react-dom-16.6.1.tgz#5476e08694ae504ae385a28b62ecc4fe3a29add9"
+  integrity sha512-zm+wBuEMGm009Wt1uE4Zw5KcXOW7qC4E/xW/fpJsCsbOco4U/R84u+DzzO/S4SYSdNBcqcaulcp4w3FXl8pImw==
   dependencies:
     loose-envify "^1.1.0"
     object-assign "^4.1.1"
@@ -6868,14 +6869,15 @@ react-scripts@2.1.1:
   optionalDependencies:
     fsevents "1.2.4"

-react@16.6.1:
-  version "16.6.1"
-  resolved "https://registry.yarnpkg.com/react/-/react-16.6.1.tgz#ee2aef4f0a09e494594882029821049772f915fe"
+react@16.7.0-alpha.0:
+  version "16.7.0-alpha.0"
+  resolved "https://registry.yarnpkg.com/react/-/react-16.7.0-alpha.0.tgz#e2ed4abe6f268c9b092a1d1e572953684d1783a9"
+  integrity sha512-V0za4H01aoAF0SdzahHepvfvzTQ1xxkgMX4z8uKzn+wzZAlVk0IVpleqyxZWluqmdftNedj6fIIZRO/rVYVFvQ==
   dependencies:
     loose-envify "^1.1.0"
     object-assign "^4.1.1"
     prop-types "^15.6.2"
-    scheduler "^0.11.0"
+    scheduler "^0.11.0-alpha.0"

 read-pkg-up@^1.0.1:
   version "1.0.1"
@@ -7268,7 +7270,7 @@ saxes@^3.1.3:
   dependencies:
     xmlchars "^1.3.1"

-scheduler@^0.11.0:
+scheduler@^0.11.0, scheduler@^0.11.0-alpha.0:
   version "0.11.0"
   resolved "https://registry.yarnpkg.com/scheduler/-/scheduler-0.11.0.tgz#def1f1bfa6550cc57981a87106e65e8aea41a6b5"
   dependencies:
```

diffを見てみると、既存のバージョンが最新のバージョンにリプレースされていることがわかります。バージョンアップに成功しました。

続いて、react-domも更新しておきましょう。

reactと同じように`yarn add`でインストールします。

```bash
$ yarn add react-dom@16.7.0-alpha.0
yarn add v1.12.1
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency.
info Direct dependencies
└─ react-dom@16.7.0-alpha.0
info All dependencies
└─ react-dom@16.7.0-alpha.0
✨  Done in 6.39s.
```

はい、`react-dom`もサクッと入りましたね。

```diff
diff --git a/package.json b/package.json
index 10313fc..3704908 100644
--- a/package.json
+++ b/package.json
@@ -4,7 +4,7 @@
   "private": true,
   "dependencies": {
     "react": "16.7.0-alpha.0",
-    "react-dom": "^16.6.1",
+    "react-dom": "16.7.0-alpha.0",
     "react-scripts": "2.1.1"
   },
   "scripts": {
diff --git a/yarn.lock b/yarn.lock
index 78a085a..5fbf6b3 100644
--- a/yarn.lock
+++ b/yarn.lock
@@ -6801,15 +6801,15 @@ react-dev-utils@^6.1.1:
     strip-ansi "4.0.0"
     text-table "0.2.0"

-react-dom@^16.6.1:
-  version "16.6.1"
-  resolved "https://registry.yarnpkg.com/react-dom/-/react-dom-16.6.1.tgz#5476e08694ae504ae385a28b62ecc4fe3a29add9"
-  integrity sha512-zm+wBuEMGm009Wt1uE4Zw5KcXOW7qC4E/xW/fpJsCsbOco4U/R84u+DzzO/S4SYSdNBcqcaulcp4w3FXl8pImw==
+react-dom@16.7.0-alpha.0:
+  version "16.7.0-alpha.0"
+  resolved "https://registry.yarnpkg.com/react-dom/-/react-dom-16.7.0-alpha.0.tgz#8379158d4c76d63c989f325f45dfa5762582584f"
+  integrity sha512-/XUn1ldxmoV2B7ov0rWT5LMZaaHMlF9GGLkUsmPRxmWTJwRDOuAPXidSaSlmR/VOhDSI1s+v3+KzFqhhDFJxYA==
   dependencies:
     loose-envify "^1.1.0"
     object-assign "^4.1.1"
     prop-types "^15.6.2"
-    scheduler "^0.11.0"
+    scheduler "^0.11.0-alpha.0"

 react-error-overlay@^5.1.0:
   version "5.1.0"
@@ -7270,7 +7270,7 @@ saxes@^3.1.3:
   dependencies:
     xmlchars "^1.3.1"

-scheduler@^0.11.0, scheduler@^0.11.0-alpha.0:
+scheduler@^0.11.0-alpha.0:
   version "0.11.0"
   resolved "https://registry.yarnpkg.com/scheduler/-/scheduler-0.11.0.tgz#def1f1bfa6550cc57981a87106e65e8aea41a6b5"
   dependencies:
```

react-domの方もdiffを見ておきます。

確かに、既存のバージョンから最新に切り替わっていることが確認できました。

これで、React Hooksを使うための準備ができているはずです。

では、次に`yarn start`を実行してアプリケーションからReactのバージョンを確認しておきましょう。

```diff
diff --git a/src/App.js b/src/App.js
index 7e261ca..d9b0a4c 100644
--- a/src/App.js
+++ b/src/App.js
@@ -2,6 +2,8 @@ import React, { Component } from 'react';
 import logo from './logo.svg';
 import './App.css';

+console.log(React.version)
+
 class App extends Component {
   render() {
     return (
```

`src/App.js`を開いて、import文の直後に`console.log(React.version)`を追記します。

そして、ブラウザのログを見ると、、、

```
16.7.0-alpha.0
```

と表示されています。先程インストールしたバージョンです。

これで、React [Hooks](https://reactjs.org/docs/hooks-intro.html)を利用するための準備ができました。

では、こちらの公式のReactの[ページ](https://reactjs.org/docs/hooks-intro.html)の

「Introducing Hooks」で紹介されているコードをコピペして、src/App.jsに貼り付けましょう。

まずは、src/App.jsの内容を全て削除します。

そして、Introducing Hooksで紹介されているコードをコピーしてsrc/App.jsに貼り付けて保存します。

```diff
diff --git a/src/App.js b/src/App.js
index e69de29..98cab75 100644
--- a/src/App.js
+++ b/src/App.js
@@ -0,0 +1,15 @@
+import { useState } from 'react';
+
+function Example() {
+  // Declare a new state variable, which we'll call "count"
+  const [count, setCount] = useState(0);
+
+  return (
+    <div>
+      <p>You clicked {count} times</p>
+      <button onClick={() => setCount(count + 1)}>
+        Click me
+      </button>
+    </div>
+  );
+}
```

すると、ブラウザに下記の様なerrorが表示されました。

```
index.js:1452 ./src/index.js
Attempted import error: './App' does not contain a default export (imported as 'App').
function.console.(anonymous function) @ index.js:1452
handleErrors @ webpackHotDevClient.js:156
push../node_modules/react-dev-utils/webpackHotDevClient.js.connection.onmessage @ webpackHotDevClient.js:193
push../node_modules/sockjs-client/lib/event/eventtarget.js.EventTarget.dispatchEvent @ eventtarget.js:56
(anonymous) @ main.js:282
push../node_modules/sockjs-client/lib/main.js.SockJS._transportMessage @ main.js:280
push../node_modules/sockjs-client/lib/event/emitter.js.EventEmitter.emit @ emitter.js:53
WebSocketTransport.ws.onmessage @ websocket.js:36
index.js:1452 ./src/App.js
  Line 8:   'React' must be in scope when using JSX  react/react-in-jsx-scope
  Line 9:   'React' must be in scope when using JSX  react/react-in-jsx-scope
  Line 10:  'React' must be in scope when using JSX  react/react-in-jsx-scope

Search for the keywords to learn more about each error.
```

1つ目のerrorはexport文が無いことで発生しているものです。

2つ目のerrorはReactのimportが無いことで発生しているものです。

それぞれの問題を解消していきましょう。

```diff
diff --git a/src/App.js b/src/App.js
index 98cab75..e788358 100644
--- a/src/App.js
+++ b/src/App.js
@@ -1,4 +1,4 @@
-import { useState } from 'react';
+import React, { useState } from 'react';

 function Example() {
   // Declare a new state variable, which we'll call "count"
@@ -13,3 +13,5 @@ function Example() {
     </div>
   );
 }
+
+export default Example
```

errorを解消するには、上記のように修正します。

Reactの追加と、export defaultの追加をします。

すると、先程表示されていたerrorは全て消えました。

また、画面にも

`You clicked 0 times`という文と、`Click me`と書かれたボタンが表示されました。

せっかくなので`Click me`のボタンをクリックしてみます。

クリックすると、

`You clicked 0 times`が `You clicked 1 times`に変更されました。

1 は単数だから timesじゃなくてtimeじゃないと！

というつっこみはおいておいて

Click meボタンのonClickによるイベントハンドラによってsetCountという関数がcountという状態を更新していることが画面から確認することができました。


```javascript
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

export default Example
```

全体のコードを解説すると、 まず、従来のFunction Componentでは状態を持つことができませんでしたが、 今回導入されたHooksによって状態の保持が可能となりました。

Hooksを利用するには、冒頭で、状態を扱うための関数`useState`をimport します。

関数の中で、useStateを実行して、状態と状態に変更を加える関数を用意します。

useStateには引数に初期値を与えます。

今回の例では0でカウンターの値を初期化したいので、0を引数に与えています。

これはカウンターの初期値を100から開始したい場合はuseStateの引数に100を与えれば良いということになります。

そして、useStateは配列を返します。

1つ目の要素に状態、2つ目の要素に関数を返します。

そして、これらの状態と関数を使ってよしなにイベントハンドラのコールバック関数にロジックを実装します。

今回の例ではonClickイベントで状態を+1する処理を登録しています。

以上がHooksの紹介です。

簡単ですが、Hooksの雰囲気は掴めたかと思います。

