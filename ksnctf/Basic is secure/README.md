# Basic is secure? (50pt)
> q8.pcap

与えられたキャプチャをWiresharkで見てみる。Basic認証ということで、GETリクエストに認証情報が含まれているはず。
![](images/img1.png)

成功している二回目のGETリクエストを見てみると、credencialがあり、そこにフラグが記載されていた。
