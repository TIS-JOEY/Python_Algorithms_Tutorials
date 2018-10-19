# 貪婪演算法

貪婪演算法和動態規劃一樣，都是在尋求最佳解的方式，但動態規劃是利用弟回特性將一個事情分割成許多小事情進行處理。而貪婪演算法則部會切割問題，而是每一步選擇最佳過程來解出答案，也就是每次取得局部最佳解。

## 最小生成樹

所謂的最小升成樹就是包含所有頂點且有最小距離路徑，但各節點間並無連通性，所謂的連通性，便是指頂點間會形成迴路。

有許多演算法可以來找出最小生成樹，包括Prim、Kruskal等等。接下來就來看看各代碼吧。

### Algorithm 1 Prim 演算法

輸入：鄰接矩陣  
輸出：最小生成樹的增添順序路徑  
目標：找出最小生成樹

Prim演算法的意義為從任一起點開始，在每一次都會選出與目前圖最短距離的節點，而當加入點後，就可以以該點去延伸找出其他最短距離點。

```text
def prim(adj_list):
	travel = [0]
	start = 0
	while(len(travel) != len(adj_list)):
		min_value = float('inf')
		can = None
		for i in travel:
			choose = min([(w,index) for index,w in enumerate(adj_list[i]) if index not in travel])
			if(choose[0] < min_value):
				min_value = choose[0]
				can = choose[1]
		travel.append(can)

	return travel
```

### Algorithm 2 Kruskal 演算法

輸入：圖\(含節點與邊\)  
輸出：最小生成樹的增添順序路徑  
目標：找出最小生成樹

Kruskal演算法，每一次都會從圖中找出還沒遍歷過的最短路徑，但要檢查當加入此路徑後，會不會形成連通圖。

```text
class kruskal:
	def __init__(self,graph):
		self.parent = {n:n for n in graph['vertices']}
		self.edges = [v for v in graph['edges']]
		self.edges.sort()
    
    # 找到所連接的最終節點並更新 
	def find_set(self,vertice):
		if(self.parent[vertice]!=vertice):
			self.parent[vertice] = self.find_set(self.parent[vertice])

		return self.parent[vertice]

	def union(self,u,v):
		ancestor1 = self.find_set(u)
		ancestor2 = self.find_set(v)
        
         # 若兩者最終節點不同，則要進行更新，將兩者連接在一起。
		if ancestor1!=ancestor2:
			self.parent[ancestor1] = ancestor2

	def kruskal_travel(self):
		mst = []
		for edge in self.edges:
			weight,u,v = edge
             
            # 若兩者並無連接
			if self.find_set(u)!=self.find_set(v):
				mst.append(edge)
				self.union(u,v)

```

### Algorithm 3 Dijkstra 演算法

輸入：相鄰矩陣  
輸出：起點到各點的最短距離  
目標：找出起點到各點的最短距離

每次選出離原點最短的點，並更新。

```text
def dijkstra(adj_list,start = 0):
    visited = [start]
    weight = adj_list[start]
    
    while(len(visited)!=len(adj_list[0])):
        # 選出離原點最短的點。
        ans = min([(value,idx) for idx,value in enumerate(weight) if idx not in visited])
        visited.append(ans[1])
        
        # 當加入點後，對距離進行更新。
        for i in range(len(weight)):
            weight[i] = min(weight[i],ans[0] + adj_list[ans[1]][i])
    
    return weight
```

### Algorithm 4 排程問題

在現實社會中，我們常會用到排程的概念，哪些事要先做，哪些事先擱置，每件事要做的時間與價值都不同，那麼排程問題就是要來找出這個最佳順序。

輸入：一個包含各事情的截止時間、價值的串列。  
輸出：最佳排程順序  
目標：最小化成本，最大化價值

輸入的形式為

| JOB | Deadline | Profit |
| :--- | :--- | :--- |
| 1 | 2 | 30 |
| 2 | 1 | 35 |
| 3 | 2 | 25 |
| 4 | 1 | 40 |

```text
def sch(job_list,n):
	# 依照獲利由大到小排序
	job_list.sort(key = lambda x:x[2],reverse = True)

	# 找出最大時限
	max_deadline = max([i[1] for i in job_list])

	# 最多可以選最大時限個工作
	slot = [False for i in range(max_deadline)]

	
	value = 0

	# 從最大獲利的工作開始找
	for i in range(n):
		

		# 依時序塞入，若當前工作的時限小於目前時間，則以當前工作時限為主，因為代表當前工作不能塞入現在的時限。
		# 但若往前找時，發現前一個時序已經有工作了，則放棄，因為先塞入的一定是獲利較高的。
		for j in range(min(n,job_list[i][1]-1),-1,-1):
			if slot[j]== False:
				value+=job_list[i][2]
				slot[j] = True
				break
	print(value)
```

### Algorithm 5 霍夫曼編碼

霍夫曼編碼是一種可變動長度的二進位碼，他可以獲得更高的編碼效率，其會先以字頻來作為依據，來對使用的字詞進行二進位編碼，越常用到的字，其編碼長度會越小，如此可以大大縮短處理時間。

但要注意的是，各字詞的前啜字不得為其他字

如a 我們設為0，而b我們設為00，則就會產生混淆。

```text
class TreeNode:
    def __init__(self,freq):
        self.left = None
        self.right = None
        self.parent = None
        self.freq = freq
    
    def isLeft(self):
        return self.parent.left == self

class HuffmanTree:
    def __init__(self,freqs):
        self.nodes = [TreeNode(freq) for freq in freqs]
    
    def createHuffmanTree(self):
        queue = self.nodes[:]
        
        while len(queue) > 1:
        
            # 找出最小頻率的兩個節點，並將其進行結合
            queue.sort(key = lambda x:x.freq)
            node_left = queue.pop(0)
            node_right = queue.pop(0)
            node_parent = TreeNode(node_left.freq + node_right.freq)
            node_parent.left = node_left
            node_parent.right = node_right
            node_left.parent = node_parent
            node_right.parent = node_parent
            
            # 將結合好的樹加進queue，
            queue.append(node_parent)
        
        queue[0].parent = None
        return queue[0]
```

### Algorithm 6 0-1背包問題

0-1 背包問題指的是，有個最多可以負重m公斤的背包，要如何拿不同價值不同重量的物品並裝進背包中，使得在不超過負重下，只拿n個物品並價值最大化的問題。

```text
def pack01(v,w,weight,n):

    # 建構出不同物品數量不同重量下的最大價值陣列。
    # resArr的row代表著拿了幾個物品
    # 每一個column代表多少負重
    resArr = [[0 for i in range(weight+1)] for i in range(n+1)]
    
    for i in range(1,n+1):
        for j in range(1,weight+1):
            if(w[i] <= j):
                # resArr[i-1][j] 代表不拿j物品的最佳化組合
                # resArr[i-1][j-w[i]] + w[i] 代表拿了j物品後的價值總和
                resArr[i][j] = max(resArr[i-1][j], resArr[i-1][j-w[i]] + w[i])
            else:
                resArr[i][j] = resArr[i-1][j]
```

如此可以確保，在每一個負重下的組合，都會是最佳化的結果。

