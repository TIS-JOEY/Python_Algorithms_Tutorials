# 演算法效率分析

在現實社會中，同一種問題可能會有好多種不同的解決方式，同樣都可以解決掉問題，但有些方式成本小，有些方式成本大，有些方式好，那麼當然有些方式就相對壞了。

演算法，聽起來名字很玄，但他的主要概念其實可以直接翻譯成白話，就是解決問題的方法。程式若只執行一次，那麼成本控制或是就沒那麼重要，但事實上，程式通常會長時性地不斷運行，因此在我們編寫程式系統時，必須非常地精打細算。但，我們要如何去評斷一個演算法的好壞呢？就讓我們繼續看下去。

我們在看演算法程式最直接想到的成本，就是他使用了多少記憶體空間以及所執行的時間，也就是所謂的「空間複雜度」與「時間複雜度」。

正所謂「知己知彼，百戰百勝」，為了要分析一個演算法，那我們就必須要了解演算法的結構，也就是下面的幾項。

* 問題：遇到的問題。
* 輸入：手中擁有的資源。
* 解決問題的過程：演算法。
* 輸出：解決掉問題後的結果。

首先，先讓我們來看幾個簡單的演算法熟悉一下。

### Algorithm1 循序搜尋法

* 問題：判斷數字x是否位於含有n個數字的陣列S中。
* 輸入：數字x、含有n個數字的陣列S。
* 輸出：x位於陣列S的位置，若不在則回傳-1。

```text
def sequential(x,S):
    for i in range(len(S)):
        if S[i] == x:
            return i
    return -1
```

### Algorithm2 陣列加總

* 問題：加總陣列S中的數字。
* 輸入：陣列S。
* 輸出：陣列S中的數字和。

```text
from functools import reduce
def array_sum(S):
    return reduce(lambda i,j:i+j,S)
```

### Algorithm3 交換排序法

* 問題：以非遞減順序對陣列排序。
* 輸入：正整數n，含有n個數字的陣列S
* 輸出：非遞減順序的陣列S

```text
def exchangeSort(S):
    for i in range(len(S)):
        for j in range(i+1,len(S)):
            if(S[i]>S[j]):
                S[i],S[j] = S[j],S[i]
```

### Algorithm4 矩陣乘法

* 問題：求兩個矩陣A,B的乘積
* 輸入：兩個矩陣A,B
* 輸出：矩陣乘積

```text
def matrix_Mult(A,B):
    res = [[] for i in range(len(A))]
    for i in range(len(A)):
        for j in range(len(B[0])):
            k = 0
            value = 0
            while(k<len(A[0])):
                value+=(A[i][k]*B[k][j])
                k+=1
            res[i].append(value)     
    return res
            
```

