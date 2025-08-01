## John (60pt)
> user00:$6$Z4xEy/1KTCW.rz$Yxkc8XkscDusGWKan621H4eaPRjHc1bkXDjyFtcTtgxzlxvuPiE1rnqdQVO1lYgNOzg72FU95RQut93JF6Deo/:15491:0:99999:7:::  
> user01:$6$ffl1bXDBqKUiD$PoXP69PaxTTX.cgzYS6Tlj7UBvstr6JruGctoObFXCr4cYXjIbxBSMiQZiVkKvUxXUC23zP8PUyXjq6qEq63u1:15491:0:99999:7:::  
> ...  
> user99:$6$SHA512IsStrong$DictionaryIsHere.http//ksnctf.sweetduet.info/q/14/dicti0nary_8Th64ikELWEsZFrf.txt:15491:0:99999:7:::

`/etc/passwd`に似ているフォーマットだが、少し違う。調べてみると、`/etc/shadow`というrootユーザのみが閲覧できるファイルのフォーマットで、Unixではここにのみパスワードを格納するようにしているらしい。（`/etc/passwd`はシステムのユーザなら誰でも閲覧できるため、パスワードが読み取れてしまうことへの対策）  
パスワードはSHA-512アルゴリズムでハッシュ化されており、本来なら元に戻すのはほぼ不可能なのだが、`DictionaryIsHere`ということで、ご丁寧にも辞書が用意されている。後はクラックするだけだ。ツールは定番のhashcat。ハッシュタイプは1800番がこのフォーマット用のものなのでそれを使う。
```
$ hashcat -m 1800 -a 0 hash.txt dic.txt
```

(`hash.txt`は`user99`を除く問題文のコピペ、`dic.txt`は`dicti0nary_8Th64ikELWEsZFrf.txt`のリネーム)  
上のコマンドを実行すると、私の環境では30秒足らずでクラックが終了した。終わったら`--show`で結果をチェック。  
結果はここには載せないが、解析されたパスワードの頭文字を並べるとフラグになっている。適当にPythonスクリプトを書いたりして抜き出してくると良いだろう。