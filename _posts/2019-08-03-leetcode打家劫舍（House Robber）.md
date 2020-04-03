---
layout: post
title: leetcode打家劫舍（House Robber）
tag: 算法
---
## 1. 难度easy

**题目**：你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

**示例 1:**

```java
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2:**

```java
输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**解法：**

```java
public int rob(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int len=nums.length;
        if (len==1){
            return nums[0];
        }
        if (len==2){
            return Math.max(nums[0],nums[1]);
        }
        int[] dp=new int[len];
        dp[0]=nums[0];
        dp[1]=Math.max(nums[0],nums[1]);
        for (int i=2;i<len;i++){
            dp[i]=Math.max(dp[i-1],dp[i-2]+nums[i]);//dp[i]为第i个数结尾的最高金额
        }
        return dp[len-1];
    }
```

## 2. 难度mid

**题目：**你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下，**能够偷窃到的最高金额。

**示例 1:**

```java
输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2:**

```java
输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。偷窃到的最高金额 = 1 + 3 = 4 。
```

**解法：**

```java
//由于是圆形所以首尾两个数最多只能选一个，因此分两种情况讨论，然后比较两种情况返回最大值即可。
public int rob(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int len=nums.length;
        if (len==1){
            return nums[0];
        }
        if (len==2){
            return Math.max(nums[0],nums[1]);
        }
        if (len==3){
            return Math.max(Math.max(nums[0],nums[1]),nums[2]);
        }
        int[] dp1=new int[len-1];
        int[] dp2=new int[len];
        dp1[0]=nums[0];
        dp1[1]=Math.max(nums[0],nums[1]);
        for (int i=2;i<len-1;i++){//不计算最后一个数字
            dp1[i]=Math.max(dp1[i-1],dp1[i-2]+nums[i]);
        }
        dp2[1]=nums[1];
        dp2[2]=Math.max(nums[1],nums[2]);
        for (int i=3;i<len;i++){
            dp2[i]=Math.max(dp2[i-1],dp2[i-2]+nums[i]);//不计算第一个数字
        }
        return Math.max(dp1[len-2],dp2[len-1]);//返回两种情况的最大值
    }
```

## 3. 难度hard

**题目：**在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**示例 1:**

```java
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
```

**示例 2:**

```java
输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```

**解法：**

```java
//从根节点开始，分为两种情况：
//1.偷改节点则不能偷其左右孩子，可以尝试偷取其左右孩子的孩子节点
//2.不偷取该节点，可以尝试偷取其左右孩子节点
//3.选择两种情况的最大值
public int rob(TreeNode root) {
        if (root==null){
            return 0;
        }
        int res1=0,res2=0;
        if (root.left!=null){
            res1+=rob(root.left.left)+rob(root.left.right);
        }
        if (root.right!=null){
            res1+=rob(root.right.left)+rob(root.right.right);
        }
        res1+=root.val;
        res2=rob(root.left)+rob(root.right);
        return Math.max(res1,res2);
    }
```

