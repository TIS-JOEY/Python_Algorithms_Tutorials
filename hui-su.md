# 回溯

關於回溯，你可以想像妳在走一個迷宮，當遇到死路後，就知道這條路是沒前景的，之後可以不用再走，而轉去走其他有前景的路，直到走到終點。

### Algorithm 1 n皇后問題

n皇后問題，指的是在一個n x n棋盤上，放置n個皇后，而各皇后不能互相攻擊，皇后的攻擊範圍是在同一行或同一列，抑或是對角線。

在此種情況下，我們可以去建構一個二叉樹，每個節點記錄著&lt;i,j&gt;，i代表該皇后在第i 列，第j欄，那麼若有個皇后在&lt;i,j&gt;上，則不能有其他節點是&lt;i, k&gt;、&lt;k, j&gt;，或者是行數的差額不能等於列數的差額\(對角線\)。

輸入：n個皇后  
輸出：所有可以將n個皇后放在n x n 的棋盤上而不互相攻擊的方法。

```text
def queen(A,cur = 0):
	if cur==len(A):
		print(A)
	else:
		for col in range(len(A)):
			# 把皇后放在cur列，col欄
			A[cur] = col
			done = True

			# 檢查前面的皇后
			for r in range(cur):
				# A[r] == col 為判斷是否為同欄
				# abs(A[r]-col) == abs(cur-r) 判斷是否在對角線
				if(A[r]==col or abs(A[r]-col) == abs(cur-r)):
					done = False
					break
					
			# 若為死路就不用再遞迴下去了
			if done:
				self.queen(A,cur+1)

```



