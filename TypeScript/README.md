# TypeScript メモ

## 公式ドキュメント
https://www.typescriptlang.org/docs/handbook/

## TypeScript - Qiita
https://qiita.com/tags/typescript

## TypeScript - Zenn
https://zenn.dev/topics/typescript

<br>
<br>

## TypeScriptの特徴
* JavaScriptの上位互換
* 安全性が高い
* 型チェックが強力で柔軟
* 新しい文法を使っても古い環境で動作させることができる

<br>

## 型注釈と型推論
型注釈... 変数等を宣言するときにどんな値がそこに入るのか、型を指定する機能
<br>
型推論... コンパイラが型を自動で判別する機能

<br>

## 型推論と動的型付けの違い
型推論はコンパイルのタイミングで型が決定され、その型が変更されることはない。
<br>
一方、動的型付けでは実行時に型が決まるため、実行タイミングにより型が変化する。
<br>

実行タイミングで型が変化するため、型推論ではエラーになる処理も動的型付け言語では正常に動作することがある。

```typescript
// TypeScriptの例
let x = 1;    // xはnumber型となる(型推論)

x = "hello";  // xはnumber型と決定しているのでstring型を代入するとエラー

console.log(x.substring(1, 3));
```

```javascript
// JavaScriptの例
let x = 1;    // xはnumber型となる

x = "hello";  // xはstring型となる

console.log(x.substring(1, 3));

// 結果: "el"
```