#### 算法
1. 递归
    1. 递归操作模板
    2. 递归操作的核心思想
2. 分治
    1. 分治解决问题的思想
    2. 分治与递归的区别于联系
3. 回溯
    1. 回溯解决问题的思想
    2. 回溯与分治的区别于联系
#### 算法题
1. 爬楼梯（阿里巴巴、腾讯、字节跳动在半年内面试常考）
```java

```

2. 括号生成 (字节跳动在半年内面试中考过)
```java
// 第一次，看完视频课程后直接写出，基本思想是 回溯
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        recursionGeneratePaenthesis(0, 0, n, "", result);
        return result;
    }

    private void recursionGeneratePaenthesis(int left, int right, int n, String currentItem, List<String> result) {
        // 结束条件
        if (left == n && right == n) {
            result.add(currentItem);
            return;
        }

        // 处理逻辑
        if (left < n) {
            recursionGeneratePaenthesis(left + 1, right, n, currentItem + "(", result);
        }
        if (left > right ) {
            recursionGeneratePaenthesis(left, right + 1, n, currentItem + ")", result);
        }

        // 清理现场
    }
}
// 第二次
```

3. 翻转二叉树 (谷歌、字节跳动、Facebook 在半年内面试中考过)
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        recusionInvert(root);
        return root;
    }

    private void recusionInvert(TreeNode treeNode) {
        if (treeNode.left == null && treeNode.right == null) {
            return;
        }

        // 交换逻辑
        TreeNode temp = treeNode.left;
        treeNode.left = treeNode.right;
        treeNode.right = temp;

        if (treeNode.left != null)
            recusionInvert(treeNode.left);
        if (treeNode.right != null)
            recusionInvert(treeNode.right);

    }

    // 第一次提交解题思路
    // 递归方程，深度遍历，遇到存在子节点，就执行旋转，结束条件是，没有子节点
}
```
4. 验证二叉搜索树（亚马逊、微软、Facebook 在半年内面试中考过）
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        Boolean result = Boolean.TRUE;
        recusionValid(root, result);
        return result;
    }

    private void recusionValid(TreeNode treeNode, Boolean result) {
        // 结束条件
        if (treeNode == null || ( treeNode.left == null && treeNode.right == null ) ) {
            return;
        }

        // 处理逻辑，左子树
        if (treeNode.left != null) {
            // 分支结束条件
            if (treeNode.left.val > treeNode.val) {
                result = false;
                return;
            }
            recusionValid(treeNode.left, result);
        }

        // 处理右子树
        if (treeNode.right != null) {
            // 分支结束条件
            if (treeNode.right.val < treeNode.val) {
                result = false;
                return;
            }
            recusionValid(treeNode.right, result);
        }
    }
}
```
5. 二叉树的最大深度（亚马逊、微软、字节跳动在半年内面试中考过）
```java
class Solution {
    public int maxDepth(TreeNode root) {
        int maxDepth = 0;
        recusion(Arrays.asList(root), maxDepth);
        return maxDepth;
    }

    private void recusion(List<TreeNode> levelNodes, int maxDepth) {
        if (levelNodes.isEmpty()) {
            return;
        }

        List<TreeNode> currentLevelChildren = new ArrayList<>();
        for (TreeNode treeNode : levelNodes) {
            if (treeNode.left != null) {
                currentLevelChildren.add(treeNode.left);
            }

            if (treeNode.right != null) {
                currentLevelChildren.add(treeNode.right);
            }
        }

        recusion(currentLevelChildren, maxDepth + 1);
    }
}
```

6. 二叉树的最小深度（Facebook、字节跳动、谷歌在半年内面试中考过）
```java
class Solution {
    public int minDepth(TreeNode root) {
        int minDepth = 0;
        recursion(root, minDepth, 0);
        return minDepth;
    }

    private void recursion(TreeNode treeNode, int minDepth, int currentDepth) {
        if (treeNode == null || (treeNode.left == null && treeNode.right == null)) {
            if (currentDepth < minDepth) {
                minDepth = currentDepth;
            }
            return;
        }

        if (treeNode.left != null) {
            recursion(treeNode.left, minDepth, currentDepth + 1);
        }

        if (treeNode.right != null) {
            recursion(treeNode.right, minDepth, currentDepth + 1);
        }
    }
}
```
7. 二叉树的序列化与反序列化（Facebook、亚马逊在半年内面试常考）
```java

```

