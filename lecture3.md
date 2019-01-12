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
# divide-conquer
    def routes2(self, root):
        routes = []
        result = []
        if root:
            routes.append(root.val)

        if not root.left and not root.right:
            result.append(routes)
            return result

        # new_route = copy.deepcopy(routes)
        if root.left:
            # result += [new_route + route for route in self.routes2(root.left)]
            for route in self.routes2(root.left):
                new_route = copy.deepcopy(routes)
                new_route += route
                result.append(new_route)

        # new_route = copy.deepcopy(routes)
        if root.right:
            # result += [new_route + route for route in self.routes2(root.right)]
            for route in self.routes2(root.right):
                new_route = copy.deepcopy(routes)
                new_route += route
                result.append(new_route)

        return result

```
##### return depth
```py
# traverse
    def run_depth1(self, root):
        result = self.depth1(root, 0, 0)
        return result

    def depth1(self, root, depth, result):
        if root:
            depth += 1
        else:
            return result

        if not root.left and not root.right:
            if depth > result:
                result = depth
            return result

        if root.left:
            new_depth = copy.deepcopy(depth)
            new_result = self.depth1(root.left, new_depth, result)
            if result < new_result:
                result = new_result

        if root.right:
            new_depth = copy.deepcopy(depth)
            new_result = copy.deepcopy(result)
            new_result = self.depth1(root.right, new_depth, new_result)
            if result < new_result:
                result = new_result

        return result
```

```py
# divide-conquer
    def depth2(self, root):
        depth = 0
        if root:
            depth += 1
        else:
            return depth

        if not root.left and not root.right:
            return depth

        left_depth = 0
        if root.left:
            left_depth = self.depth2(root.left)

        right_depth = 0
        if root.right:
            right_depth = self.depth2(root.right)

        if left_depth > right_depth:
            return depth + left_depth
        else:
            return depth + right_depth
```
##### Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.
```py
# divide-conquer combined with traverse
    def run_root_subtree(self, root):
        result = [sys.maxsize, root]
        result = self.root_subtree_with_min_sum1(root, result)
        return result[0], result[1].val

    def root_subtree_with_min_sum1(self, root, result):
        # divide-conquer
        min_sum = 0
        if root:
            min_sum += root.val

        if not root.left and not root.right:
            if min_sum < result[0]:
                result[0] = min_sum
                result[1] = root
            return result

        if root.left:
            min_sum += self.root_subtree_with_min_sum1(root.left, result)[0]

        if root.right:
            min_sum += self.root_subtree_with_min_sum1(root.right, result)[0]

        if min_sum < result[0]:
            result[0] = min_sum
            result[1] = root

        return result
```

##### subtree with maximum average（不好使用全局变量）
(left_sum + right_sum)/(left_size + right_size)
```py
    def run_max_avg(self, root):
        # avg, num, avg_max, root_max
        res = self.root_subtree_with_max_avg(root)
        return res[0], res[1], res[2], res[3].val

    def root_subtree_with_max_avg(self, root):
        # divide-conquer, from bottom to up
        res = [None, 0, 0, 0]
        avg_left = 0
        num_left = 0
        avg_right = 0
        num_right = 0
        avg_left_max = 0
        avg_right_max = 0
        avg_left_root = None
        avg_right_root = None

        if root:
            res[0] = root.val
            res[1] = 1
        else:
            return

        if not root.left and not root.right:
            res[2] = res[0]
            res[3] = root
            return res

        if root.left:
            avg_left, num_left, avg_left_max, avg_left_root = self.root_subtree_with_max_avg(root.left)

        if root.right:
            avg_right, num_right, avg_right_max, avg_right_root = self.root_subtree_with_max_avg(root.right)

        print('left and right')
        print(avg_left, num_left, avg_left_max, avg_left_root)
        print(avg_right, num_right, avg_right_max, avg_right_root)

        num = res[1] + num_left + num_right
        avg = (res[0] + avg_left * num_left + avg_right * num_right) / num

        res[0] = avg
        res[1] = num
        if res[0] > avg_left_max:
            if res[0] > avg_right_max:
                res[2] = res[0]
                res[3] = root
            else:
                res[2] = avg_right_max
                res[3] = avg_right_root
        else:
            if avg_left_max > avg_right_max:
                res[2] = avg_left_max
                res[3] = avg_left_root
            else:
                res[2] = avg_right_max
                res[3] = avg_right_root

        print('avg', avg, res)
        return res
```
##### lowest common ancestor (最近公共祖先)
```py
    def run_lowest_common_ancestor(self, root, node1, node2):
        # bottom-up
        if not root:
            return None

        # verify that if both are tree nodes
        node = [root]
        node_val = [root.val]
        i = 0
        while i < len(node):
            if node[i].left:
                node.append(node[i].left)
                node_val.append(node[i].left.val)
            if node[i].right:
                node.append(node[i].right)
                node_val.append(node[i].right.val)
            i += 1

        if node1 not in node_val or node2 not in node_val:
            return None

        result = self.lowest_common_ancestor(root, node1, node2)
        return result

    def lowest_common_ancestor(self, root, node1, node2):
        if root.val == node1 or root.val == node2:
            return root

        else:
            # the main logic!
            res_left = None
            res_right = None

            if root.left:
                res_left = self.lowest_common_ancestor(root.left, node1, node2)
            if root.right:
                res_right = self.lowest_common_ancestor(root.right, node1, node2)

            if res_left:
                if res_right:
                    return root
                else:
                    return res_left
            else:
                if res_right:
                    return res_right
                else:
                    return None
```
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