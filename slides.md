---
theme: penguin
layout: intro
highlighter: shiki
---

# プログラミング言語 Wasabi の設計と実装

hotaka B1 環境情報学部

---
layout: text-window
---

# WebAssembly

- Webブラウザで高速にプログラムを実行できる仮想命令セット
- バイナリ形式でプログラムを実行する
- 中間表現としてS式を利用できる
- Webブラウザ以外の環境(OS上)でも動作するようになってきている
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

- 使い所
  - Webアプリを Rust などの言語の資産を利用しながら開発したい場合
  - JavaScript 以外で書かれた画像処理などの重い処理を行うライブラリを利用したい場合
- 向かないこと
  - DOM操作を頻繁に行う処理は JavaScript の関数を呼び出す際にオーバーヘッドが生じるため処理が重くなる場合がある

---
---

# JavaScript と WebAssembly の処理速度の比較

- 環境: Brave(Chromium: 109.0.5414.87)
- 以下の2つの処理についてベンチマークを取る
  1. 生成: ランダムな位置と角度を持つParticleを 100,000個生成する
  2. 更新と描画: 生成したパーティクルの位置を更新し `Canvas API` で描画する

---
---

# 生成のベンチマーク

ランダムな位置と角度を持つParticleを 100,000個生成した際の時間

- WebAssembly
  - 12.837890625 ms
- JavaScript
  - 18.35107421875 ms

---
---

# 更新と描画のベンチマーク

更新と描画を100回行い、その処理に掛かった平均時間

- WebAssembly
  - 75.39 ms
- JavaScript
  - 72.16 ms

