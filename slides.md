---
theme: penguin
layout: intro
highlighter: shiki
---

# プログラミング言語 Wasabi の設計と実装

hotaka B1 環境情報学部

---

# Index

→ もっと Wasabi言語自体をフォーカスした方が良い(ハマリポイントや設計、コンパイル時間、JavaScriptとの比較)
→ 今のWasabi言語でできる範囲内でベンチマークを取るのもあり

- 背景
  - 目的、動機
- WebAssembly について
- Rust で出力した WebAssembly と JavaScript のベンチマーク
- Wasabi 言語について
- Wasabi 言語のベンチマークと考察
- まとめ

---
layout: text-window
---

# WebAssembly

- Webブラウザで高速にプログラムを実行できる仮想命令セット
- バイナリ形式でプログラムを実行する
- 中間表現としてS式を利用できる
  - → スタックマシン
- Webブラウザ以外の環境(OS上)でも動作するようになってきている
  - → どのブラウザ/プラットフォームで動作するかを追加
- C言語やRustからコンパイルして利用する
- DOM操作やブラウザのAPIを操作する際は JavaScript で定義した関数を呼び出して行う

::window::

```
;; スタックマシンで処理される

i32.const 1
i32.const 2
i32.add
;; 3

;; JavaScript の関数を呼び出す

(import "js" "print" (func $print (param i32)))
i32.const 10
call $print
```

---
---

# WebAssembly のユースケース

→ (前提をはっきり書く)

- 使い所
  - Webアプリを Rust などの言語の資産を利用しながら開発したい場合
  - JavaScript 以外で書かれた画像処理などの重い処理を行うライブラリを利用したい場合
- 向かないこと
  - DOM操作を頻繁に行う処理は JavaScript の関数を呼び出す際にオーバーヘッドが生じるため処理が重くなる場合がある

---
---

# JavaScript と WebAssembly の処理速度の比較

- 環境: Brave(Chromium: 109.0.5414.87)
- WebAssembly は Rust から出力
- 以下の2つの処理についてベンチマークを取る
  1. JavaScript(V8) と WebAssembly(Rust) の: ランダムな位置と角度を持つParticleを 100,000個生成する
  2. API呼び出しを伴う処理能力: 生成したパーティクルの位置を更新し `Canvas API` で描画する

---
---

# 生成のベンチマーク

ランダムな位置と角度を持つParticleを 100,000個生成した際の時間
→ 小数点2桁で大丈夫

- WebAssembly(Rust)
  - 12.837890625 ms
- JavaScript
  - 18.35107421875 ms

---
---

# 更新と描画のベンチマーク

更新と描画を100回行い、その処理に掛かった平均時間
→ 小数点2桁で大丈夫

- WebAssembly(Rust)
  - 75.39 ms
- JavaScript
  - 72.16 ms

---

