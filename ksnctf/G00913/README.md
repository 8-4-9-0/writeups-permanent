## G00913 (30pt)
> FLAG_Q20_{first 10-digit prime found in consecutive digits of π}

first 10-digit prime found in consecutive digits of π、すなわち円周率内で最初に見つかる10桁の素数を見つけろという問題。  
最初はPythonで解くことを考え、`math.pi`を使ったら良いのかなと思ったが、https://pyex.srbrnote.work/library/math/math.pi.html によると全然正確じゃないらしい。なので円周率を載せてる適当なサイトからとりあえず100桁持ってきて、次に示すソルバを書いた。https://qiita.com/ppza53893/items/e0f464340d6f97760cd5 を大いに参考にしている。
```python
def trial_odd_sqrt(n: int):
    assert n > 1
    if n == 2:
        return True
    elif n % 2 == 0:
        return False
    i = 3
    while i <= n**0.5:
        if n%i == 0:
            return False
        i+=2
    return True

p = (円周率100桁)
nums = str(p).replace('.', '')
for i in range(digits-8):
    num = int(nums[i:i+10])
    if trial_odd_sqrt(num) == True:
        print(f'Found: {num}')
        break
```

実行してみると10桁の素数が見つかった。これを当てはめて完成したフラグを提出すると無事通った。