# BFS
## Binary Tree
### level order Traversal
use Queue
    bfs + queue
    dfs + stack

### Serialization (Microsoft, Yahoo)
#### Serialize and Deserialize Binary Tree
```py
# BFS
class TreeNode(object):
    def __init__(self, x=None, left=None, right=None):
        self.val = x
        self.left = left
        self.right = right


class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string.

        :type root: TreeNode
        :rtype: str
        """

        if root is None:
            return None

        node = [root]
        i = 0
        num = 0
        while i < len(node):
            if node[i].val is not None:
                if node[i].left is None and node[i].right is None:
                    num += 2
                else:
                    node += [TreeNode(None)] * num
                    num = 0

                    if node[i].left:
                        node.append(node[i].left)
                    else:
                        node.append(TreeNode(None))
                    if node[i].right:
                        node.append(node[i].right)
                    else:
                        num += 1
            i += 1
        node_val = [n.val for n in node]
        return str(node_val)

    def deserialize(self, data):
        """Decodes your encoded data to tree.

        :type data: str
        :rtype: TreeNode
        """
        if data is None or len(data) <= 2:
            return None

        # str to list
        data = data[1:-1]
        data_list = data.split(', ')
        for j in range(len(data_list)):
            if data_list[j] != 'None':
                data_list[j] = int(data_list[j])
            else:
                data_list[j] = None

        d = 0
        i = 0
        node = [TreeNode(data_list[0])]
        len_data = len(data_list)
        while i < len(node):
            if node[i].val is not None:
                d += 1
                if d < len_data:
                    node[i].left = TreeNode(data_list[d])
                    node.append(node[i].left)
                d += 1
                if d < len_data:
                    node[i].right = TreeNode(data_list[d])
                    node.append(node[i].right)
            i += 1

        val = [i.val for i in node]

        return val
```
```py
# dfs leetcode solution
    def serialize_dfs(self, root):
        """ Encodes a tree to a single string.
        :type root: TreeNode
        :rtype: str
        """
        def rserialize(root, string):
            """ a recursive helper function for the serialize() function."""
            # check base case
            if root is None:
                string += 'None,'
            else:
                string += str(root.val) + ','
                string = rserialize(root.left, string)
                string = rserialize(root.right, string)
            return string

        return rserialize(root, '')



    def deserialize_dfs(self, data):

        """Decodes your encoded data to tree.
        :type data: str
        :rtype: TreeNode
        """

        def rdeserialize(l):
            """ a recursive helper function for deserialization."""
            if l[0] == 'None':
                l.pop(0)
                return None

            root = TreeNode(l[0])
            l.pop(0)
            root.left = rdeserialize(l)
            root.right = rdeserialize(l)
            return root

        data_list = data.split(',')
        root = rdeserialize(data_list)
        return root
```

## Graph
与树不同之处：存在环，用HashMap去重

### Topological Sorting
#### Sequence Reconstructino
#### 裸拓扑排序（？）
### Connected Component 比如判断连通性
queue, hash

#### Graph Valid Tree
#### different graph presentation
#### clone graph
```py
# BFS
class Solution:
    # @param node, a undirected graph node
    # @return a undirected graph node
    def cloneGraph(self, node):
        if not node:
            return None
        
        node_copy = UndirectedGraphNode(node.label)
        vertex = {node.label: node_copy}
        exist = {node.label: node}
        graph = [node]

        # add vertexes
        for g in graph:
            for n in g.neighbors:
                if n.label not in exist.keys():
                    graph.append(n)
                    vertex[n.label] = UndirectedGraphNode(n.label)
                    exist[n.label] = n

        # add edges
        assert len(exist) == len(vertex)

        for k, v in exist.items():
            for n in exist[k].neighbors:
                vertex[k].neighbors.append(vertex[n.label])

        return node_copy
```
```py
# BFS a more sound solution
    def cloneGraph1(self, node):
        if not node:
            return
        nodeCopy = UndirectedGraphNode(node.label)
        dic = {node: nodeCopy}
        queue = collections.deque([node])
        while queue:
            node = queue.popleft()
            for neighbor in node.neighbors:
                if neighbor not in dic:  # neighbor is not visited
                    neighborCopy = UndirectedGraphNode(neighbor.label)
                    dic[neighbor] = neighborCopy
                    dic[node].neighbors.append(neighborCopy)
                    queue.append(neighbor)
                else:
                    dic[node].neighbors.append(dic[neighbor])
        return nodeCopy
```
```py
    # DFS iteratively
    def cloneGraph2(self, node):
        if not node:
            return
        nodeCopy = UndirectedGraphNode(node.label)
        dic = {node: nodeCopy}
        stack = [node]
        while stack:
            node = stack.pop()
            for neighbor in node.neighbors:
                if neighbor not in dic:
                    neighborCopy = UndirectedGraphNode(neighbor.label)
                    dic[neighbor] = neighborCopy
                    dic[node].neighbors.append(neighborCopy)
                    stack.append(neighbor)
                else:
                    dic[node].neighbors.append(dic[neighbor])
        return nodeCopy

    # DFS recursively
    def cloneGraph(self, node):
        if not node:
            return
        nodeCopy = UndirectedGraphNode(node.label)
        dic = {node: nodeCopy}
        self.dfs(node, dic)
        return nodeCopy

    def dfs(self, node, dic):
        for neighbor in node.neighbors:
            if neighbor not in dic:
                neighborCopy = UndirectedGraphNode(neighbor.label)
                dic[neighbor] = neighborCopy
                dic[node].neighbors.append(neighborCopy)
                print('dic_before', len(dic))
                self.dfs(neighbor, dic)
                print('dic', len(dic))
            else:
                dic[node].neighbors.append(dic[neighbor])

```
#### Search Graph Nodes

### Level Order Traversal

### Shortest Path in Simple Graph (length = 1, no directino)
For shortest path: dynamic programming
For longest path: DP, DFS(cannot apply BFS)

### Matrix
#### Number of Islands
##### 坐标变换数组
#### Zombie in matrix

## Chess Sorting
#### Knight Shortest Path

