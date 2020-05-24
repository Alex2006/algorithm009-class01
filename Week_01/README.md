学习笔记

#### 思路
1. leetcode如何使用
    1. 五遍学习（1. 看代码 2. 一个思路实现代码  3. 优化当前思路代码  4. ）
    2. 思路整理(超过20分钟，没有思路，就迅速放弃)
    3. 刷题方法(1. 首先提出思路 2. 实现思路  3. 阅读更好的思路 4. 实现最好的思路)

2. 算法的时间和空间复杂度
    1. 关注(O(1), O(logn), O(n), O(nlogn), 0(n2))


#### 数据结构
1. 数组
2. 链表
3. 跳表
4. 栈
5. 队列
6. 优先队列
7. 双端队列

#### 代码
1.移动零(https://leetcode-cn.com/problems/move-zeroes/)

```java
// 第一遍参考答案逻辑，写出
class Solution {
    public void moveZeroes(int[] nums) {
        int zeorCusor = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[zeorCusor++]=nums[i];
            }
        }

        for(int i = zeorCusor; i < nums.length; i++ ) {
            nums[i] = 0;
        }
    }
}
```

2.盛最多水的容器()
```java
// 看视频，直接写出比较暴力的解法
class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                max = Math.max(max, calcMaxArea(i, height[i], j, height[j]));
            }
        }
        return max;
    }

    private int calcMaxArea(int i, int iValue, int j, int jValue) {
        return (Math.max(i, j) - Math.min(i, j)) * Math.min(iValue, jValue);
    }
}
```


3.爬楼梯()
```java
class Solution {
    // 第一次提交未使用cache，造成执行超时
    public int climbStairs(int n) {
        int[] cache = new int[n];
        if (n == 1) {
            return 1;
        }

        if ( n == 2) {
            return 2;
        }

        cache[0] = 1;
        cache[1] = 2;

        return getCacheCalc(n -1, cache) + getCacheCalc(n - 2, cache);
    }

    public int getCacheCalc(int n, int[] cache) {
        if (cache[ n -1 ] == 0) {
            cache[ n -1 ] = getCacheCalc( n -1, cache) + getCacheCalc( n - 2, cache);
        }
        return cache[ n - 1];
    }
}
```


4.三数之和
```java
class Solution {
    // 当前算法超时，需要进一步优化计算时间
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        
        Set<String> resultItme = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k =  j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0 ) {
                        String stringValue = new StringBuilder().append(nums[i]).append(nums[j]).append(nums[k]).toString();
                        if (resultItme.contains(stringValue)) {
                            continue;
                        }
                        List<Integer> item = Arrays.asList(nums[i], nums[j], nums[k]);
                        result.add(item);
                        resultItme.add(stirngValue);
                    }
                }
            }
        }
        return result;
    }
}
```

5.两数之和
```java
class Solution {
    // 较简单，双重遍历，得到答案，未进行较深入计算
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if(nums[i] + nums[j] == target) {
                    result[0] = i;
                    result[1] = j;
                }
            }
        }
        return result;
    }
}

```

6.旋转数组
```java

```

7.两两交换链表中的节点
```java

```



8.环形链表
```java

```

9.环形链表 II
```java

```

10.K 个一组翻转链表
```java

```

11.有效的括号
```java

```

12.最小栈
```java


```


13.柱状图中最大的矩形
```java


```


14.滑动窗口最大值
```java




```


15. 接雨水
```java

```


16.删除排序数组中的重复项
```java

```

17.合并两个有序链表
```java

```

#### 总结
1. 用 add first 或 add last 这套新的 API 改写 Deque 的代码
```java

```
2. 分析 Queue 和 Priority Queue 的源码
```java

```
3. 设计循环双端队列
```java

```
