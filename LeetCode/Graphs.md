BFS:
```
def bfs(r,c):
	q = deque()
	visited.add((r,c))
	q.append((r,c))
	while q:
		r,c = q.popleft()
		directions = [[1,0], [-1,0], [0,1], [0,-1]]
		for dr,dc in directions: 
			new_row, new_col = r + dr, c+dc
			if (check the conditions ex. if not in visited):
				q.append((new_row, new_col))
				visited.add((new_row, new_col))
```
DFS:
```
def dfs(r,c):
	q = deque()
	visited.add((r,c))
	q.append((r,c))
	while q:
		r,c = q.popright() // just using stack instead of a queue
		directions = [[1,0], [-1,0], [0,1], [0,-1]]
		for dr,dc in directions: 
			new_row, new_col = r + dr, c+dc
			if (check the conditions ex. if not in visited):
				q.append((new_row, new_col))
				visited.add((new_row, new_col))
```



