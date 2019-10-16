### 恵比寿.rb #25
- - -

### Ruby でフィボナッチ数を求めよう！

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 1年半のクソザコナメクジ              <!-- .element: class="fragment" -->
* 趣味で Ruby にパッチを投げたりしてます              <!-- .element: class="fragment" -->
* 最近のトレンド              <!-- .element: class="fragment" -->
  * `binding_of_caller`
  * Refinements <del>バグ直したい</del>広めたい

---

## 今日話すこと

---

### Ruby でフィボナッチ数列を求めよう！

---

#### フィボナッチ数列とは
- - -

* 2つ前の数を合わせると次の数になる数列              <!-- .element: class="fragment" -->
* 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...              <!-- .element: class="fragment" -->
  * 1番目と 2番目は 0, 1 で固定
* この数列のことをフィボナッチ数列と呼ぶ              <!-- .element: class="fragment" -->
  * 数自体はフィボナッチ数と呼ばれる
* プログラミングの演習問題としてよく見かける              <!-- .element: class="fragment" -->

---

## 普通に Ruby で書いてみる

---

#### フィボナッチ数列の実装
- - -

```ruby
def fibonacci(n)
  if n >= 2
    fibonacci(n - 2) + fibonacci(n - 1)
  else
    n
  end
end

pp fibonacci 0   # => 0
pp fibonacci 1   # => 1
pp fibonacci 2   # => 1
pp fibonacci 3   # => 2
pp fibonacci 4   # => 3
pp fibonacci 5   # => 5

pp (1..10).map { |it| fibonacci it }
# => [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

---

# 問題点

---

## パフォーマンスが悪い

---

## どうしようか

---

## そうだ
## 結果をキャッシュしよう

---

#### キャッシュ化
- - -

* fibonacci(n) を呼び出した時にその結果をキャシュしておく
* そうすることで2回目以降はキャッシュした結果を返すだけなので計算が必要なくなる

```
fibonacci(0) => 0
fibonacci(1) => 1
fibonacci(2) => 1
fibonacci(3) => 2
fibonacci(4) => 3
fibonacci(5) => 5
fibonacci(6) => 8
fibonacci(7) => 13
fibonacci(8) => 21
fibonacci(9) => 34
...
```

---

# つまり

---

### こうするのじゃ！

```ruby
def fibonacci1
  1
end

def fibonacci2
  1
end

def fibonacci3
  2
end

def fibonacci4
  3
end

def fibonacci5
  5
end

...
```

---

# 完全解決

---

## 必要なもの
- - -

* send               <!-- .element: class="fragment" -->
* method_missing              <!-- .element: class="fragment" -->
* define_singleton_method              <!-- .element: class="fragment" -->

---

#### 実装
- - -

```ruby
class Fib
  def fibonacci n
    send "fibonacci_#{n}"
  end

  def method_missing name, *args
    return super if name !~ /fibonacci_(\d+)/
    num = $1.to_i
    if num >= 2
      num = fibonacci(num - 2) + fibonacci(num - 1)
    end
    define_singleton_method(name) { num } && num
  end
end

fib = Fib.new
p fib.fibonacci(0)  # => 0
p fib.fibonacci(1)  # => 1
p fib.fibonacci(2)  # => 1
p fib.fibonacci(3)  # => 2
p fib.fibonacci(4)  # => 3
```

---

### まとめ
- - -

* メタプログラミング楽しー        <!-- .element: class="fragment" -->


---

# 宣伝1

---

#### [Ruby Hack Challenge Holiday](https://rhc.connpass.com/event/151557/)
- - -

* Ruby 本体を自分でビルドしたりハックしたりするイベント           <!-- .element: class="fragment" -->
* Ruby のコミッタの方が主催しているので Ruby について聞けるチャンス       <!-- .element: class="fragment" -->
* steep を開発して @soutaro さんをお呼びして「Ruby の型をどう書くのか」「実際に組込クラスの型をつけて貢献する方法」をレクチャーがある        <!-- .element: class="fragment" -->
* 次回は 11/10(日) 開催予定          <!-- .element: class="fragment" -->

---

# 宣伝2

---

#### [関数型プログラミングカンファレンス](https://fpc2019japan-event.peatix.com/)
- - -

* 関数型プログラミングカンファレンスというイベントをやります          <!-- .element: class="fragment" -->
* Haskell や GHC の開発者の方や Rust のコミッタ、Elixir のフルスタックエンジニアと豪華な講演者          <!-- .element: class="fragment" -->
* 気になる方は募集ページを見て募集してね！          <!-- .element: class="fragment" -->
* あとスポンサーしてくださる企業も募集しています！          <!-- .element: class="fragment" -->


---


## ご清聴
## ありがとうございました
