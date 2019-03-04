# DataFrame での加工ネタ

もとのカラムの値を加工した値で新カラムを追加したい。例えば、 `先頭の１文字除去しfloatに変換する加工` の例

``` python
newDataFrame = someDataFrame.assign(new_column = someDataFrame['target_column'].str[1:].astype(np.float))
```

`str` というメソッド経由で、`Series`の要素の文字列にアクセスできる。

同様に、
`dt` というメソッド経由で、`Series`の要素の日付型のデータにアクセスできる。

例：日付の感覚が１日以下の行を取り出す。

deltatimeオブジェクト同士を引き算することで、timedeltaオブジェクトが得られていて、その `days`属性を取得している。

``` python
someDataFrame[(someDataFrame['update_dt'] - someDataFrame['entry_dt']).dt.days  < 1 ]
```

参考： https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.html

