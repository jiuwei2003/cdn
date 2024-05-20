---
title: LeetCode
tags:
  - 算法
categories:
  - 算法
comments: true
abbrlink: 61255
date: 2024-05-08 22:11:36
---

## LeetCode

<!--more-->

## [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

找出数组中重复的数字。
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

**限制：**

```
2 <= n <= 100000
```

**解答：**

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
// 哈希表/Set
//时间复杂度 O(N) ： 遍历数组使用 O(N) ，HashSet 添加与查找元素皆为 O(1)
//空间复杂度 O(N) ： HashSet 占用 O(N) 大小的额外空间。

        // //初始化HashSet集合(没有顺序和不重复)
        //  HashSet<Integer> hash = new HashSet<>();
        //  //遍历数组，集合中有num值代表重复，没有添加到集合中
        //  for(Integer num : nums){
        //      if(hash.contains(num)) return num;
        //      hash.add(num);
        //  }
        //  return -1;
 

// 原地交换
// 时间复杂度 O(N) ： 遍历数组使用 O(N) ，每轮遍历的判断和交换操作使用 O(1) 。
// 空间复杂度 O(1) ： 使用常数复杂度的额外空间
            int i=0;
            while(i<nums.length){
            //下标和对应的值相等，直接i++，向后移一位
            if(nums[i]==i){
                i++;
                continue;
            }
            //如果下标对应的值和这个值所对应的下标的值相等，就说明重复，直接返回nums[i]
            if(nums[i]==nums[nums[i]]) return nums[i];
            //如果不相等，则下标对应的值和这个值所对应的下标值进行交换
            int temp = nums[i];
            nums[i] = nums[temp];
            nums[temp] = temp;
        }
        return -1;
    }
}
```

## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

 

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

 

**限制：**

```
0 <= s 的长度 <= 10000
```

```
class Solution {
    /*
    1.遍历列表 s 中的每个字符 c ：
    当 c 为空格时：向 res 后添加字符串 "%20" ；
    当 c 不为空格时：向 res 后添加字符 c ；
    将列表 res 转化为字符串并返回，时间复杂度O(n)，空间复杂度O(n)
    2.或者直接使用replaceAll(原字符，新字符);
    */
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for (Character c: s.toCharArray()
        ) {
            if (c == ' ') {
                sb.append('%').append('2').append('0');
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
        // return s.replaceAll(" ","%20");
    }
}
```

## [剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

```
0 <= 链表长度 <= 10000
```

![image-20230903124123860](C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20230903124123860.png)

## 704 二分查找

**前提是数组为有序数组**，同时题目还强调**数组中无重复元素**，因为一旦有重复元素，使用二分查找法返回的元素下标可能不是唯一的，这些都是使用二分法的前提条件

区间的定义这就决定了二分法的代码应该如何写，**因为定义target在[left, right]区间，所以有如下两点：**

- while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
- if (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target（已经确定了这个middle不是target），那么接下来要查找的左区间（就不用判断这个值了，判断前一个值就ok）结束下标位置就是 middle - 1

定义 target 是在一个在左闭右开的区间里，也就是[left, right) ，那么二分法的边界处理方式则截然不同。

有如下两点：

- while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
- if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]

```java
class Solution {
    public int search(int[] nums, int target) {
        if(target<nums[0] || target > nums[nums.length-1]){
            return -1;
        }
        int left = 0;
        int right = nums.length-1; //左闭右闭
        while(left<=right) //合法
        {
            int middle = right+(left-right)/2;  //防止数组溢出
            // 中间值大于目标值，取前一段进行比较
            if(nums[middle] > target)
            {
                right = middle-1;
            } 
            // 中间值小于目标值，取后一段进行比较
            else if(nums[middle] < target)
            {
                left = middle + 1;
            }
            // 中间值和目标值相等
            else {
                return middle;
            }
        }
        return -1;

    }
}
```

## 27 移除元素

暴力破解 

- 时间复杂度是O(n^2)

- 空间复杂度：O(1)

https://code-thinking.cdn.bcebos.com/gifs/27.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0-%E6%9A%B4%E5%8A%9B%E8%A7%A3%E6%B3%95.gif

~~~java
class Solution {
    public int removeElement(int[] nums, int val) {
        /*
        *暴力破解
        */
        int size = nums.length;
        for(int i=0;i<size;i++){
            //如果下标对应的值等于目标值，就将后面元素向前移
            if(nums[i]==val){
                for(int j=i+1;j<size;j++){
                    nums[j-1] = nums[j];
                }
                i--;  // 因为下标i以后的数值都向前移动了一位，所以i也向前移动一
                size--;// 此时数组的大小-1
            }
        }
        return size;
    }
}
~~~

双指针法

https://code-thinking.cdn.bcebos.com/gifs/27.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0-%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95.gif

- 时间复杂度 O(n)
- 空间复杂度O(1)
- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

~~~java
class Solution {
    public int removeElement(int[] nums, int val) {
        int size = nums.length;
        int slow=0;
        for(int fast=0;fast<size;fast++){
            if(nums[fast]!=val){ 
                //快指针指向的元素等于目标值，就不能将元素放到慢指针数组里
                //快指针指向的元素不等于目标值，就可以放到慢指针数组里
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
~~~

## 977有序数组的平方

双指针法

- 时间复杂度 O(n)
- 空间复杂度O(n)

数组其实是有序的， 只不过负数平方之后可能成为最大数了。

那么数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间。

此时可以考虑双指针法了，i指向起始位置，j指向终止位置。

定义一个新数组result，和A数组一样的大小，让k指向result数组终止位置。

如果`A[i] * A[i] < A[j] * A[j]` 那么`result[k--] = A[j] * A[j];` 。

如果`A[i] * A[i] >= A[j] * A[j]` 那么`result[k--] = A[i] * A[i];` 。

如动画所示：

https://code-thinking.cdn.bcebos.com/gifs/977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.gif

~~~java
class Solution {
    public int[] sortedSquares(int[] nums) {
         int size = nums.length-1;
         //可以分为两个指针，然后加一个数组   时间复杂度是O(n)
         int left=0;
         int right=size;
         int[] target = new int[size+1];
         int index=target.length-1; //新数组最后一个值
        
         while(left<=right)
         {
             //左边的乘积大于右边，就将左边元素加入到新数组最后一个元素，然后新数组最后一位下标需要往前移一位，最左边的元素下标也需要往前移一位
             if(nums[left]*nums[left]>nums[right]*nums[right])
             {
                 target[index--] = nums[left]*nums[left];
                 left++;
             }
             else 
             {
                 target[index--] = nums[right]*nums[right];
                 right--;
             }
         }
         return target;
    }
}
~~~

暴力破解

- 时间复杂度 O(n+nlog(n))
- 空间复杂度O(1)

~~~java
class Solution {
    public int[] sortedSquares(int[] nums) {
//或者就是循环先给每一个元素平方，然后排序  时间复杂度是O(n+logn)
         for(int i=0;i<nums.length;i++){
            nums[i] = nums[i]*nums[i];
         }
         Arrays.sort(nums);
         return nums;
    }
}
~~~

## 209长度最小的子数组

滑动窗口

首先要思考 如果用一个for循环，那么应该表示 滑动窗口的起始位置，还是终止位置。

如果只用一个for循环来表示 滑动窗口的起始位置，那么如何遍历剩下的终止位置？

此时难免再次陷入 暴力解法的怪圈。

所以 只用一个for循环，那么这个循环的索引，一定是表示 滑动窗口的终止位置。

那么问题来了， 滑动窗口的起始位置如何移动呢？

这里还是以题目中的示例来举例，s=7， 数组是 2，3，1，2，4，3，来看一下查找的过程：

![209.长度最小的子数组](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

最后找到 4，3 是最短距离。

其实从动画中可以发现滑动窗口也可以理解为双指针法的一种！只不过这种解法更像是一个窗口的移动，所以叫做滑动窗口更适合一些。

在本题中实现滑动窗口，主要确定如下三点：

- 窗口内是什么？
- 如何移动窗口的起始位置？
- 如何移动窗口的结束位置？

窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组。

窗口的起始位置如何移动：如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）。

窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。

解题的关键在于 窗口的起始位置如何移动，如图所示：

![leetcode_209](https://code-thinking-1253855093.file.myqcloud.com/pics/20210312160441942.png)

可以发现**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将O(n^2)暴力解法降为O(n)。**

- 时间复杂度 O(n)

~~~java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result=Integer.MAX_VALUE;
        int sum=0;
        int i=0;
        for(int j=0;j<nums.length;j++) //j代表终止位置
        {
            sum+=nums[j];
            while(sum>=target) //知道和大于等于目标值
            {
                //长度为
                int slength = j-i+1; // 取子序列的长度
                result = Math.min(result,slength);
                //然后和需要减去起始位置的元素，因为往前移了一位
                sum -= nums[i++];
            }
        }
        //如果result没有赋值就返回0 否则返回result
        return result==Integer.MAX_VALUE?0:result;
    }
}
~~~

暴力破解

- 时间复杂度为O(n^2)

~~~java
int result=Integer.MAX_VALUE;
    int sum=0;
    for(int i=0;i<nums.length;i++){ //起始位置
    sum = 0;
    for(int j=i;j<nums.length;j++){
        sum+=nums[j];
        if(sum>=target){
            int slength = j-i+1; // 取子序列的长度
            result = Math.min(result,slength);
            break; //找到比他大的元素然后就去i++,找下一个长度
        }
       }
    }
           return result==Integer.MAX_VALUE?0:result;
    }

~~~

## 59螺旋矩阵

而求解本题依然是要坚持循环不变量原则。

模拟顺时针画矩阵的过程:

- 填充上行从左到右
- 填充右列从上到下
- 填充下行从右到左
- 填充左列从下到上

由外向内一圈一圈这么画下去。

这里一圈下来，我们要画每四条边，这四条边怎么画，每画一条边都要坚持一致的左闭右开，或者左开右闭的原则，这样这一圈才能按照统一的规则画下来。

那么我按照左闭右开的原则，来画一圈，大家看一下：

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20220922102236.png)

这里每一种颜色，代表一条边，我们遍历的长度，可以看出每一个拐角处的处理规则，拐角处让给新的一条边来继续画。

这也是坚持了每条边左闭右开的原则。

一些同学做这道题目之所以一直写不好，代码越写越乱。

就是因为在画每一条边的时候，一会左开右闭，一会左闭右闭，一会又来左闭右开，岂能不乱。

代码里处理的原则也是统一的左闭右开。

需要循环几次，可以将行在中间分割两半，一半看能分成几层

~~~~java
class Solution {
    public int[][] generateMatrix(int n) {
        int loop = 0; //循环次数
        int[][] res = new int[n][n];
        int start =0;//开始位置
        int count = 1; //打印数字
        int i,j;
        while(loop++<n/2)
        {
            for(j=start;j<n-loop;j++)
            res[start][j] = count++;
            for(i=start;i<n-loop;i++)
            res[i][j] = count++;     //j=n-loop了 就是第一行的最后一个数
            for(j=j;j>=loop;j--)
            res[i][j] = count++;
            for(i=i;i>=loop;i--)
            res[i][j] = count++;
            start++; //循环之后，start++ 变为1行1列开始
        }
        if(n%2==1) //奇数，就将最后一个数放到最中间
        res[start][start] = count;

        return res;
    }
}
~~~~

## 203.移除链表元素

- **直接使用原来的链表来进行删除操作。**
- **设置一个虚拟头结点在进行删除操作。**

来看第一种操作：直接使用原来的链表来进行移除。

![203_链表删除元素3](https://code-thinking-1253855093.file.myqcloud.com/pics/2021031609544922.png)

移除头结点和移除其他节点的操作是不一样的，因为链表的其他节点都是通过前一个节点来移除当前节点，而头结点没有前一个节点。

所以头结点如何移除呢，其实只要将头结点向后移动一位就可以，这样就从链表中移除了一个头结点。

![203_链表删除元素4](https://code-thinking-1253855093.file.myqcloud.com/pics/20210316095512470.png)

依然别忘将原头结点从内存中删掉。 ![203_链表删除元素5](https://code-thinking-1253855093.file.myqcloud.com/pics/20210316095543775.png)

这样移除了一个头结点，是不是发现，在单链表中移除头结点 和 移除其他节点的操作方式是不一样，其实在写代码的时候也会发现，需要单独写一段逻辑来处理移除头结点的情况。

那么可不可以 以一种统一的逻辑来移除 链表的节点呢。

其实**可以设置一个虚拟头结点**，这样原链表的所有节点就都可以按照统一的方式进行移除了。

来看看如何设置一个虚拟头。依然还是在这个链表中，移除元素1。

![203_链表删除元素6](https://code-thinking-1253855093.file.myqcloud.com/pics/20210316095619221.png)

这里来给链表添加一个虚拟头结点为新的头结点，此时要移除这个旧头结点元素1。

这样是不是就可以使用和移除链表其他节点的方式统一了呢？

来看一下，如何移除元素1 呢，还是熟悉的方式，然后从内存中删除元素1。

最后呢在题目中，return 头结点的时候，别忘了 `return dummyNode->next;`， 这才是新的头结点

- 不使用虚拟头结点

~~~java
/** 
 public class ListNode {
     int val;
     ListNode next;
     ListNode() {}
     ListNode(int val) { this.val = val}
     ListNode(int val,ListNode next) { this.val = val; this.next = next; }
 }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        //先不使用虚拟头结点
        //首先判断待删除元素是不是头结点
        while(head!=null&&head.val==val)  //使用while因为  如果是 (1,1,1,1) 1需要一直删
        {
            head = head.next; //就让头结点往后移
        }
        //然后定义一个节点，用于存储head
        ListNode cur = head; //为什么不定义head->next 因为如果删除第二个元素，就不能找到第一个元素，然后指向下下一个指针
        //第二个开始
        while(cur!=null){
        while(cur.next!=null&&cur.next.val==val)
        {
            cur.next = cur.next.next;
        }
        cur = cur.next;
        }
        return head;
    }
}
~~~

- 使用虚拟头结点

~~~java
/** 
 public class ListNode {
     int val;
     ListNode next;
     ListNode() {}
     ListNode(int val) { this.val = val}
     ListNode(int val,ListNode next) { this.val = val; this.next = next; }
 }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
        return head;
        }
        //使用虚拟头结点
        ListNode pre = new ListNode(-1,head); //定义一个虚拟头节点
// 将虚拟头结点指向head，这样方便后面做删除操作 让虚拟头结点和本身头节点联系起来
        ListNode cur = pre; //不能是pre.next，因为没法删除head
        //第二个开始
        while(cur.next!=null){
        if(cur.next.val==val)
        {
            cur.next = cur.next.next;
        }
        else{
            cur = cur.next;
           }
        }
        return pre.next;  //它的下一个节点是头节点，因为头结点可能会被删除
    }
}
~~~









## **[1379. 找出克隆二叉树中的相同节点](https://leetcode.cn/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/)[](https://leetcode.cn/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/)**

思路：克隆树和原树数据一样，并且元素唯一，不会重复。原树和clone树同时遍历。原树节点等于target的话就返回clone对应节点

~~~java
class Solution {
    public final TreeNode getTargetCopy(final TreeNode original, final TreeNode cloned, final TreeNode target) {
        if (original == null)
            return null;
        // 节点值等于target 返回cloned的节点 原树的值等于target 然后返回clone树的值
        if (original == target)
            return cloned;
        // 然后递归
        TreeNode left = getTargetCopy(original.left,cloned.left, target);
        if (left != null)
            return left; // 去dfs左子树，然后找到后返回

        // 肯定不会为null
        return getTargetCopy(original.right,cloned.right, target);
    }
}
~~~



## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

拆分中序，再拆分后续 左开右闭

~~~java
class Solution {
    //用于存放后序节点
    Map<Integer,Integer> map ;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map = new HashMap<>();
        //将后续节点加入到map里,以便好拿值 方便根据中节点进行分割
        for(int i=0;i<inorder.length;i++) {
            map.put(inorder[i],i);
        }
        //起始值、终止值
        return dfs(inorder,0,inorder.length,postorder,0,postorder.length);
    }

    TreeNode dfs(int[] inorder,int startIn,int endIn, int[] postorder,int startPost,int endPost) {
        //结束条件
        if(startIn>=endIn||startPost>=endPost) { // 不满足左闭右开，说明没有元素，返回空树
            return null;
        }
        //按照左闭右开进行分割
        // 找到后序遍历的最后一个元素在中序遍历中的位置
        //首先拿出后序遍历最后一个元素 然后去中序遍历进行分割
        int rootIndex =  map.get(postorder[endPost-1]); //根节点的位置
        //构造节点
        TreeNode root = new TreeNode(inorder[rootIndex]); //中序遍历里根节点
        //然后记录根节点左侧左子树长度 rootIndex-ednIn
        int leftLength = rootIndex-startIn; //左闭右开

        //然后进行递归 左树 左终止值是根节点的前面那个，因为是左闭右开 后序遍历左子树长度等于startPost+leftLength
        root.left = dfs(inorder,startIn,rootIndex,postorder,startPost,startPost+leftLength); 
        //右子树的起始值就是startpost+leftLength长度
        root.right = dfs(inorder,rootIndex+1,endIn,postorder,startPost+leftLength,endPost-1);

        return root;
    }
}
~~~



## 654.最大二叉树

首先确定参数和返回值 参数是操作的数组，起始值，终止值  返回值是root节点

确定终止的条件：终止值<起始值就数组为null，直接返回空节点

中间过程：首先找出最大值和最大值所在的位置  让最大值作为根节点 用位置确定划分左右子树



**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {

        return constructMaxTree(nums, 0, nums.length);
    }

    TreeNode constructMaxTree(int[] nums, int begin, int end) {
        if (end - begin < 1)
            return null;
        // 等于1就将其作为根节点
        if (end - begin == 1)
            return new TreeNode(nums[begin]);

        // 寻找最大值和索引位置
        int maxValue = nums[begin];
        int maxIndex = begin;
        for (int i = begin + 1; i < end; i++) {
            if (nums[i] > maxValue) {
                maxValue = nums[i];
                maxIndex = i;
            }
        }

        // 去创建根
        TreeNode node = new TreeNode(maxValue);
        // 去创建左子树
        node.left = constructMaxTree(nums, begin, maxIndex);
        node.right = constructMaxTree(nums, maxIndex + 1, end);

        return node;
    }
}
~~~

## 700.二叉搜索树中的搜索

给定二叉搜索树（BST）的根节点 `root` 和一个整数值 `val`。

你需要在 BST 中找到节点值等于 `val` 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `null` 。



思路：

dfs：递归搜索目标节点，遇到直接返回

迭代：因为是二叉搜索树，满足左小右大，小于根节点就向左，大于就向右寻找，找到就返回



**代码**

~~~java
//递归
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {

        if (root == null || root.val == val)
            return root; // 找到或者为null
        TreeNode result = null;
        if (root.val > val) { // 小于向左
            result = searchBST(root.left, val);
        }
        if (root.val < val) // 大于向右
        {
            result = searchBST(root.right, val);
        }
        return result;
    }
}
//迭代
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {

        while(root!=null) {
            if(root.val>val) //小于 
            root= root.left ;
            else if(root.val<val)
            root = root.right;
            else return root;
        }
        return null;
    }
}
~~~





## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

二叉搜索树的中序遍历是升序，我们将节点保存到数组中，然后判断一下数组是否是升序



~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */

// 中序遍历二叉树是升序就是二叉搜索树
class Solution {
    List<Integer> list = new ArrayList<>();

    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;
        isValid(root, list);
        // 然后判断是不是升序
        boolean flag = true;
        for (int i = 1; i < list.size(); i++) {
            // ==重复的也为false
            if (list.get(i - 1) >= list.get(i)) {
                flag = false;
                break;
            }
        }
        return flag;
    }

    // 中序遍历将其放到数组中
    void isValid(TreeNode root, List list) {
        if (root.left != null)
            isValid(root.left, list);
        list.add(root.val);
        if (root.right != null)
            isValid(root.right, list);
    }

}


//可以在中序遍历的时候直接比较当前节点和前一节点的大小值
class Solution {
    TreeNode preNode ;  //当前节点的前一个节点
    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;  //null树也是二叉搜索树
        boolean left = isValidBST(root.left);
        if(!left) return false; //不满足返回false
        //当前节点的值前一个节点的值小于等于 不是升序 返回false
        if(preNode!=null && root.val<=preNode.val) return false;

        //然后更新一下当前节点的值
        preNode = root;
        //再遍历右子树
        return isValidBST(root.right);
    }
}
~~~

# 双指针

## [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

双指针解法

因为数组是有序的，所以可以定义左右指针，相加大于target就right--，相反则left++，相等返回   O(n)

**代码**

~~~java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        // 双指针
        int left = 0, right = numbers.length - 1;
        while (left < right) {
            // 等于就返回下标
            if (numbers[left] + numbers[right] == target)
                return new int[] { left+1, right+1 };
            // 两者相加大于目标值，right--
            else if (numbers[left] + numbers[right] > target)
                right--;
            else
                left++;
        }
        return new int[] { 0 };
    }
}
~~~

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

首先进行排序，然后判断首位是否大于0，对i去重，sum进行上面判断，需要对left和right去重



**代码**

~~~java

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // 存放结果
        List<List<Integer>> resList = new ArrayList<>();
        // 先进行排序
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            // 第一个数大于0就不存在
            if (nums[i] > 0)
                return resList;
            // 对i去重
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0)
                    right--; // 大于0 往右移动
                else if (sum < 0)
                    left++; // 小于0，往左移动
                else {
                    // 加入到集合
                    resList.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    // 对left去重 看是否和后面的那个相等
                    while (right > left && nums[left] == nums[left + 1])
                        left++;
                    // 对right去重 看是否和前面的那个相等
                    while (right > left && nums[right] == nums[right - 1])
                        right--;
                    left++;
                    right--;
                }
            }
        }
        return resList;
    }
}
~~~

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

思路双指针 找最大面积，初始化一个面积，左右指针，计算面积，更新最大面积，左边高度小就往左移，右边高度小就往右移动



**代码**

~~~java
class Solution {
    public int maxArea(int[] height) {
        int maxA = 0; // 初始面积
        int left = 0;
        int right = height.length - 1;

        // 左右未相遇
        while (left < right) {
            int area = Math.min(height[left], height[right]) * (right - left); // 面积就是两者的小的高乘以两者宽度
            maxA = Math.max(area, maxA); // 更新面积
            // 如果右边的小于左边的就将右边往左移
            if (height[right] < height[left])
                right--;
            else
                left++;
        }
        return maxA;
    }
}
~~~



# 滑动窗口

## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

双指针，计算sum的值是否大于target 大于的话则更新长度，将左指针的数剪掉判断是否还大于target，left需要++  在返回时也需要判断返回的长度是不是比初始的长度小于或者等于，满足则可直接返回，不行则返回0

**双指针在满足单调性的时候可以使用**

**代码**

~~~java
class Solution {
    //时间复杂度O(n) 空间复杂度O(1)
    public int minSubArrayLen(int target, int[] nums) {
        int minSub = nums.length+1; //最小的长度
        int sum = 0;
        int left = 0;
        for(int right = 0;right<nums.length;right++) {
            sum += nums[right];
            // //sum>target 满足要求到不满足要求->单调性 满足单调性才可以使用双指针
            // while(sum-nums[left]>=target) {
            //     sum =  sum - nums[left];
            //     left++;
            // }
            // if(sum>=target)  minSub = Math.min(minSub,right-left+1);
            while(sum>=target) {   //为什么不需要判断left<=right 因为相等的情况下减去左指针的数会为0，不满足正整数
                //然后去不断更新子数组的长度
                minSub = Math.min(minSub,right-left+1); //模拟 right==left 代表是一个数，他的长度为1
                //然后减去左指针去判断是否还大于target
                sum -= nums[left];
                left++;
            }
        }
        //ifminSUub小于等于初始长度，然后可以放心返回 else返回0  因为数组所有数加起来都不会大于target，它的最小长度就不会改变，初始的minSub就会大于nums.length
        return minSub <= nums.length ? minSub:0;
    }
}
~~~

## [713. 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)

k<=1 子数组不存在，直接返回0

个数 是右指针减去左指针+1  都是从满足的左指针的下一位开始到右指针 

满足* 然后去掉左边的数 判断是否满足，不满足就去算个数

**代码**

~~~java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        // 假设l r 【l+1,r】 [l+2,r] [r,r] r-l+1 10 5 2 [5,2] [2]满足 2-1+1=2
        if(k<=1) return 0; //因为是严格小于k k=1或者0 数组肯定没有满足的
        int ans = 0; // 返回个数
        int cj = 1; // 乘积
        int l = 0;
        for (int r = 0; r < nums.length; r++) {
            cj *= nums[r];
            while (cj >= k) { // 不满足到满足
                cj /= nums[l]; // 左往右移动
                l += 1;
            }
            ans += r-l+1;
        }
        return ans;
    }
}
~~~



## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

滑动窗口   利用hashSet来判断集合里面是否有元素，有元素的话最左侧集合元素右移 ,没元素的话将其加入  将字符串转为字符数组，用boolean数组来替换hashSet

**代码**

~~~java
class Solution {
    public int lengthOfLongestSubstring(String S) {
        int max = 0; // 最长的长度
        int left = 0;
        // 存放字符出现次数 （可以降低字符串的消耗）
        char[] s = S.toCharArray(); // 转换成 char[] 加快效率（忽略带来的空间消耗）
        //Set<Character> has = new HashSet<>(); 
         boolean[] has = new boolean[128];
        for (int right = 0; right < s.length; right++) {
            char c = s[right];
            // 判断是否有重复的，有的话就不能加，然后left++
            while (has[c]) { //加入c会导致有重复元素
                has[s[left++]] = false; //缩小窗口
            }
            //不重复就加入
            has[c] = true;
            //更新长度
            max = Math.max(max,right-left+1);
        }
        return max;
    }
}



//HashMap
class Solution {
    public int lengthOfLongestSubstring(String S) {
        int max = 0; // 最长的长度
        int left = -1;
        // 存放字符出现次数 （可以降低字符串的消耗）
        char[] s = S.toCharArray(); // 转换成 char[] 加快效率（忽略带来的空间消耗）
        Map<Character, Integer> has = new HashMap<>();    //value保存的是元素的下标
        //  boolean[] has = new boolean[128];
        for (int right = 0; right < s.length; right++) {
            char c = s[right];
            // 判断是否有重复的，有的话就不能加，然后left++
            // while (has[c]) { //加入c会导致有重复元素   
            if(has.containsKey(c)){
                // has[s[left++]] = false; //缩小窗口
                //找到这个重复的然后 更新左子针 更新到这个元素的前面那个元素的下标是刚访问的下标  
                left = Math.max(left,has.get(c));
            }
            //不重复就加入
            // has[c] = true; 
            has.put(c,right);
            //更新长度
            max = Math.max(max,right-left);
        }
        return max;
    }
}
~~~

# 二分查找

解题思路：二分查找low_bound 【时间复杂度O(lgn),n是数组长度】

\- 核心要素
    \- 注意区间开闭，三种都可以
    \- 循环结束条件：当前区间内没有元素
    \- 下一次二分查找区间：不能再查找(区间不包含)mid，防止死循环
    \- 返回值：大于等于target的第一个下标（注意循环不变量）

\- 有序数组中二分查找的四种类型（下面的转换仅适用于数组中都是整数）

​    1. 第一个大于等于x的下标： low_bound(x)

​    2. 第一个大于x的下标：可以转换为`第一个大于等于 x+1 的下标` ，low_bound(x+1)

​    3. 最后一个一个小于x的下标：可以转换为`第一个大于等于 x 的下标` 的`左边位置`, low_bound(x) - 1;

    4. 最后一个小于等于x的下标：可以转换为`第一个大于等于 x+1 的下标` 的 `左边位置`, low_bound(x+1) - 1;



## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

​		//第一个出现的位置 就是>=target的位置
​        //第一个大于x的下标 >=(target+1)
​        //最后一个小于x的下标就是>=(target)-1
​        //最后一个出现的位置 就是 >=(target+1)-1的位置



闭区间 mid + 1 mid -1

左闭右开 mid+1 mid

左开右闭 start=-1 mid mid+1



循环不变量 最后的时候
            // nums[left-1] < target
            // nums[right+1] >= target





“红蓝染色法”关键：

1. right 左移使右侧变蓝 (判断条件为 true )
2. left 右移使左侧变红 (判断条件为 false )
3. 故确定二分处 ( mid ) 的染色条件是关键  闭区间，和判断条件相同的话就是什么颜色

**代码**

~~~java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        //第一个出现的位置 就是>=target的位置
        int start = midBound(nums,target);
        //不存在
        if(start==nums.length || nums[start]!=target) return new int[]{-1,-1};
        //第一个大于x的下标 >=(target+1)
        //最后一个小于x的下标就是>=(target)-1
        //最后一个出现的位置 就是 >=(target+1)-1的位置
        int end = midBound(nums,target+1)-1; 
        return new int[]{start,end};
    }

    int midBound(int[] nums, int target) {
        //闭区间[start, end]
        int start = 0;
        int end = nums.length-1;
        while(start<=end) {
            // 循环不变量： 最后的时候
            // nums[left-1] < target
            // nums[right+1] >= target
            int mid = start+(end-start)/2;
            if(nums[mid]<target) {  //[mid+1,end]
            start = mid+1; //因为是闭区间，所以mid已经判断过，不满足条件
            } else {
                end = mid-1;
            }
        }
        return start;
    }
}
~~~

## [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

根据红黑染色法，蓝色代表峰顶及其峰顶右侧的值，数组中一定有封顶存在，所以最后一个肯定是蓝色，红色代表封顶左侧的值

比较 mid和mid+1的值

**代码**

~~~java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length-2; //因为封顶一定存在，然后最右侧肯定为峰顶或者小于峰顶的数
        while(left<=right) {
            int mid = left+(right-left) / 2;
            if(nums[mid]<nums[mid+1]) {
                left = mid +1;
            } else {
                right = mid -1;
            }
        } 
        return left; //退出循环后 right最后可能会到峰顶的左侧
    }
}
~~~

## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

红蓝标记法，红色代表大于最小数，蓝色代表小于等于最小的数 拿中间值和最后一个数进行比较   小于最后值，right-1   大于最后的值 left+1

**代码**

~~~java
class Solution {
    public int findMin(int[] nums) {
        // 最左侧要么是最小值或者最小值右侧
        //蓝色是小于等于最小数,红色 大于最小数
        int  left = 0;
        int right = nums.length - 2;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[nums.length - 1]) { // 大于的话就往左移动 //红色
                left = mid + 1; 
            } else { //蓝色
                right = mid - 1;
            }
        }
        return nums[left];
    }
}
~~~

# 堆

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)



~~~java
/**
小顶堆
**/
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 采用小顶堆
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num,0) + 1); // 记录每个出现的频率
        }

        PriorityQueue<int[]> queue = new PriorityQueue<>((p1, p2) -> (p1[1] - p2[1])); // 小堆
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            // queue里的数量小于k
            if (queue.size() < k) {
                queue.add(new int[] { entry.getKey(), entry.getValue() });
            } else {
                // 当前是否比堆顶的大，大的话把对顶元素踢出 然后将数加进去
                if (entry.getValue() > queue.peek()[1]) { // [1]代表value
                    queue.poll();
                    queue.add(new int[] { entry.getKey(), entry.getValue() });
                }
            }
        }
        int[] res = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            res[i] = queue.poll()[0];
        }
        return res;
    }
}
/**
大顶堆
**/
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 采用大顶堆
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1); // 记录每个出现的频率
        }

        PriorityQueue<int[]> queue = new PriorityQueue<>((p1, p2) -> (p2[1] - p1[1])); // 小堆
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) { // 会将最大的放到堆头
            queue.add(new int[] { entry.getKey(), entry.getValue() });
        }
        //堆头都是大的 拿出k个就行
        int[] res = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            res[i] = queue.poll()[0];
        }
        return res;
    }
}
~~~



# 反转链表

## 反转链表Ⅰ

先保存后一个节点（避免最后当前节点不能后移），然后指向前一个节点，前一个节点后移，当前节点也后移

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null) {
            return null;
        } 
        ListNode pre = null; //前缀节点
        while(head!=null) {
            //保存root节点的后一个节点
            ListNode lastNode = head.next;
            //head指向pre节点
            head.next = pre;
            //pre后移
            pre = head;
            //head后移
            head = lastNode;
        }
        return pre;

    }
}
~~~



## [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

首先申请一个节点dummy ,p0让其指向head  然后dummy到left的左一个节点，然后让其后一个节点(left)为cur，申请一个null节点做反转，可以从测试样例看出执行次数是right+left-1，反转和之前一样，先保存后一个节点，然后指向前一个节点，前一个节点后移，当前节点也后移 最后让p0.next.next=cur p0.next=pre

- cur会指向最后一个节点，pre会指向后一个节点的前一个节点

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // 申请一个前节点 next是head
        ListNode dummy = new ListNode(0, head);
        ListNode p0 = dummy;
        // p0到left的左节点
        for (int i = 0; i < left - 1; i++)
            p0 = p0.next; 
        ListNode pre = null; // 用来第一个节点反转指向null
        ListNode cur = p0.next; // p0的下一个节点为cur
        for (int i = 0; i < right - left +1 ; i++) { // 循环到right
            ListNode lastNode = cur.next;
            //每次循环只修改一个next
            cur.next = pre;
            pre = cur;
            cur = lastNode;
        }
        // 最后 连接一块
        p0.next.next = cur;
        p0.next = pre;
        return dummy.next;
    }
}
~~~

# 快慢指针

- 快指针走两步，慢指针走一步
- 最后快指针在null或者在最后一个节点，慢指针在中间位置



## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        //使用快慢指针
        //需要申请一个虚拟头结点，然后slow才可以指向倒数n节点的前一个节点
        ListNode dummy = new ListNode(0,head);
        ListNode fast = head;
        while(n>0){
            fast = fast.next;
            n--;
        }
        ListNode slow = dummy;
        //然后快慢指针一起走，直到快指针到null
        while(fast!=null) {
            fast = fast.next;
            slow = slow.next;
        }

        //让slow指向后一个节点
        slow.next = slow.next.next;

        //最后返回slow
        return dummy.next;
    }
}
~~~



## [876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast!=null && fast.next!=null) {
            fast = fast.next.next;
            slow = slow.next; 
        }
        //最后返回慢指针
        return slow;
    }
}
~~~

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

- 环，快慢指针总会相遇，并且快指针追到慢指针，慢慢指针进入环后循环次数小于环的长度



**代码**

~~~java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 // 时间复杂度 O(n)  慢指针进入环后循环次数小于环的长度
 // 空间复杂度 O(1) 
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast!=null && fast.next!=null) {
            fast = fast.next.next;
            slow = slow.next; 
            if(slow==fast) {
                return true; //进入环后总会相遇 追及相遇的问题
            }
        }
        return false;
    }
}
~~~

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

两个节点相遇后，让其中一个节点从head从新开始走，另一个节点继续一步一步走，两者会在入口相遇。

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            //快的走两步，慢的走一步
            fast = fast.next.next;
            slow = slow.next;
            if(fast==slow) {
                //有环，然后就相遇
                //相遇然后一个从头结点，一个从相遇节点开始向后走，相遇就是入口
                ListNode index1 = head;
                ListNode index2 = fast;
                while(index1!=index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        
        return null;
    }
}
~~~

## [143. 重排链表](https://leetcode.cn/problems/reorder-list/)

<img src="C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20240414140527854.png" alt="image-20240414140527854" style="zoom:80%;" />



 最终想得到的数据是 1->5->2->4->3  先走第一个然后倒数第一个 然后第二个倒数第二个

首先通过快慢指针找到中间节点，然后将其及其后面反转。 让最后一个节点为head2 保留head和head2的下一个节点，头结点指向head2。然后往后走，循环直到head2的next指向null

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    ListNode midNode(ListNode head) {
        // 先去找到中间节点，然后再将后面节点反转
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    ListNode reverseNode(ListNode head) {
        // 然后申请个null，然后给它反转
        ListNode temp = null;
        while (head != null) {
            ListNode lastNode = head.next;
            head.next = temp;
            temp = head;
            head = lastNode;
        }
        return temp; //这里返回的是temp，最后temp到最后一个节点，而head到null
    }

    public void reorderList(ListNode head) {
        
        ListNode mid = midNode(head);
        ListNode head2 = reverseNode(mid);
        // //用head2 表示最后一个节点

        // 保存head1后的节点
        while (head2.next != null) {
            ListNode hNode1 = head.next;
            ListNode hNode2 = head2.next;
            // head1->head2
            head.next = head2;
            // 5->2
            head2.next = hNode1;
            head = hNode1;
            head2 = hNode2;
        }
    }
}
~~~

# 前后指针

单链表想要删除一个节点需要获取上一个节点，但是如果题目只给你要删除的节点，不知道上一个节点是什么，该怎么做？ 见237

## [237. 删除链表中的节点](https://leetcode.cn/problems/delete-node-in-a-linked-list/)



有一个单链表的 `head`，我们想删除它其中的一个节点 `node`。

给你一个需要删除的节点 `node` 。你将 **无法访问** 第一个节点 `head`。

链表的所有值都是 **唯一的**，并且保证给定的节点 `node` 不是链表中的最后一个节点。

删除给定的节点。注意，删除节点并不是指从内存中删除它。这里的意思是：

- 给定节点的值不应该存在于链表中。
- 链表中的节点数应该减少 1。
- `node` 前面的所有值顺序相同。
- `node` 后面的所有值顺序相同。

**自定义测试：**

- 对于输入，你应该提供整个链表 `head` 和要给出的节点 `node`。`node` 不应该是链表的最后一个节点，而应该是链表中的一个实际节点。
- 我们将构建链表，并将节点传递给你的函数。
- 输出将是调用你函数后的整个链表。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/01/node1.jpg)

```
输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：指定链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9
```

思路：将想要删除的节点的下一节点的值copy到要删除节点。然后将node.next指向node.next.next节点

**代码**

~~~Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val; //将想要删除的节点的下一节点的值copy到要删除节点
        //然后将node.next指向node.next.next节点
        node.next = node.next.next;
    }
}
~~~

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

- 判断是不是需要申请前缀节点 ：**唯一就是head节点是否会被删除**

**本题：n可能等于链表长度 n就会等于头结点了**。所以需要申请前缀节点让其指向head

先让快指针向前走n步，然后让慢指针和快指针一块走。最后让慢指针的next指向next.next  返回前缀节点的next

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0,head);
        //申请快指针 指向head
        ListNode fast = head;
        while((n--)>0) {
            fast = fast.next;
        }
        ListNode slow = dummy;
        while(fast!=null) {
            fast = fast.next;
            slow = slow.next;
        }

        //然后slow的next指向slow.next.next
        slow.next = slow.next.next;

        return dummy.next;
    }
}
~~~

## [83. 删除排序链表中的重复元素 Ⅰ](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

cur = head 然后遍历，查看是否和后面节点相同，相同的话就指向next.next

**代码**

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null) {
            return null;
        }
        ListNode cur = head;
        while(cur.next!=null) {
            //判断是否和后面的相同
            if(cur.val==cur.next.val) {
                //指向后后指针
                cur.next = cur.next.next;
            } else {
                //else往后走
                cur = cur.next;
            }
        }
        return head;
    }
}
~~~

## [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。



可能head节点也为重复元素，需要删除   所以需要申请一个dummy节点

cur = dummy判断cur.next和cur.next.next节点是否相同，相同的话然后去循环删除这个重复的节点（判断是否下一节点和重复的值相同，相同就指向后一个节点），不相同的话，让cur节点指向下一个节点

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(-1, head);
        ListNode cur = dummy;
        // 判断next和next.next是否重复 .需要他俩存在
        while (cur.next != null && cur.next.next != null) {
            int val = cur.next.val;
            // 相等
            if (val == cur.next.next.val) {
               //重要： //循环删除重复的   cur.next.val是不是等于重复的，等于重复的然后指向后一个节点  cur.next等于重复的值，然后往后移
                while(cur.next!=null && cur.next.val == val) {
                    cur.next = cur.next.next;
                }
            //不相等
            } else {
                cur = cur.next;
            }
        }
        return dummy.next;
    }
}
~~~



# 递归

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

获取左子树的深度和右子树的深度，判断哪个大，然后+1

**非全局变量法**

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
       if(root==null) return 0;

       int llength = maxDepth(root.left);
       int rlength = maxDepth(root.right);

       return Math.max(llength,rlength) + 1;
    }
}

//全局变量法
//经过每个节点，然后更新一下全局变量的值
class Solution {
    private int ans; //ans代表经过多少个节点
    public int maxDepth(TreeNode root) {
       dfs(root,0);
       return ans;
    }

    //前序遍历
    //cnt 表示在到达当前节点之前，经过了多少个节点。
    public void dfs(TreeNode node,int cnt){
        if(node==null) return ;
        //每次加1
        ++cnt;
        //然后更新ans值
        ans = Math.max(ans,cnt);
        dfs(node.left,deep);
        dfs(node.right,deep);
    }
}
~~~

## [100. 相同的树](https://leetcode.cn/problems/same-tree/)

遍历p树和q树，让p的左树和q的左树相同，p的r和q的r相同就返回   true   结束条件是p或者q为null   两者都为null，返回true

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        //结束标识   两个节点都为null 返回true 
        if(p==null || q==null) return p==q;
        //判断是否相同
        //左树的左子树 和右树的左子树 右的右子树 和左的左子树
        return p.val == q.val && isSameTree(p.left,q.left) && isSameTree(p.right,q.right);        
    }
}
~~~

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

遍历树，树的左树和树的右树相同，左树的左节点和右树的右节点相同，左数的右节点和右树的左节点相同就为 true   结束条件是p或者q为null   两者都为null，返回true



**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isSameTree(root.left,root.right);
    }
    boolean isSameTree(TreeNode p , TreeNode q) {
        //左子树的右子树和右子树的左子树相同 右子树的右子树和左子树的左子树相同
        //结束标识   两个节点都为null 返回true 
        if(p==null || q==null) return p==q;
        //判断是否相同
        //左树的左子树 和右树的左子树 右的右子树 和左的左子树
        return p.val == q.val && isSameTree(p.left,q.right) && isSameTree(p.right,q.left);
    } 
}
~~~

## [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

左右高度的绝对值为1就是平衡二叉树。可以设置为-1为不平衡，在归的过程中，如果出现等于-1的话，直接返回-1  不是-1就返回它的高度 最后判断是不是等于-1，

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        //-1表示不平衡了
        return getHeight(root)==-1?false:true;
    }
    int getHeight(TreeNode root) {
        if(root==null) return 0;
        //获取left的高度
        int llength = getHeight(root.left);
        //判断是不是已经不平衡了
        if(llength==-1) {
            return -1;
        }
        int rlength = getHeight(root.right);
        if(rlength==-1) {
            return -1;
        }
        //判断两者的高度是不是大于1 >就代表不是平衡
        if(Math.abs(llength-rlength) > 1) {
            return -1;
        }
        //返回高度
        return Math.max(llength,rlength)+1;
    }
}
~~~

## [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

dfs：可以用高度和集合大小，来找右视图。必须先递归右子树，然后递归左子树 判断高度是不是等于集合长度，等于就把val加入集合，![image-20240415171856149](C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20240415171856149.png) 这个图，递归左子树，高度又变为1和2 但是集合长度为3  不会加入，但是6的话和长度相同

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    List<Integer> resList = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0); // dfs记录高度和 list大小 相同才加
        return resList;
    }

    void dfs(TreeNode root, int ans) { // ans代表树的高度
        // 递归右子树，然后树的高度和list的大小一样就放入list集合
        if(root==null) return;
        if (ans == resList.size())
            resList.add(root.val);
        ans++;
        dfs(root.right,ans);
        dfs(root.left,ans);
    }
}
~~~

bfs：队列解题，可以通过队列来判断，for循环 i是队列的长度的时候就记录下来

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    List<Integer> resList = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) {
        if(root==null) return resList;
       Deque<TreeNode> que = new LinkedList();
       que.offerLast(root); //队尾加入
       while(!que.isEmpty()){
        //记录队列长度
        int len = que.size();
        //循环len次，然后让左右节点进队列
        for(int i=0;i<len;i++) {
            //出队列
            TreeNode node = que.poll();
            //判断是不是队列最后一个 i=len-1 循环次数是不是队列长度
            if(i==len-1) resList.add(node.val);
            //左树
            if(node.left!=null) que.addLast(node.left);
            //右树
            if(node.right!=null) que.addLast(node.right);
        }
       }
       //返回resList
       return resList;
    }
}
~~~

# 验证二叉搜索树

## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

**前序遍历：**

初始化根节点的范围为 负无穷到正无穷     判断节点是否在这个区间内，向左子树遍历，让其右范围更新为上一节点的值，让其在负无穷到上一节点值之间    向右子树遍历，让其左范围更新为上一节点的值，让其在上一节点值到正无穷之间

**代码**

~~~java
//前序遍历 根左右
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root,Long.MIN_VALUE,Long.MAX_VALUE);
    }
    boolean isValidBST(TreeNode root,long left,long right) {
        if(root==null) {
           return true;
        }
        //记录节点值
        int x = root.val;
        return left<x && x<right && isValidBST(root.left,left,x) && isValidBST(root.right,x,right);
    }
}
~~~

**中序遍历**

是一个升序，让其判断当前节点和上一节点的大小，先走到最左侧节点，然后往上遍历，如果不满足，直接向上返回false 当前节点大于上一节点就是升序，否则不是 更新前缀的节点为当前节点继续遍历   初始化根节点的上一节点为null

**代码**

~~~java
class Solution {
    TreeNode pre;
    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;
        boolean left = isValidBST(root.left);
        if(!left) return false; //不是升序就返回false
        // 获取当前节点的值
        int x = root.val;
        if (pre!=null && x <= pre.val) {
            return false;
        }
        // 然后更新pre值
        pre = root;
        return isValidBST(root.right);
    }
}
~~~



**后序遍历**：

如果是null，然后返回最小值和最大值分别为正无穷和负无穷，然后遍历左子树拿去最小值和最大值。遍历右子树拿去最小值和最大值，获取当前节点的值，去比较 是不是大于左子树的最大值，和小于右子树的最小值。不满足的话返回最小值为负无穷 最大值为正无穷 返回当前节点值和左侧最小值，当前节点值和右侧最大值(因为会有无穷)

**代码**

~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        // 最后判断最大值不为正无穷即可 因为不满足就会返回 36
        return dfs(root)[1] != Long.MAX_VALUE;
    }

    private long[] dfs(TreeNode node) {
        // null 返回正无穷和负无穷
        if (node == null) { // 37行最大值要和负无穷做比较，所以是负无穷才可以给值
            return new long[] { Long.MAX_VALUE, Long.MIN_VALUE };
        }
        // 获取left的最小值和最大值
        long[] left = dfs(node.left);
        // 获取right的最小值和最大值
        long[] right = dfs(node.right);
        // 获取当前节点值
        int x = node.val;
        // 判断是不是x大于左的最大值和右的最小值
        if (x <= left[1] || x >= right[0]) {
            // 不是就返回负无穷和正无穷
            return new long[] { Long.MIN_VALUE, Long.MAX_VALUE };
        }
        // 返回 当前值和左侧的最小值和右侧的最大值，返回给上一层
        return new long[] { Math.min(x, left[0]), Math.max(x, right[1]) };
    }
}
~~~

# 排序算法

## 冒泡排序：

升序 ， 当前值大于后一个值，发生交换，交换次数是j-i-1 因为每一轮执行完之后都会有已经排好序的

**代码：**

~~~java
public int[] sortArray(int[] nums) {
       //冒泡排序  当前数大于后一个数，交换位置 时间复杂度O(n^2) 需要比较n-1轮 n数组的长度
       for(int i = 0;i<nums.length-1;i++) {
        for(int j = 0 ; j<nums.length-1-i ; j++) {
            if(nums[j]>nums[j+1]) {
                //交换位置
                int temp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = temp;
              }
           }
        }
        return nums;
    }
~~~

**优化冒泡排序**

~~~java
public int[] sortArray(int[] nums) {
       //冒泡排序  当前数大于后一个数，交换位置 时间复杂度O(n^2) 需要比较n-1轮 n数组的长度
       for(int i = 0;i<nums.length-1;i++) {
           //默认数组有序，只要发生一次交换，就必须进行下一轮交换
           //如果，没有发生交换，就代表后续的是有序的，然后开启下轮循环
           boolean flag = true;
        for(int j = 0 ; j<nums.length-1-i ; j++) {
            if(nums[j]>nums[j+1]) {
                //交换位置
                int temp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = temp;
                flag = false;
              }
           }
           if(flag) {
               break;  //去下一轮循环
           }
        }
        return nums;
    }
~~~



## **选择排序：**

每一轮寻找最小的数和最前面进行交换，直到最后有序

每轮找到最小的值，结束后把它和头部进行交换

**代码：**

~~~Java
 public int[] sortArray(int[] nums) {
        // 选择排序：每一轮选择最小元素交换到未排定部分的开头   O(n^2)
        for (int i = 0; i < nums.length; i++) {
            int minIndex = i;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[minIndex]) {
                    minIndex = j;   //将最小值复为当前值 找到最小的值
                }
            }
            //走完一轮，然后将最小的值和i值进行交换
                    int temp = nums[i];
                    nums[i] = nums[minIndex];
                    nums[minIndex] = temp;
        }
        return nums;
    }
~~~

## 插入排序：

**时间复杂度是 O(n^2)**  适合数组短的排序 因为**「短数组」的特点是：每个元素离它最终排定的位置都不会太远**  并且在**数组「几乎有序」**的前提下，「插入排序」的时间复杂度可以达到 **O(N)** 

思路：将一个数字插入到有序的数组里，成为一个更长的有序数组，一段时间后数组有序

从第二个数开始，和前面的数做比较，如果小于前面的数就交换，依次进行

~~~java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        // 插入排序
        for(int i=1;i<len;i++) {
            for(int j = i ; j>0 && nums[j]<nums[j-1];j--) {
                if(nums[j]<nums[j-1]) {
                    swap(nums,j,j-1);
                }
            }
        }
        return nums;
    }
    static void swap(int[] nums,int a,int b) {
        int temp  = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
~~~



**优化   ** 不使用swap方法，使用临时变量来存储当前值，然后让前面的数后移，将这个临时变量在放到前面

~~~java

class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        // 插入排序
        for (int i = 1; i < len; i++) {
            int temp = nums[i]; // 临时变量用来保存当前值，赋值给前面
            int j = i;
            while (j > 0 && temp < nums[j - 1]) {
                // temp小于前面的数，然后就进行交换位置
                nums[j] = nums[j - 1];
                j--;
            }
            // 最后将这个临时变量赋值给 j
            nums[j] = temp;
        }
        return nums;
    }
}
~~~



# 最近的公共祖先

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

递归  如果节点为null或者为p或者为q 就返回当前节点

然后遍历左子树 和右子树 得到返回结果，判断 如果左右子树都找到就返回当前节点，只在左树找到返回递归左子树的结果，只在右树找到返回递归右子树的结果，如果都没找到返回null

遇到pq就返回，不需继续遍历，因为他已经是一个节点，无论q还是p在后面都是要返回这个节点

pq在左右两侧返回当前节点，在右侧返回右侧 在左侧返回左侧

![image-20240417114323385](C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20240417114323385.png)

![image-20240417114608192](C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20240417114608192.png)

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 递归遍历遇到这种情况直接返回节点，无需后续遍历
        if (root == null || root == q || root == p) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // left和right都找到了 就返回根节点
        if (left != null && right != null) {
            return root;
        }
        if (left != null) {
            return left;
        }
        return right;
    }
}
~~~

## [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

二叉搜索树 满足左小右大，可以先判断pq分别属于哪部分，然后去递归子树。

pq都小于根节点，就去遍历左子树，然后返回 ;大于就去找右子树

如果qp在左右子树中，返回root， p或q是当前节点时也返回当前节点  

![image-20240417114348814](C:\Users\康\AppData\Roaming\Typora\typora-user-images\image-20240417114348814.png)

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //不需判断root==null，因为pq一定存在
        //如果节点值在左子树，就去遍历左子树的值
        //左侧返回左侧的根节点
        if(root!=null && p.val<root.val && q.val<root.val) {
            return lowestCommonAncestor(root.left,p,q);
        }
        //右侧返回右侧的根节点
        if(root!=null&&p.val>root.val && q.val>root.val) {
            return lowestCommonAncestor(root.right,p,q);
        }
        //左右树都有 返回根节点
        return root;
    }
}
~~~



# BFS

## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)  

队列来完成，计算队列的大小，然后循环加入左右子树

**代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null)
            return res;
        Deque<TreeNode> queue = new LinkedList();
        queue.offerLast(root);
        while(!queue.isEmpty()) {
            //计算队列的长度
            int len = queue.size();
            List<Integer> list = new ArrayList<>();
            while(len>0) {
                TreeNode node = queue.poll();
                list.add(node.val); //添加进去
                if(node.left!=null) queue.addLast(node.left);
                if(node.right!=null) queue.addLast(node.right);
                len--;
            } 
            res.add(list);
        }
        return res;
    }
}
~~~

## [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)

奇数数组不变，偶数数组逆序

可以定义一个flag 第一层时奇数 flag=false 第二层是偶数 flag=true，在完成遍历后取反对flag，然后添加到列表中前判断是不是偶数还是奇数，偶数反转

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */

//时空复杂度 O(n)
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (root == null)
            return res;
        Deque<TreeNode> queue = new LinkedList();
        queue.offerLast(root);
        // flag=false 代表是奇数
        boolean flag = false;
        while (!queue.isEmpty()) {
            // 计算队列的长度
            int len = queue.size();
            List<Integer> list = new ArrayList<>();

            while (len > 0) {
                TreeNode node = queue.poll();
                list.add(node.val); // 添加进去
                if (node.left != null)
                    queue.addLast(node.left);
                if (node.right != null)
                    queue.addLast(node.right);
                len--;
            }
            // 加之前判断是不是偶数
            if (flag) {
                Collections.reverse(list);
                res.add(list);
            } else {
                res.add(list);
            }
            // 第一次完了之后将其设为!flag if flag=true 就变为false
            flag = !flag;
        }
        return res;
    }
}
~~~

## [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

两种方法：

**第一种**：层序遍历，找到最后第一层，然后取第一个节点   可以定义一个cnt，然后每层第一个数都赋值，最后一个值就是最后一层的第一个值

**第二种**：层序遍历，从右往左遍历，最后一个出队的节点的值就是答案 不需要去双重循环，然后直接拿出节点值就OK  每一个出队列都为node赋值，最后一个出的就是最底层最左侧的那个值

**第一种代码**

~~~java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        // 层序遍历 从右向左进行遍历，最后一个就是最底层最左边的值
        Deque<TreeNode> queue = new LinkedList();
        if (root == null)
            return 0;
        queue.offerLast(root);
        TreeNode node = null;
        int res = 0; // 每一层的第一个数都循环赋值
        while (!queue.isEmpty()) {
            int len = queue.size();
            for (int i = 0; i < len; i++) {
                node = queue.pollFirst();
                if (i == 0)
                    res = node.val;
                // 右先进然后右先出
                if (node.left != null)
                    queue.addLast(node.left);
                if (node.right != null)
                    queue.addLast(node.right);
            }
        }
        // 返回最后一个出队的节点值
        return res;
    }
}
~~~





**第二种代码**

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        //层序遍历 从右向左进行遍历，最后一个就是最底层最左边的值
        Deque<TreeNode> queue = new LinkedList();
        if(root==null) return 0;
        queue.offerLast(root);
        TreeNode node = null;
        while(!queue.isEmpty()) {
            //不需双重循环
           node = queue.pollFirst();
           //右先进然后右先出
           if(node.right!=null) queue.addLast(node.right);
           if(node.left!=null) queue.addLast(node.left);
        }
        //返回最后一个出队的节点值
        return node.val;
    }
}
~~~

# 回溯

## 组合问题

向右 for循环遍历   

向下 递归



回溯三部曲：

- 参数和返回值

- 终止条件

- 单层递归逻辑

  

判断是否能够重复获取前面的元素

不能就有startIndex

### [77. 组合](https://leetcode.cn/problems/combinations/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

n代表可以取的值，k代表终止条件

定义一个path保存每个符合的结果，result集合保存最后的返回结果，startindex代表从哪个数开始进行遍历 



终止：path.size大小==k 

单层递归：从startIndex遍历，加入到path集合，然后递归从startIndex+1取值，然后回溯



**代码**

~~~java
/**
未剪枝优化
*/
class Solution {
    List<List<Integer>> resList = new ArrayList<>();
    List<Integer> path = new ArrayList<>();

    public List<List<Integer>> combine(int n, int k) {
        backstracking(n,k,1);
        return resList;
    }

    void backstracking(int n,int k,int startIndex){
        //返回条件
        if(path.size() == k) {
            resList.add(new ArrayList<>(path));
            return;
        }
        //单层递归
        for(int i=startIndex;i<=n;i++) {
            //加入path
            path.add(i);
            //递归
            backstracking(n,k,i+1);
            //回溯
            path.removeLast();
        }
    }
}



/**
剪枝
 */
class Solution {
    List<List<Integer>> resList = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backstracking(n,k,1);
        return resList;
    }
    void backstracking(int n,int k,int startIndex){
        //返回条件
        if(path.size() == k) {
            resList.add(new ArrayList<>(path));
            return;
        }
        //单层递归 
        //范围太大，需要剪枝，比如 n=4，k=4 从2开始就无需遍历，因为不会满足k=4  2，3，4   至多需要遍历 1就可以 1，2，3，4正好k=4
        for(int i=startIndex;i<=n-(k-path.size())+1;i++) {  //至多需要在哪里开始遍历   
            //加入path
            path.add(i);
            //递归
            backstracking(n,k,i+1);
            //回溯
            path.removeLast();
        }
    }
}
~~~







### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)



找出所有相加之和为 `n` 的 `k` 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 **最多使用一次** 

返回 *所有可能的有效组合的列表* 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```



剪枝操作：

1. sum大于n的话就无需再进行递归
2. 至多到9-(k-midList.size())+1 ，才能满足mid集合里有k个元素  比如 k=2  至多到8 ，到9的话mid集合里面只能有1个元素，直接会不满足

**代码**

~~~java
class Solution {

