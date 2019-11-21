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

# redshift (postgresql) で出力したファイルを read_csv して DataFrame に読み込むときのコネタ

`psql < hoge.sql > fuga.txt` のようなやり方で取得したファイルを read_csv する場合、整形めいたことが必要なのでメモ

```
 nantoka_id | kantoka_id |      update_dt      
------------+------------+---------------------
     108985 | 01         | 2019-03-01 04:18:30
     108007 | 01         | 2019-03-01 04:20:16
     917711 | 09         | 2019-03-01 05:21:46
    6907246 | 69         | 2019-03-01 05:30:00
(4 行)
```

このファイルは以下のようにすれば普通に読める。

```
import pandas as pd

# セパレータを　`|`  2行目をスキップ、　とりあえず全部文字列として読み込む。
fuga = pd.read_csv('fuga.txt' , sep='|' , skiprows=[1] ,dtype=str )
# 行数表示をしている最終行を削除
fuga.drop(len(fuga)-1, inplace=True)
# 全部のフィールドの左右の空白文字を除去
fuga = fuga.applymap(lambda x : x.strip())
# 全部のカラム名の左右の空白文字を除去
fuga = fuga.rename(columns=lambda s: s.strip())
```


# もともとのDataFrameに一定間隔でダミーの行を入れるサンプル

列を直接挿入するようなAPIは無いらしい。
サンプルコードは１行ずつ走査していて遅そうだから、100行ずつまとめて処理すると速いかもしれない。

``` python
df = pd.read_csv("/path/to/file.tsv" , sep='\t', header=None)

dummy_record = pd.Series( [0,0,0,0,0,0,0,0,0,0,0]  ) # dummy record

dest = pd.DataFrame(columns=dummy_record.index ,  dtype='float64')

for index,item in df_sorted.iterrows():
    dest = dest.append(item)
    if(index % 500 == 0):
        dest = dest.append(tmp_se,ignore_index=True)
        print(index) ## for logging
        
dest.head()
```
