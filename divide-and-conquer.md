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

時間複雜度：θ\(log n\)。

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

時間複雜度：合併的時間複雜度為兩個陣列的各自長度相加減一，因為最壞的情況下，其中一個陣列已排序進去，而另一個陣列還差1，再排進去後會跳出迴圈。對於某n，排序的複雜度應為W\(n\) = W\(h\) + W\(m\) + h + m -1。假設n為2的乘冪，則每次進行合併排序時應為W\(n\) = 2W\(n/2\) + n -1，而W\(1\)應為0。基於此概念，因此合併排序的時間複雜度應為 θ\(n log n\)。

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
T\(n\) = n\(n-1\)//2 --&gt; 因此時間複雜度為θ\(n ^ 2\)。

最好情況下則會發生在pivot可把S切分為2 --&gt; T\(n\) = 2 \* T\(n/2\) + n-1，因此時間複雜度為 θ\(n log n\)。

