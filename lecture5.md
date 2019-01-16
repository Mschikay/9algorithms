# DFS

## problems
### all kinds of combination sum (leetcode)
#### leetcode 216.
```py
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """
        result = []

        def dfs(num, iterK, remainN, path):
            if iterK == k:
                if not remainN:
                    result.append(path)
                return

            for i in range(num, 10):
                if remainN < i:
                    break
                # 这一步在递归的同时，也已经把i考虑完了
                dfs(i + 1, iterK + 1, remainN - i, path + [i])
            return

        dfs(1, 0, n, [])
        return result
```
```py
# a sounder solution
def combinationSum3(self, k, n):
    res = []
    self.dfs(xrange(1,10), k, n, 0, [], res)
    return res
    
def dfs(self, nums, k, n, index, path, res):
    if k < 0 or n < 0: # backtracking 
        return 
    if k == 0 and n == 0: 
        res.append(path)
    for i in xrange(index, len(nums)):
        self.dfs(nums, k-1, n-nums[i], i+1, path+[nums[i]], res)
```
#### leetcode 40.
```py
# my solution
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        r = []

        def dfs(startIndex, remainTarget, result):
            if remainTarget == 0:
                # eliminate duplicate combination
                if result not in r:
                    r.append(result)
            elif remainTarget < 0:
                pass
            else:
                for i in range(startIndex + 1, len(candidates)):
                    if remainTarget - candidates[i] < 0:
                        break
                    dfs(i, remainTarget - candidates[i], result + [candidates[i]])
            return

        # sort first to make some combination's elements list the same way
        # like: 1, 1, 7 and 1, 1, 7. not 1, 7, 1, etc
        candidates.sort()
        dfs(-1, target, [])

        print(r)
        return r
```
```py
# a sounder solution online:
class Solution(object):
    def combinationSum2(self, candidates, target):

        # Sorting is really helpful, se we can avoid over counting easily
        candidates.sort()
        result = []
        self.combine_sum_2(candidates, 0, [], result, target)
        print(result)
        return result

    def combine_sum_2(self, nums, start, path, result, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        # Base case: if the sum of the path satisfies the target, we will consider
        # it as a solution, and stop there
        if not target:
            result.append(path)
            return

        for i in range(start, len(nums)):
            # Very important here! We don't use `i > 0` because we always want
            # to count the first element in this recursive step even if it is the same
            # as one before. To avoid overcounting, we just ignore the duplicates
            # after the first element.
            if i > start and nums[i] == nums[i - 1]:
                continue

            # If the current element is bigger than the assigned target, there is
            # no need to keep searching, since all the numbers are positive
            if nums[i] > target:
                break

            # We change the start to `i + 1` because one element only could
            # be used once
            self.combine_sum_2(nums, i + 1, path + [nums[i]],
                               result, target - nums[i])
```
#### leetcode 39
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


if __name__ == "__main__":
    s = Solution()
    print(s.combinationSum([1, 2, 3, 6, 4], 7))
```