    List<List<Integer>> resList = new ArrayList<>();
    List<Integer> midList = new ArrayList<>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracing(0, k, n, 1);
        return resList;
    }

    void backtracing(int targetSum, int k, int n, int startIndex) {
        // 结束条件
        if (midList.size() == k) {
            if (targetSum == n) {
                resList.add(new ArrayList<>(midList));
                return;
            }
        }
        // 单层逻辑 只使用数字1到9
        for (int i = startIndex; i <= 9 - (k - midList.size()) + 1; i++) { // 至多到 9-(k-midList.size())+1 才能满足mid集合里有k个元素
            // 如果k=5 那么6之后的就不需要去操作，因为后面是4个数
            // 剪枝 如果i已经大于目标和了，就直接返回
            if (targetSum > n) {
                return;
            }
            targetSum = targetSum + i;
            midList.add(i);
            // 递归
            backtracing(targetSum, k, n, i + 1);
            // 回溯 和前面相反操作
            targetSum = targetSum - i;
            midList.removeLast();
        }

    }
}
~~~

### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

解题思路：

首先将对应关系保存到String集合里

定义全局遍历 s 和集合 reslist

参数：字符串 和 index代表是第几个数字

停止递归的条件是：当index和字符串的长度相同时，就将s放入reslist return

