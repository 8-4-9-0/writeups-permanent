## Onion (70pt)
> (ものすごい長さの暗号文)

ぱっと見はbase64っぽいので、Cyberchefで復号してみる。"Onion"ということは、玉ねぎの如く何重にもbase64で暗号化されているということだろう。  
そんなこんなで**16回**もの復号を行って、ようやくフラグが......と思いきや、次に示すまた違う形式の暗号？が現れる。
```
begin 666 <data>
51DQ!1U]&94QG4#-3:4%797I74$AU
 
end
```

調べてみると、これはuuencodeと呼ばれる変換方式によって変換された文字列らしい。  
https://tool-taro.com/uudecode/ を用いてuudecodeすると、ついにフラグを得られた。
