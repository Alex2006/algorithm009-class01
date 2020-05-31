#### 数据结构
1. 哈希表
    1. 概念
        1. 二维数据结构，通过对数据进行生成检索key，方式提高查询速度
        2. 普遍采用拉链法，数组+链表来存储数据
    2. Java实现
        1. HashMap
            1. 线程不安全，数据在1.8前使用链表，1.8以后使用红黑树 or 链表
            2. 不安全的原因：执行扩容操作的期间，使用头插法，将当前元素插入到新数组的头结点
                ```java
                void transfer(Entry[] newTable, boolean rehash) {
                    int newCapacity = newTable.length;
                    for (Entry<K,V> e : table) {
                        while(null != e) {
                            Entry<K,V> next = e.next;
                            if (rehash) {
                                e.hash = null == e.key ? 0 : hash(e.key);
                            }
                            int i = indexFor(e.hash, newCapacity);
                            e.next = newTable[i];
                            newTable[i] = e;
                            e = next;
                        }
                    }
                }
               // 问题原因简述：
               // 线程1，执行至 newTable[i] = e; 后切换上线文，线程2，完成transfer后，在回到当前上下文
               // 线程1继续执行时，获取到已经被移动的元素，由于元素的前后关系，造成环，继而引发cpu使用上升
                ```
            3. getNode逻辑
                1. 对需要获取的key进行hash运算
                2. 查找hash后的key所在的数组位置进行访问
                3. 如果 -> null，则返回空
                4. 如果 -> 碰撞，则遍历整个链表，直至找到对应的元素的key
            4. putNode逻辑
                1. 对需要存储的key进行hash运算
                2. 查找hash后的key所在数据位置进行访问
                3. 如果 -> null, 则将改元素放入
                4. 如果 -> hash碰撞，则将数据插入
                    1. 如果当前不出发扩容，则直接插入链表尾部
                    2. 如果发生扩容,(扩容条件 当前元素数量 > 当前容量 * 负载因子)
            5. 1.7与1.8的差异
                1. 1.8修复了1.7中拷贝数据造成的线程安全问题，采用尾插法
                2. 1.8中hashmap线程依旧不安全，不安全的原因 -> put元素时，如果没有发生hash碰撞，则直接插入元素，多线程环境下，会覆盖数据
                3. 1.8中的链表结构改为红黑树和链表并存，(通过阈值进行处理，如果链表元素大于阈值，进行升维，转红黑树)
                4. 1.8中查找操作修正(引入红黑树后，遍历元素逻辑发生变更)
                5. 查找效率从1.7遍历链表的O(n)到红黑树O(logn)
        2. currentHashMap
            1. 如何解决HashMap的线程不安全问题，对比HashTable优势在哪里
                1. HashTable使用Synchronized，对put, get, remove进行了并发控制，所以线程安全，但开销较大
                2. CurrentHashMap使用分段锁实现
            2. CurrentHashMap如何实现线程安全
                1. 1.7中实现
                    1. 数据结构 (数组 + segment(实现了ReentrantLock) + 链表)
                    2. 定位元素需要进行2次hash ( 第一次定位 segment，第二次定位链表头部)
                2. 1.8中实现
                    1. 数据结构 (数组 + (链表 or 红黑树))
                    2. 与1.8的hashmap一直，仅仅通过cas和volatile保证了线程安全
        2. HashSet
            1. 基于HashMap，value存放一个空Object
            2. 线程不安全,不保证插入顺序
2. 二叉树
    1. 概念
        1. 存在父亲节点和子节点的数据结构
        2. 有且仅有1个根节点
        3. 节点拥有子节点的个数为度，拥有2个节点，就是二叉树
        4. 二叉树的第i层，最多有2(i-1)个节点
        5. 标准的二叉树的声明如下：
        ```java
          public class TreeNode {
              int val;
              TreeNode left;
              TreeNode right;
              TreeNode(int x) { val = x; }
          }
        ```
       6. 二叉树的遍历，参考下面算法实现，递归较为简单    
3. 堆
    1. 概念
        1. 可以快速找到一直对书中最大或者最小值的数据结构
        2. 根节点为最大值 -> 大顶堆  根节点为最小值 -> 小顶堆
        3. 常见操作
            1. 查找最大值(最小值) O(1)
            2. 删除最大值(最小值) O(logn)
            3. 插入元素 O(logn) or O(1)
    2. 常见对的结构
        1 二叉堆
            1. 数据结构 - 数组
            2. 时间复杂度
                1. 查找最大值(最小值) O(1)
                2. 删除最大值(最小值) O(logn)
                3. 插入元素 O(logn) or O(1)
    3. java实现
        1. BinaryHeap
        1. priority_queue
            1. PriorityQueue是基于优先堆的一个无界队列，这个优先队列中的元素可以默认自然排序或者通过提供的Comparator（比较器）在队列实例化的时排序。要求使用Java Comparable和Comparator接口给对象排序，并且在排序时会按照优先级处理其中的元素。
            2. 接口操作
                ```java
                   //插入一个元素，不成功会抛出异常
                   public boolean add(E e) { return offer(e);}
                   // 插入一个元素，不能被立即执行的情况下会返回一个特殊的值（true 或者 false）
                    public boolean add(E e) {
                           return offer(e);
                       }
                   // remove：删除一个元素，如果不成功会返回false。
                     public boolean remove(Object o) {
                         int i = indexOf(o);
                         if (i == -1)
                             return false;
                         else {
                             removeAt(i);
                             return true;
                         }
                     }
                   // poll：删除一个元素，并返回删除的元素
                    public E poll() {
                        if (size == 0)
                            return null;
                        int s = --size;
                        modCount++;
                        E result = (E) queue[0];
                        E x = (E) queue[s];
                        queue[s] = null;
                        if (s != 0)
                            siftDown(0, x);
                        return result;
                    }
                   // peek：查询队顶元素
                   public E peek() {
                       return (size == 0) ? null : (E) queue[0];
                   }                
                ```