单层递归的逻辑：

首先获取数字对应的字符串

for循环，从0开始然后到字符串结束。 为什么不是startIndex 因为这是两个字母集合，然后不需要去判断是不是重复

然后将每个字符拼接到s中，然后递归，index+1

然后把放进去的字符拿出来



**代码**

~~~java
class Solution {
    //每次迭代获取一个字符串，所以会设计大量的字符串拼接，所以这里选择更为高效的 StringBuilder
    StringBuilder temp = new StringBuilder();    
    List<String> resList = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        if(digits==null|| digits.length()==0) {
            return resList;
        }
        //将其放入String[]中
        String[] sarr = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        trackbacking(digits,sarr,0);
        return resList;
    }
    void trackbacking(String digits,String[] sarr,int index) {
        if(index == digits.length()) {
            resList.add(temp.toString());
            return;
        }
        //从字符串中拿出字符
        String ss = sarr[digits.charAt(index)-'0'];  //"2"-'0' = sarr[2] = abc
        for(int i=0;i<ss.length();i++) {
            //拿出第一个数组字符，拼接到temp中
            temp.append(ss.charAt(i));
            //递归去遍历第二个数组中的字符
            trackbacking(digits,sarr,index+1);
            temp.deleteCharAt(temp.length()-1);
        }
    }
}
~~~

### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```



**代码**

~~~java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates); // 先进行排序
        backTracking(res, new ArrayList<>(), candidates, target, 0, 0);

        return res;
    }

    // startIndex因为是对一个集合进行的操作，所以需要下标
    void backTracking(List<List<Integer>> res, List<Integer> mid, int[] candidates, int target, int startIndex,
            int sum) {
        // 终止条件
        if (sum > target) {
            return;
        }
        if (sum == target) {
            // 符合条件，将mid放入res中
            res.add(new ArrayList<>(mid));
            return;
        }
        // 单层循环 剪枝优化，当前sum+candidates[i]如果大于target的话，就没必要进行下一次递归了
        for (int i = startIndex; i < candidates.length; i++) {
            if (sum + candidates[i] > target)
                break;
            mid.add(candidates[i]);
            // 关键点:不用i+1了，表示可以重复读取当前的数
            backTracking(res, mid, candidates, target, i, sum + candidates[i]);

            // 回溯
            // sum-=candidates[i];
            mid.remove(mid.size() - 1);
        }
    }
}
~~~

### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

**代码**

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> mid = new ArrayList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        //需要先排序
        Arrays.sort(candidates);
        backTracking(candidates,target,0,0);
        return res;
    }

    void backTracking(int[] candidates, int target,int sum,int startIndex) {
        
        if(sum == target) {
            res.add(new ArrayList<>(mid));
            return;
        }
        //剪枝，如果途中sum已经大于target的话，就无需继续递归
        for(int i=startIndex;i<candidates.length && sum + candidates[i] <= target;i++) {
            if(i>startIndex && candidates[i-1] == candidates[i]) {
                continue;
            }
            sum+=candidates[i];
            mid.add(candidates[i]);
            backTracking(candidates,target,sum,i+1);
            sum-=candidates[i];
            mid.removeLast();
        }
    }
}
~~~



