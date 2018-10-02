# Divide-and-Conquer

Divider-and-Conquer是由上而下\(Top-Down\)的思考概念，將問題分為各個小問題，分而治之，進而解決問題。相信大家都有學過遞迴Recursive，它的精神就是Divide-and-Conquer，但要注意幾點：

1. 決定終止條件
2. 要隨著呼叫函式，越來越朝向終止條件方向前進。

接下來，介紹幾個遞迴的演算法。

## Algorithm1 二元搜尋法\(遞迴版\)

* 問題：判斷數字goal\_value是否位於陣列S中。
* 輸入：數字goal\_value、陣列S。
* 輸出：x位於陣列S的位置，若不在則回傳-1。

```text
def binary_Search_recursive(S,low, high,goal_value):
    if(low<=high):
        mid = (low+high)//2
        if(goal_value==S[mid]):
            return mid
        elif(goal_value<S[mid]):
            return binary_Search_recursive(array,low,mid-1,goal_value)
        else:
            return binary_Search_recursive(array,mid+1,high,goal_value)
    else:
        return -1
```

時間複雜度：O\(log n\)。

> 此演算法採用tail-recursion，代表所有運算都在遞迴呼叫前完成，對於C++等於言中，利用iteration來取代tail-recursion是較好的，因為這樣可以消除遞迴呼叫中使用的堆疊來節省大量的記憶體。。
>
> 在呼叫遞迴時，其工作原理為，將呼叫的函式擱置的結果放置Activation Record的堆疊中，然後執行被呼叫的程式，當輪到原本呼叫的函式時，就會從該堆疊中pop出來。

## Algorithm2 合併排序法

* 問題：將陣列S進行由小到大排序。
* 輸入：陣列S。
* 輸出：由小到大排序的陣列S。

```text
def mergeSort(S):
    if(len(S)>1):
        mid = len(S)//2
        
        left_part = S[:mid]
        right_part = S[mid:]
        mergeSort(left_part)
        mergeSort(right_part)
        
        left_index = 0
        right_index = 0
        ans_index = 0
        
        while(left_index < len(left_part) and right_index < len(right_part)):
            if(left_part[left_index] < right_part[right_index]):
                S[ans_index] = left_part[left_index]
                left_index+=1
            else:
                S[ans_index] = right_part[right_index]
                right_index+=1
            ans_index+=1
            
        
        while(left_index < len(left_part)):
            S[ans_index] = left_part[left_index]
            left_index+=1
            ans_index+=1
        
        while(right_index) < len(right_part):
            S[ans_index] = right_part[right_index]
            right_index+=1
            ans_index+=1
```

時間複雜度：合併的時間複雜度為兩個陣列的各自長度相加減一，因為最壞的情況下，其中一個陣列已排序進去，而另一個陣列還差1，再排進去後會跳出迴圈。對於某n，因此排序的複雜度應為W\(n\) = W\(h\) + W\(m\) + h + m -1。假設n為2的乘冪，則每次進行合併排序時應為W\(n\) = 2W\(n/2\) + n -1，而W\(1\)應為0。基於此概念，因此合併排序的時間複雜度應為 O\(n log n\)。

## Algorithm3 快速排序法

* 問題：將陣列S進行由小到大排序。
* 輸入：陣列S。
* 輸出：由小到大排序的陣列S。

```text
def quickSort(S):
    if(len(S))<1:
        return []
    pivot = S[0]
    return quickSort([i for i in S if pivot > i]) + [pivot] + quickSort([i for i in S if pivot < i])
```

時間複雜度：  
最壞情況下會發生在pivot剛好在發生在S的最小值或最大值，導致無法發揮切分為2的效果--&gt; T\(n\) = T\(0\) + T\(n-1\) + n-1 \(分割時間\) for n &gt; 0, T\(0\) = 0  
T\(n\) = n\(n-1\)//2 --&gt; 因此時間複雜度為O\(n ^ 2\)。

最好情況下則會發生在pivot可把S切分為2 --&gt; T\(n\) = 2 \* T\(n/2\) + n-1，因此時間複雜度為 O\(n log n\)。

## Algorithm4 Strassen矩陣相乘

* 問題：計算方陣乘積。
* 輸入：方陣A,B，方陣維度。
* 輸出：乘積。

傳統的方陣相乘的時間複雜度為θ\(n ^ 3\)，其中n為列數與行數。假設C為A和B的乘積，Strassen令  
m1 = \(a11 + a22\) \* \(b11 + b22\)  
m2 = \(a21 + a22\) \* b11  
m3 = a11 \* \(b12 - b22\)  
m4 = a22 \* \(b21 - b11\)  
m5 = \(a11 + a12\) \* b22  
m6 = \(a21 - a11\) \* \(b11 + b12\)  
m7 = \(a12 - a22\) \* \(b21 + b22\)

