# 文字列のフォーマットのやり方例


``` python
a = "Hello"
b = "World"
print(f"{a} {b}")
print(f"{a + ' python ' + b}")
```

``` console
Hello World
Hello python World
```


文字列の先頭に `f` をつけて、中括弧で変数名をくくる。中括弧の中に文字列リテラルを組み込めるらしい。
