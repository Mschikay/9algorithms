# Binary Search

如果超时，一定是因为出现了死循环

注意循环的结束条件，否则死循环/漏解

二分法的思想是缩小空间，不一定while循环后，一定只剩一个空间了。
 

```py
if not num:
    return None

start = 0
end = len(num) - 1
while(start + 1 < end):
    mid = (start+end)/2
    # 因为start+end可能会越界，所以可以如下：
    # mid = start+(end-start)/2
    if num[mid] == target:
        end = mid
    
    elif num[mid] > target:
        end = mid - 1
        # or: end = mid
    elif num[mid] < target:
        start = mid + 1
        # or: start = mid

if num[start] == target:
    return mid
if num[start] == target:
    return mid

return None
```
### 典型题
#### Search In a Big Sorted Array
#### Find Minimum in Rotated Sorted Array
#### Search in a 2D Matrix
#### Maximum number in a mountain sequence
#### Find Peak Element
#### 返回rotate array中某一个数的位置

```py
# 考虑start mid end 之间的关系 再分类讨论
class Solution:
    def find(self, l, n):
        if not l:
            return None

        start = 0
        end = len(l)-1

        while start + 1 < end:
            mid = start+(end-start)//2

            if n > l[mid]:
                if l[mid] < l[end] < l[start]:
                    if n > l[end]:
                        end = mid
                    else:
                        start = mid
                else:
                    start = mid

            elif n < l[mid]:
                if l[end] < l[start] < l[mid]:
                    if n > l[start]:
                        end = mid
                    else:
                        start = mid
                else:
                    end = mid

            elif n == l[mid]:
                return mid

        if n == l[start]:
            return start

        if n == l[end]:
            return end

        return None
```

# 时间复杂度

| 时间（O） | 典型 |
|---|---|
| 1 | 极少 |
| logn | 二分法 |
| 根号下n | 分解质因数 |
| n | 高频 |
| nlogn | 排序 |
| n平方 | 数组、枚举、动态规划 |
| n立方 | 数组、枚举、动态规划 |
| 2^n | 与组合有关的搜索|
| n! | 与排列有关的搜索 |

一般的，答案的个数是时间复杂度的下限

# Recursive or not
1. 是否要求
2. 不用Recursion是否会变得很复杂
3. Recursion的深度是否会很深
4. 题目的考点是Recursion VS Non-Recursion 还是考是否会Recursion

