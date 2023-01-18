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

