# DataFrame での加工ネタ

もとのカラムの値を加工した値で新カラムを追加したい。例えば、 `先頭の１文字除去しfloatに変換する加工` の例

``` python
newDataFrame = someDataFrame.assign(new_column = someDataFrame['target_column'].str[1:].astype(np.float))
```

`str` というメソッド経由で、`Series`の要素の文字列にアクセスできる。


参考： https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.html

