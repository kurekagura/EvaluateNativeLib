# Entropia File System Watcher(efsw)に関するノート

Linux/macOSにも対応したファイルシステムウォッチャーのライブラリです。Linux版はinotifyが利用されています。今回の対象はWindows版です。

## 調査内容

.NETのFileSystemWatcherが「（ファイル・フォルダの）移動」をdeleteとcreateの別イベントでしか検知できません。このライブラリではその点でどうなのか？という事を実験しました。

【結果】
プラットフォーム固有のWatcherとGenericのWatcherがあるようなので期待しましたが、残念ながら同じでした。付属の「efsw-test」で useGeneric=false/true のどちらで試しても同じ結果でした。

```console
Press ^C to exit demo
Watching directory: O:\src\SpartanJ\efsw\bin\test
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\) FILE (file1.txt) has event Delete
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\dir1\) FILE (file1.txt) has event Add
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\) FILE (dir1) has event Modified
```

```console
Press ^C to exit demo
Watching directory: O:\src\SpartanJ\efsw\bin\test
Using generic backend watcher
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\) FILE (file1.txt) has event Delete
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\) FILE (dir1) has event Modified
Watch ID 1 DIR (O:\src\SpartanJ\efsw\bin\test\dir1\) FILE (file1.txt) has event Add
```

【補足】

この仕様はドキュメントの記載されている下記点と思われます。

https://github.com/SpartanJ/efsw

>Generic watcher relies on the inode information to detect file and directories renames/move. Since Windows has no concept of inodes as Unix platforms do, there is no current reliable way of determining file/directory movement on Windows without help from the Windows API ( this is replaced with Add/Delete events ).
