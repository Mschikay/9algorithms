# Subsets 输出集合的所有子集

在逻辑之前，先判断参数是不是空。

这道题可以用DFS和BFS做，但是目的是考察DFS

#### DFS
常见的场景：return all posible results

排序就可以去重

##### 递归三要素：

1. 递归的定义
2. 递归的拆解
3. 递归的出口


```py
import copy
from pprint import pprint


class Solution:
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if nums is None:
            return None

        if nums == []:
            return []

        result = [[]]
        return self.recursive(nums, [], 0, result)

    def recursive(self, nums, subset, offset, result):
        for n in range(offset, len(nums)):
            result.append(subset+[nums[n]])
            self.recursive(nums, subset+[nums[n]], n + 1, result)

        return result

    # 非递归 这应该不能算DFS。。
    def subsets2(self, numbs):
        result = [[]]
        for num in nums:
            result += [i + [num] for i in result]
        return result

                    
        
            
        
```

#### BFS
最短路、连通性、图的遍历；一般不用来“求出所有可能解”。因为太耗费空间

#### 同类型题
排列组合的模版适用于所有搜索问题
##### Permutation

```py
def getPermutation():
    numbers = []
    for i in range(1, n + 1):
        numbers.append(i)

    perm = [[1]]
    for i in range(1, len(nums)):
        n = nums[i]
        new_perm = []
        for p in perm:
            l_perm = len(p)
            for j in range(l_perm+1):
                new_perm.append(p[:j] + [n] + p[j:l_perm])
        perm = new_perm
    return perm

```

unique Permutations

##### combination sum
```py
class Solution:
    def combinationSum(self, candidates, target):
        res = []
        candidates.sort()
        self.dfs(candidates, target, 0, [], res)
        return res

    def dfs(self, nums, target, index, path, res):
        if target < 0:
            return  # backtracking
        if target == 0:
            res.append(path)
            return 
        for i in range(index, len(nums)):
            self.dfs(nums, target-nums[i], i, path+[nums[i]], res)
```
letter combination of a phone number

palindrome partitioning

##### restore ip address
```py
# My solution,
# Runtime: 568 ms, faster than 0.94% of Python3 online submissions for Restore IP Addresses.
# ……= =
import copy

class Solution:
    def restoreIpAddresses(self, str):
        """
        :type s: str
        :rtype: List[str]
        """
        result = []

        length = len(str)
        if length < 4:
            return result

        for i in range(1, len(str)):
            res = []
            self.dfs(len(str), str, i, 3, '', res)
            result += res

        return list(set(result))

    def dfs(self, length, s, i, times, path, res):
        if times >= 0:
            if s and int(s[0:i]) <= 255:
                path += str(int(s[0:i]))+'.'
            else:
                return

        else:
            if s == '':
                if len(path)-4 == length:
                    res.append(path[:-1])
            return

        for j in range(1, len(s[i:])+2):
            new_times = copy.deepcopy(times) - 1
            new_path = copy.deepcopy(path)
            self.dfs(length, s[i:], j, new_times, new_path, res)
```

```py
# a more sound solution
def restoreIpAddresses(self, s):
    res = []
    self.dfs(s, 0, "", res)
    return res
    
def dfs(self, s, index, path, res):
    if index == 4:
        if not s:
            res.append(path[:-1])
        return # backtracking
    for i in xrange(1, 4):
        # the digits we choose should no more than the length of s
        if i <= len(s):
            #choose one digit
            if i == 1: 
                self.dfs(s[i:], index+1, path+s[:i]+".", res)
            #choose two digits, the first one should not be "0"
            elif i == 2 and s[0] != "0": 
                self.dfs(s[i:], index+1, path+s[:i]+".", res)
            #choose three digits, the first one should not be "0", and should less than 256
            elif i == 3 and s[0] != "0" and int(s[:3]) <= 255:
                self.dfs(s[i:], index+1, path+s[:i]+".", res)
```