8. 二叉树的最近公共祖先（Facebook 在半年内面试常考）
```java

```

9. 从前序与中序遍历序列构造二叉树（字节跳动、亚马逊、微软在半年内面试中考过）
```java

```

10. 组合（微软、亚马逊、谷歌在半年内面试中考过）
```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        recursion(k, n, 0, new Stack<>(), result);
        return result;
    }

    private void recursion(int k, int n,  int cusor, Stack<Integer> value, List<List<Integer>> result) {
        // 结束条件
        if (k == value.size()) {
            result.add(new ArrayList<>(value));
            return;
        }

        for (int i = cusor + 1; i <= n; i++) {
            value.add(i);
            recursion(k, n, cusor + 1, value, result);
            value.pop();
            cusor = cusor + 1;
        }
    }

    // 解题思路
    // 2层遍历，过滤重复
}
```

11. 全排列（字节跳动在半年内面试常考）
```java

```

12. 全排列 II （亚马逊、字节跳动、Facebook 在半年内面试中考过）
```java

```


13. Pow(x, n) （Facebook 在半年内面试常考）
```java

```
14. 子集（Facebook、字节跳动、亚马逊在半年内面试中考过）
```java

```
15. 多数元素 （亚马逊、字节跳动、Facebook 在半年内面试中考过）
```java
// 第一次求解，使用map直接处理
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> itemCount = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (itemCount.containsKey(nums[i])) {
                itemCount.put(nums[i], itemCount.get(nums[i]) + 1);
            } else {
                itemCount.put(nums[i], 1);
            }
        }

        for (int key: itemCount.keySet()) {
            if (itemCount.get(key) > nums.length / 2) {
                return key;
            }
        }
        return 0;
    }
}
```
16. 电话号码的字母组合（亚马逊在半年内面试常考）
```java
class Solution {
    public List<String> letterCombinations(String digits) {
        HashMap<Integer, List<String>> numberCharMap = generateMap();
        List<String> result = new ArrayList<>();
        recursion(digits, 0, "", result, numberCharMap);
        return result;
    }


    private void recursion(String digits, int cursor, String current, List<String> result, HashMap<Integer, List<String>> cons) {
        // 结束条件 = 遍历至最后一个数字
        if (cursor == digits.length()) {
            if (current != "") {
                result.add(current);
            }
            return;
        }
        // 处理逻辑 - 找到当前数字对应的字符列表，迭代每一个字符，同时游标+1
        List<String> targetStr = cons.get( Integer.valueOf( digits.toCharArray()[cursor] + "") );
        for (String itemStr: targetStr) {
            recursion(digits, cursor + 1, current + itemStr, result, cons);
        }
    }

    private HashMap<Integer, List<String>> generateMap() {
        HashMap<Integer, List<String>> numberCharMap = new HashMap<>();
        numberCharMap.put(2, Arrays.asList("a", "b", "c"));
        numberCharMap.put(3, Arrays.asList("d", "e", "f"));
        numberCharMap.put(4, Arrays.asList("g", "h", "i"));
        numberCharMap.put(5, Arrays.asList("j", "k", "l"));
        numberCharMap.put(6, Arrays.asList("m", "n", "o"));
        numberCharMap.put(7, Arrays.asList("p", "q", "r", "s"));
        numberCharMap.put(8, Arrays.asList("t", "u", "v"));
        numberCharMap.put(9, Arrays.asList("w", "x", "y", "z"));
        return numberCharMap;
    }

    // 解题思路
    // 首先构造一个 hashmap key 为 2 - 9  value  为list 包含对应的字符 examp  2 -> [ 'a', 'b', 'c']
    // bfs，深度优先进行递归
}
```
17. N 皇后（字节跳动、苹果、谷歌在半年内面试中考过）
```java

```

#### 总结
1. 回溯算法是递归的升级版，递归更多的关注与重复子问题，回溯会依据当前状态判断是否进行下一步操作
2. 递归和回溯以及分治都是对问题进行函数化，转化为子问题，同时通过条件进行更进一步的处理