## 切割问题

### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

如何分割，无重复的、判断回文字符串

递归终止条件：起始位置大于等于字符串的长度

单层递归：判断是否是回文字符串，是的话获取这个字段加入到集合中，不是的话直接跳过进行下一次循环。进行递归需要起始位置+1

回溯：mid集合最后一个位置删除

**代码**

~~~java
class Solution {
    List<List<String>> res = new ArrayList<>();

    public List<List<String>> partition(String s) {
        backTracking(new ArrayList<>(), s, 0);

        return res;
    }

    void backTracking(List<String> mid, String s, int startIndex) {
        if (startIndex >= s.length()) {
            // 起始位置已经大于等于s的长度
            res.add(new ArrayList<>(mid));
        }
        for (int i = startIndex; i < s.length(); i++) {
            if (isPalindrome(s, startIndex, i)) { // 是回文子串
                // 获取[startIndex,i]在s中的子串
                String str = s.substring(startIndex, i+1);
                mid.add(str);
            } else { // 如果不是则直接跳过
                continue;
            }
            // 切割过的位置，不能重复切割，所以，backtracking(s, i + 1); 传入下一层的起始位置为i + 1。
            backTracking(mid, s, i + 1);
            // 回溯
            mid.remove(mid.size() - 1);  //回溯，恢复上一次递归逻辑之前的数据,截取第二个a然后回撤截取a去i+1截取b，在去执行下一次循环，aab  因为i已经指向了第二个a，再去执行的话，i+1就是b了，所以切割b得到的结果是ab
        }
    }

