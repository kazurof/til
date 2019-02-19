
# chownでグループとユーザを一括で変更する。

以下のように名前の後ろに `:` をつければOK.

```
$ chown nantokakun: kantoka-data/
```

参考

ユーザー名に続いてコロンもしくはドットがあるのにグループ名が無い場合、ファイルのグループはそのユーザのプライマリグループに変更されます。

https://kazmax.zpp.jp/linux_beginner/chown.html