```text
C = [[c11,c12],[c21,c22]]
A = [[a11,a12],[a21,a22]]
B = [[b11,b12],[b21,b22]]
```

則C可表示為

```text
C = [[m1+m4-m5+m7,m3+m5],[m2+m4,m1+m3-m2+m6]]
```

此方法需要7次乘法與18次的加減法，直接計算則需要8次乘法與4次加減法，其公式概念是將較大的矩陣分成四個較小的子矩陣，而當矩陣已足夠小時，則使用傳統的矩陣乘法，最後再把結果合併起來。

```text
import numpy as np
def strassenMatrixMult(n,A,B):
    if(n<=2):
        return np.array(np.dot(A,B))
    else:
        mid = len(A)//2
        A11,A12,A21,A22 = np.array([i[:mid] for i in A[:mid]]),np.array([i[mid:] for i in A[:mid]]),np.array([i[:mid] for i in A[mid:]]),np.array([i[mid:] for i in A[mid:]])
        B11,B12,B21,B22 = np.array([i[:mid] for i in B[:mid]]),np.array([i[mid:] for i in B[:mid]]),np.array([i[:mid] for i in B[mid:]]),np.array([i[mid:] for i in B[mid:]])
        m1 = strassenMatrixMult(n//2,A11+A22,B11+B22)
        m2 = strassenMatrixMult(n//2,A21+A22,B11)
        m3 = strassenMatrixMult(n//2,A11,B12-B22)
        m4 = strassenMatrixMult(n//2,A22,B21-B11)
        m5 = strassenMatrixMult(n//2,A11+A12,B22)
        m6 = strassenMatrixMult(n//2,A21-A11,B11+B12)
        m7 = strassenMatrixMult(n//2,A12-A22,B21+B22)

        C11 = m1+m4-m5+m7
        C12 = m3+m5
        C21 = m2+m4
        C22 = m1+m3-m2+m6
        return np.vstack((np.hstack((C11,C12)),np.hstack((C21,C22))))
```

時間複雜度：  
7次乘法，18次加減法  
T\(n\) = 7T\(n//2\)+18\(\(n//2\)^2\)  
--&gt; T\(n\) = 6\(n^log7\) - 6n^2 = O\(n^2.81\)

## Algorithm 5 大整數計算

在大整數計算而大小超過硬體表達整數能力會產生誤差，這時要確保每位數值皆正確，改用浮點數表示法就無法派上用法。這時就要過divide-and-conquer的概念，將原大整數以不同底不同位數的次方表示出來。以10為底的方式來說，4321 = 4\*\(10^3\)+3\*\(10^2\)+2\*\(10^1\)+1\*\(10^0\)，以陣列表示的話就是S\[3\] = 4,S\[2\] = 3,S\[1\] = 2,S\[0\] = 1。

因此我們都可以將某數值A以x\*10^m + y來表示。  
而假設我們又有另一數值B可以以w\*10^m + z來表示。  
兩者乘積可以表達為xw\*10^\(2m\) + \(xz+wy\)\*\(10^m\) + yz，同樣地這些較小整數的乘積亦可不斷遞迴直到遇到閥值，轉用傳統乘法完成。

因此我們先來分析我們的問題、輸入與輸出吧

* 問題：計算A與B的乘積
* 輸入：整數A與B。
* 輸出：乘積。

```text
def largeNumberMulti(A,B):
    n = max(len(str(A)),len(str(B)))
    if(A==0 or B ==0):
        return 0
    elif(n<=2):
        return A*B
    else:
        m = n//2
        quotient1,rem1 = divmod(A,10**m)
        
        quotient2,rem2 = divmod(B,10**m)
        return largeNumber(quotient1,quotient2)*(10**(2*m)) + (largeNumber(quotient2,rem1)+largeNumber(quotient1,rem2))*(10**m)+largeNumber(rem1,rem2)
```

時間複雜度：O\(n^2\)

可以再進一步簡化，將一些乘法改為線性的加減法，原本的運算有xw、xz、wy與yz，令r = \(x+y\)\(w+z\) = xw+\(xz+yw\)+yz，可得xz+yw = r-xw-yz，因此我們要用的乘法就剩xw, yz和r了。

```text
def largeNumberMulti2(A,B):
    n = max(len(str(A)),len(str(B)))
    if(A==0 or B ==0):
        return 0
    elif(n<=2):
        return A*B
    else:
        m = n//2
        quotient1,rem1 = divmod(A,10**m)
        quotient2,rem2 = divmod(B,10**m)
        r = largeNumberMulti(quotient1+rem1,quotient2+rem2)
        p = largeNumberMulti(quotient1,quotient2)
        q = largeNumberMulti(rem1,rem2)
        return p*(10**(2*m)) + (r-p-q)*(10**m) + q
```

時間複雜度：O\(n^1.58\)