    boolean isPalindrome(String s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
}
~~~

### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

找准怎么切割，是否需要起始点（不可重复），终止条件，单层递归

s转换为StringBuilder，效率更快  一次递归终止条件是   .的个数是3 并且第四个字段满足0-255 然后可以加入集合 并结束

单层递归， 判断是否符合0-255 符合在后面加入.   然后递归i+2（因为有个.）.的个数也需要+1，然后回溯，将.删除就是i+1  不满足的话直接结束这次递归



**代码**

~~~java
class Solution {
    List<String> res = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {
        StringBuilder sb = new StringBuilder(s);

        backTracking(sb, 0, 0);
        return res;
    }

    /**
     * startnum 不能重复
     * numSize .的个数，不能超过3
     */
    void backTracking(StringBuilder sb, int startnum, int numSize) {
        // 递归终止条件 numSize的个数等于3 然后去判断是否满足0-255
        if (numSize == 3) {
            //判断第四个数是否满足
            if (valid(sb, startnum, sb.length() - 1)) {
                res.add(sb.toString());
            }
            return;
        }
        // 单层递归
        for (int i = startnum; i < sb.length(); i++) {
            // 先判断是不是符合0-255
            if (valid(sb, startnum, i)) {
                sb.insert(i + 1, "."); // 如果满足，然后在满足的最后i+1的位置加一个.
                // 递归下一个 i+2因为中间有个.,numSize.的个数+1
                backTracking(sb, i + 2, numSize + 1);
                //回溯截取的数字,去继续往后截取i+1去判断，然后加再去执行循环
                sb.deleteCharAt(i + 1);
            } else {
                // 不满足直接break
                break;
            }
        }
    }

