# 動態規劃

動態規劃\(Dynamic programming\)與Divide-and-Conquer都會先將問題切成數個較小且性質相同的問題，但DP會先去計算小的問題，並儲存結果，以快取cache的概念，使得有些東西不用一直重複地被計算，而可以直接取得，屬於由下而上\(Bottom-up\)的思考概念。

## Algorithm1 費伯納西數列\(Divide-and-Conquer\)

* 問題：求得費伯納西數列的第n個值
* 輸入：非負整數n
* 輸出：費柏納西數列的第n個值

```text
def fibnoacci_recursive(n):
    if(n<0):
        raise ValueError
    elif(n<=1):
        return 1
    else:
        return fibnoacci_recursive(n-1)+fibnoacci_recursive(n-2)
```

## Algorithm2 費伯納西數列\(動態規劃\)

* 問題：求得費伯納西數列的第n個值
* 輸入：非負整數n
* 輸出：費柏納西數列的第n個值

```text
def fibnoacci_iterative(n):
    if(n<0):
        raise ValueError
    x,y = 0,1
    for i in range(n):
        x,y = y,x+y
    return y
```

可以看到上面兩個不同的費伯納西數列演算法，都可以達到我們要的目的，但所花費的成本卻大大不同。首先是遞迴版的，很明顯這就是屬於Divide-and-Conquer的演算法，因為每個數字都是前兩個數字的總和，因此我們可以以遞迴的方式達到，並由上而下慢慢遞減，直到達到閥值可以直接return，再慢慢往上進行總和最終求得目標值。但這種方式卻有個缺點，那就是有很多數字會重複被計算，這樣就會造成時間的浪費。Algorithm1 的時間複雜度應為O\(2^n\)，其所花費的成本是相當大的。

那麼再來讓我們看看迴圈版吧，可以看到其是由最小的數字開始慢慢累加，可以看到這樣就可以避免數字被重複計算，此演算法的時間複雜度為O\(n\)，大大節省了時間成本。

## Algorithm3 二次項係數\(Divide-and-Conquer\)

* 問題：計算二次項係數
* 輸入：非負整數n與k，其中k&lt;=n
* 輸出：二次項係數值

```text
def bin_divide_and_conquer(n,k):
    if(n==0 or k==n):
        return 1
    else:
        return bin(n-1,k-1)+bin(n-1,k)
```

因公式轉換可求得上述的演算法邏輯，但一樣，會有個問題，那就是會重複計算數值，此演算法的時間複雜度為2\*bin\(n,k\) - 1。

## Algorithm4 二次項係數\(DP\)

* 問題：計算二次項係數
* 輸入：非負整數n與k，其中k&lt;=n
* 輸出：二次項係數值

```text
def bin_dp(n,k):
    mapping_table = [[0 for _ in range(k+1)] for _ in range(n+1)]
    for i in range(n+1):
        for j in range(k+1):
            if(j==0 or i==j):
                mapping_table[i][j] = 1
            else:
                mapping_table[i][j] = mapping_table[i-1][j-1] + mapping_table[i-1][j]
    return mapping_table[n][k]
```

時間複雜度為：O\(nk\)。

## Algorithm5 佛洛伊德最短路徑

* 問題：計算各頂點間的最短路徑
* 輸入：有向權重圖，以相鄰矩陣表示
* 輸出：最短路徑圖

```text
def floyd(w):
    for i in range(len(w)):
        for j in range(len(w)):
            for k in range(len(w)):
                w[j][k] = min(w[j][k],w[j][i]+w[i][k])
    return w
```

時間複雜度為：O\(n^3\)。

## Algorithm6 佛洛伊德最短路徑2

* 問題：計算各頂點間的最短路徑
* 輸入：有向權重圖，以相鄰矩陣表示
* 輸出：最短路徑圖與各頂點最短路徑中中介點之矩陣圖

```text
def floyd(w):
    p = [[-1 for _ in range(len(w))] for _ in range(len(w))]
    for i in range(len(w)):
        for j in range(len(w)):
            for k in range(len(w)):
                if(w[j][k] > w[j][i]+w[i][k]):
                    p[j][k] = i
                    w[j][k] = w[j][i]+w[i][k]
    return w,p
```

時間複雜度為：O\(n^3\)。

### 印出最短路徑

```text
def shortest_floyd_path(P,q,r):
    if(P[q][r]!=-1):
        shortest_floyd_path(P,q,P[q][r])
        print('v'+str(P[q][r]))
        shortest_floyd_path(P,P[q][r],r)
```

時間複雜度為：O\(n\)

> 最佳化原則：
>
> 1. 建立一個遞迴性質解出該問題的某最佳解
> 2. 以bottom-up求出最佳解
> 3. 用bottom-up建構出最佳解
>
> 若最佳化原則要可應用在某問題上，他必須符合以下條件：當某問題存在最佳解，則所有子問題必存在最佳解。

