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

## Algorithm7 連續矩陣相乘

假設我們有A,B,C,D,E,F要相乘，而維度分別為：  
A = 5 \* 2  
B = 2 \* 3  
C = 3 \* 4  
D = 4 \* 6  
E = 6 \* 7  
F = 7 \* 8  
那麼若我們使用結合律的概念將一些矩陣先進行乘法，所花費的乘法次數會不會下降呢？

先來看看普通兩個矩陣相乘時，他所花費的乘法次數會多少，假設是A\*B，那麼他的乘法次數應該為 5 \* 2 \* 3，而若A \* \(B \* C\)則他所花費的乘法應為 2 \* 3 \* 4 + 5 \* 2 \* 4 = 64，但若為\(A\*B\) \* C，則所花費的乘法應為 5 \* 2 \* 3 + 5 \* 3 \* 4 = 90，透過此小例子就知道我們在這邊所要解決的就是找出最佳的矩陣相乘順序。

而在這個問題中，就是不斷地去測試哪些乘法順序的乘法次數較少，那麼很肯定的，一定有很多東西會被重複計算，這時動態規劃又派上用場了。

我們會嘗試建立兩個矩陣，一個為記錄所花費的最少乘法次數，一個是記錄乘法的順序。其中記錄花費的最少乘法次數的矩陣應該長得像下面這個圖：

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">A</th>
      <th style="text-align:left">B</th>
      <th style="text-align:left">C</th>
      <th style="text-align:left">D</th>
      <th style="text-align:left">E</th>
      <th style="text-align:left">F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">A</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">(A*B)</td>
      <td style="text-align:left">(A*B)*C
        <br />A*(B*C)</td>
      <td style="text-align:left">
        <p>((A*B)*C)*D</p>
        <p>(A*(B*C))*D</p>
        <p>A*((B*C)*D)</p>
        <p>A*(B*(C*D))</p>
      </td>
      <td style="text-align:left">
        <p>(((A*B)*C)*D)*E</p>
        <p>((A*(B*C))*D)*E</p>
        <p>(A*((B*C)*D))*E</p>
        <p>(A*(B*(C*D)))*E</p>
        <p>A*(((B*C)*D)*E)</p>
        <p>A*((B* (C*D))*E)</p>
        <p>A*(B*((C*D)*E))</p>
        <p>A*(B*(C*(D*E)))</p>
      </td>
      <td style="text-align:left">~</td>
    </tr>
    <tr>
      <td style="text-align:left">B</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">(B*C)</td>
      <td style="text-align:left">
        <p>(B*C)*D</p>
        <p>B* (C*D)</p>
      </td>
      <td style="text-align:left">
        <p>((B*C)*D)*E</p>
        <p>(B* (C*D))*E</p>
        <p>B*((C*D)*E)</p>
        <p>B*(C*(D*E))</p>
      </td>
      <td style="text-align:left">~</td>
    </tr>
    <tr>
      <td style="text-align:left">C</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">(C*D)</td>
      <td style="text-align:left">
        <p>(C*D)*E</p>
        <p>C*(D*E)</p>
      </td>
      <td style="text-align:left">~</td>
    </tr>
    <tr>
      <td style="text-align:left">D</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">(D*E)</td>
      <td style="text-align:left">~</td>
    </tr>
    <tr>
      <td style="text-align:left">E</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">(E*F)</td>
    </tr>
    <tr>
      <td style="text-align:left">F</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
    </tr>
  </tbody>
</table>可以發現row與column對應的矩陣是相鄰時，他會紀錄其直接相乘的，而其餘所記錄的皆是：  
列數+1的欄位中的乘積組合結合自己的欄的矩陣的新組成 以及  
欄數-1的欄位中的成績組合結合自己的列的矩陣的新組成  
例如：\[1\]\[3\]的位置，會是由\[1\]\[2\]與自己欄值的矩陣結合 和 \[2\]\[3\]與自己的列值的矩陣結合。

> \[1\]\[2\]為\(B\*C\)，則\[1\]\[3\]就要加上\(B\*C\)\*D的組合，而\[2\]\[3\]為\(C\*D\)，則\[1\]\[3\]就要再加上B\*\(C\*D\)。

基於上述的概念，我們可以發現我們會重複計算很多東西，但事實上，我們僅需要在各欄位中先找出他們最小乘法次數的組合並記錄起來，就可以節省很多不必要的重複計算功夫。

* 問題：找出最佳連續矩陣乘法順序
* 輸入：記錄各矩陣的維度的串列，每個矩陣以tuple方式記錄
* 輸出：回傳記錄最少乘法次數的矩陣M、記錄最佳乘法順序P

```text
def multiMulti(U):
    n = len(U)
    M = [[0 for _ in range(n)] for _ in range(n)]
    P = [[0 for _ in range(n)] for _ in range(n)]
    for j in range(1,n):
        for i in range(j-1,-1,-1):
            if(j==i+1):
                #連續矩陣
                
                #記錄相鄰矩陣乘法次數
                M[i][j] = U[i][0] * U[j][0] * U[j][1]
                
                #記錄乘法順序
                P[i][j] = [i,j]
                
            else:
                val1 = M[i][j-1] + U[i][0] * U[j-1][1] * U[j][1]
                #欄數-1的位置值
                
                val2 = M[i+1][j] + U[i][0] * U[i+1][0] * U[j][1]
                #列數-1的位置
                
                #記錄擁有較少乘法次數的組合
                if(val1>val2):
                    M[i][j] = val2
                    #記錄乘法次數
                    
                    P[i][j] = P[i+1][j] + [i]
                    #記錄選擇出的子乘積順序以及自己
                else:
                    M[i][j] = val1
                    #記錄乘法次數
                    
                    P[i][j] = P[i][j-1] + [j]
                    #記錄選擇出的子乘積順序以及自己
                    
    return M,P
```

則最後輸出時，M\[0\]\[-1\]和P\[0\]\[-1\]就會記錄最低乘法次數與最佳乘法順序。

時間複雜度：O\(n^2\)