    public boolean valid(StringBuilder s, int start, int end) {
        if (start > end) {
            return false;
        }
        if (s.charAt(start) == '0' && start != end) { // 0开头不合法
            return false;
        }
        int num = 0;
        for (int i = start; i <= end; i++) {
            if (s.charAt(i) > '9' || s.charAt(i) < '0') {
                return false;
            }
            int size = s.charAt(i) - '0';
            num = num * 10 + size;

            if (num > 255) {
                return false;
            }
        }
        return true;
    }
}
~~~

## 子集问题

需要保留每个树节点的子集

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> mid = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtracking(nums, 0);
        return res;
    }

    /**
     * startIndex 已经取的元素不能再取，不重复的元素
     * 子集，就是获取树的每个节点
     */
    void backtracking(int[] nums, int startIndex) {
        res.add(new ArrayList<>(mid));
        // 递归结束的条件
        if (startIndex >= nums.length) {
            return;
        }
        // 单层递归条件
        for (int i = startIndex; i < nums.length; i++) {
            // 将nums[i]放入mid集合
            mid.add(nums[i]);
            // 递归
            backtracking(nums, i + 1);
            mid.removeLast();
        }
    }
}
~~~

### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

树节点不能重复取相同的数。同树层不能有重复的元素

**代码**

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> mid = new ArrayList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        //先排序
        Arrays.sort(nums);
        backtracking(nums, 0);
        return res;
    }

    void backtracking(int[] nums, int startIndex) {
        res.add(new ArrayList<>(mid));
        // 递归结束的条件
        if (startIndex >= nums.length) {
            return;
        }
        // 单层递归条件
        for (int i = startIndex; i < nums.length; i++) {
            // 说明前后两个数是相同的，需要跳过
            if (i > startIndex && nums[i] == nums[i - 1]) { //i=3 start=2
                continue;
            }
            // 将nums[i]放入mid集合
            mid.add(nums[i]);

           // 递归
            backtracking(nums, i + 1);
            mid.removeLast();
        }
    }
}
~~~

