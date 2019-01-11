# Divide and Conquer

## Preorder 
[root, left, right]

## 2 recursion methods:
#### traverse
result in parameter

top-down

```py
    def run(self, root):
        res = []
        self.preorder1(root, res)
        print(res)
        return

    def preorder1(self, root, res):
        if root:
            res.append(root.val)
        else:
            return
        self.preorder1(root.left, res)
        self.preorder1(root.right, res)
        return

```
#### divide conquer(?)
result in return value (for merge)

bottom-up
```py
    def preorder2(self, root):
        res = []
        if root:
            res.append(root.val)

        if root.left:
            res += self.preorder2(root.left)

        if root.right:
            res += self.preorder2(root.right)

        return res
```
***
### 典型题
can solve 90% binary tree problem

##### return all routes
```py
# traverse
    def run_routes(self, root):
        result = self.routes(root, [], [])
        return result

    def routes(self, root, route, result):
        if root:
            route.append(root.val)

        if not root.left or not root.right:
            result.append(route)

        new_route = copy.deepcopy(route)
        if root.left:
            self.routes(root.left, new_route, result)

        new_route = copy.deepcopy(route)
        if root.right:
            self.routes(root.right, new_route, result)

        return result
```

```py
divide-conquer
    def routes2(self, root):
        routes = []
        result = []
        if root:
            routes.append(root.val)

        if not root.left and not root.right:
            result.append(routes)

        new_route = copy.deepcopy(routes)
        if root.left:
            result += [new_route + route for route in self.routes2(root.left)]

        new_route = copy.deepcopy(routes)
        if root.right:
            result += [new_route + route for route in self.routes2(root.right)]

        return result
```
##### return depth
##### return the root of the minimum subtree
##### subtree with maximum average（不好使用全局变量）
(left_sum + right_sum)/(left_size + right_size)
##### lowest common ancestor (最近公共祖先)

## BST
#### validate

#### operate
##### convert to doubly-linked list
##### insert node in binary search tree
##### remove node
 

# A Rule
```
DFS{
    Recursion{先写拆解， 再写出口
        
        traverse
        divide-conquer
    }
    Non-Recursion{
        ……
    }
} 
```

## Global variable is not a good pattern