4. 图
    1. 概念
        1. 邻接矩阵
    2. 常用算法
        1. DFS
        2. BFS
        3. 最短路径(迪杰斯特拉算法)
        4. 最小生成树
        5. 连通图个数

#### 算法题
1. 有效的字母异位词 (https://leetcode-cn.com/problems/valid-anagram/description/)
```java
// 第一遍题目理解错误，仅仅按照比较2个字符进行判断是否异位置
class Solution {
    // 暴力破解，按照没2个遍历数据，然后比对当前和下一个元素是否异位
    // 需要注意，边界条件
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
           return false;
       }

       int[] hash = new int[26];
        for (int i = 0; i < s.length(); i++) {
            hash[s.charAt(i) - 'a']++;
            hash[t.charAt(i) - 'a']--;
        }

        for (int i = 0; i < hash.length; i++) {
            if(hash[i] != 0) {
                return false;
            }
        }
        return true;

//        for (int i = 0; i < s.length(); i = i + 2) {
//            if(i  == s.length() - 1) {
//                return s.charAt(i) == t.charAt(i);
//            }
//            if ( (s.charAt(i) != t.charAt(i) || s.charAt(i + 1 ) != t.charAt(i + 1))
//                    && (s.charAt( i + 1) != t.charAt(i) || s.charAt(i) != t.charAt(i + 1))) {
//                return false;
//            }
//        }
//        return true;

    }
}


```

2. 字母异位词分组 (https://leetcode-cn.com/problems/group-anagrams/)
```java

```


3. 二叉树的中序遍历 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        iterTreeNode(root, result);
        return result;
    }

    private void iterTreeNode(TreeNode treeNode, List<Integer> result) {
        if (treeNode != null) {
            iterTreeNode(treeNode.left, result);
            result.add(treeNode.val);
            iterTreeNode(treeNode.right, result);
        }
    }
}
```

4. 二叉树的前序遍历 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        iterTreeNode(root, result);
        return result;
    }

    private void iterTreeNode(TreeNode treeNode, List<Integer> result) {
        if (treeNode != null) {
            result.add(treeNode.val);
            iterTreeNode(treeNode.left, result);
            iterTreeNode(treeNode.right, result);
        }
    }
}
```



5. N叉树的后序遍历 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    public List<Integer> postorder(Node root) {
        List<Integer> result = new ArrayList<>();
        iterTreeNode(root, result);
        return result;
    }

    private void iterTreeNode(Node treeNode, List<Integer> result) {
        if (treeNode != null) {
            if(treeNode.children.size() > 0) {
                for (int i = 0; i < treeNode.children.size(); i++) {
                    iterTreeNode(treeNode.children.get(i), result);
                }
            }
            result.add(treeNode.val);
        }
    }
}
```


6. N叉树的前序遍历 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> result = new ArrayList<>();
        iterTreeNode(root, result);
        return result;
    }

    private void iterTreeNode(Node treeNode, List<Integer> result) {
        if (treeNode != null) {
            result.add(treeNode.val);
            if(treeNode.children.size() > 0) {
                for (int i = 0; i < treeNode.children.size(); i++) {
                    iterTreeNode(treeNode.children.get(i), result);
                }
            }
        }
    }
}
```


7. N叉树的层序遍历 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    // 需要进一步优化，超出内存限制
     public List<List<Integer>> levelOrder(Node root) {
            List<List<Integer>> result = new ArrayList<>();
            levelIterTree(Arrays.asList(root), result);
            return result;
        }
        
        private void levelIterTree(List<Node> node, List<List<Integer>> result) {
            if (node != null && node.size() > 0) {
                List<Node> nextLevenNode = new ArrayList<>();
                List<Integer> currentLevelVal = new ArrayList<>();
                for (int i = 0; i < node.size(); i++) {
                    currentLevelVal.add(node.get(i).val);
                    if (node.get(i).children.size() > 0) {
                        nextLevenNode.addAll(node.get(i).children);
                    }
                }
                result.add(currentLevelVal);
                levelIterTree(nextLevenNode, result);
            }
        }
// 标准解法，仅仅使用了一个List来完成每次的创建操作
//    public List<List<Integer>> levelOrder(Node root) {
//        List<List<Integer>> result = new ArrayList<>();
//        if (root == null) return result;
//        Queue<Node> queue = new LinkedList<>();
//        queue.add(root);
//        while (!queue.isEmpty()) {
//            List<Integer> level = new ArrayList<>();
//            int size = queue.size();
//            for (int i = 0; i < size; i++) {
//                Node node = queue.poll();
//                level.add(node.val);
//                queue.addAll(node.children);
//            }
//            result.add(level);
//        }
//        return result;
//    }


}
```


8. 丑数 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java

```


9. 最小的 k 个数 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java：

```

10. 滑动窗口最大值 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java

```


11. 前 K 个高频元素 (https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```java

```


#### 本周二刷题目
1. 两数之和 (https://leetcode-cn.com/problems/two-sum/description/)
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> cache = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int targetValue = target - nums[i];
            if (cache.containsKey(targetValue)) {
                int[] result = new int[2];
                result[0] = i;
                result[1] =  cache.get(targetValue);
                return result;
            }
            cache.put(nums[i], i);
        }
        return new int[0];
    }
}
```

#### 本周总结
1. hashMap平常使用较多，线程问题理解较差，cas和volitile相关概念理解上较为薄弱
2. 二叉树理解还行，递归逻辑比较清晰
3. 图的相关内容需要进一步进行学习