### [491. 非递减子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

子序列需要递增，需要去重(但是因为重复的不挨着不能用之前的来去重，需要用set集合来去重)  set集合因为在一次循环里是相同的，同层就可以判断是不是取过值

~~~java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();

    public List<List<Integer>> findSubsequences(int[] nums) {
        backTracking(nums, 0);
        return result;
    }

    void backTracking(int[] nums, int startIndex) {
        if (path.size() > 1) {
            // 大于1，集合元素有两个就放进result中
            result.add(new ArrayList<>(path));
        }
        // 去重 ,因为不能排序，所以相同的不会在一起，就不能用之前的方式去判断
        Set<Integer> set = new HashSet<>();
        for (int i = startIndex; i < nums.length; i++) {
            if (!path.isEmpty() && path.get(path.size() - 1) > nums[i] || set.contains(nums[i])) {
                continue;
            }
            // 先加到path中
            path.add(nums[i]);
            // 先判断是否在集合中
            set.add(nums[i]);
            backTracking(nums, i + 1);
            path.removeLast();
        }
    }
}
~~~

## 排列问题

### [46. 全排列](https://leetcode.cn/problems/permutations/)

排列从0开始，不需要startIndex来标注是否使用过。用used布尔数组，使用过将对应位置设为true，来判断是否需要再一次记录

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    // 利用used来判断同层是否加过数据
    boolean[] used;

    public List<List<Integer>> permute(int[] nums) {
        used = new boolean[nums.length];
        backTracking(nums);
        return res;
    }

    void backTracking(int[] nums) {
        if (nums.length == path.size()) {
            res.add(new LinkedList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i] == true)
                continue; // 记录path存在过元素
            // 没存在就设置为true
            used[i] = true;
            path.add(nums[i]);
            backTracking(nums);
            //回溯 删除末尾元素，并将其设为false，去加载后面的元素
            path.removeLast();
            used[i] = false;
        }
    }
}
~~~

### 47.全排列Ⅱ

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

思路：同树层不可以使用之前使用过的数，使用set集合来查看是否用过同树层，利用used布尔数组来做排列

**代码**

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    boolean[] used;

    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        backTracking(nums);
        return res;
    }

    void backTracking(int[] nums) {
        // 结束
        if (nums.length == path.size()) {
            res.add(new ArrayList<>(path));
            return;
        }
        Set set = new HashSet();

        for (int i = 0; i < nums.length; i++) {
            //同树层是否用过，同一列是否用过，用过跳过取下一个数字
            if (!set.isEmpty() && set.contains(nums[i]) || used[i] == true) {
                continue;
            }
            set.add(nums[i]);
            path.add(nums[i]);
            used[i] = true;
            backTracking(nums);
            path.removeLast();
            used[i] = false;
        }
    }
}
~~~

第二种，不使用set集合。查看前后两个数是否相同，并且前面的数是否使用过

**代码**

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    boolean[] used;

    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        Arrays.sort(nums);
        backTracking(nums);
        return res;
    }

    void backTracking(int[] nums) {
        // 结束
        if (nums.length == path.size()) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            //表示 前后两个数相同，并且前面的数使用过是true，要是没使用过的话就可以使用
            if (i>0 && nums[i] == nums[i-1] && used[i-1] == true) {
                continue;
            }
            if (used[i] == false) {
            //set.add(nums[i]);
            path.add(nums[i]);
            used[i] = true;
            backTracking(nums);
            path.removeLast();
            used[i] = false;
            }
        }
    }
}
~~~

# 贪心问题

## [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

考虑局部最优，最终实现全局最优

先排序

可以考虑大的饼干去喂大的胃口，最终能实现最多。或者最小的胃口去选取最小的饼干。用第一种

**代码**

~~~java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int index = s.length - 1;
        // 大饼干优先喂饭量大的 .优先考虑大胃口而不是大肚子
        int res = 0;
        for (int i = g.length - 1; i >= 0; i--) {
            if (index >= 0 && s[index] >= g[i]) {
                index--;
                res++;
            }
        }
        return res;
    }
}
~~~

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

如果只有一个数就直接返回nums[0]，设为max为Integer最小值，因为可能出现都为-1的情况。count设置为0代表每个子数组的和从0开始。如果count+nums[i]小于0，就从nums[i+1]开始从count从新加

**代码**

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        int sum = Integer.MIN_VALUE; // 为什么取的是最小的值，因为如果所有数都是-1，才可以取到0
        int count = 0; // 区间初始值
        for (int i = 0; i < nums.length; i++) {
            // 如果sum+nums[i]<0 跳过这个
            count += nums[i];
            sum = Math.max(sum, count); // 取每个区间累计的最大值 第一个区间是2，然后可能遇到下一个条件从新开始，1是最大值。但sum=2
            if (count <= 0) {
                count = 0; // 如果 count+nums[i]变为负数，那么就应该从 nums[i+1]开始从 0 累积 count 了
            }
        }
        return sum;
    }
}
~~~

