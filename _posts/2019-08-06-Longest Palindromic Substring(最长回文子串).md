---
layout: post
title: Longest Palindromic Substring(最长回文子串)
date: 2019-08-06 
tag: 算法
---
**题目：**给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```java
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```java
输入: "cbbd"
输出: "bb"
```

**解法一(暴力破解法)：**

```java
 	/**
     * 暴力破解法：用两层循环遍历所有的子串，判断字串是否
     * 是回文串，如果是回文串就记录下来，当有新的回文串时，
     * 比较记录中的回文串和当前回文串的长度，用较长的串替
     * 换当前串  如果两串长度相同，保留旧的
     * @param s
     * @return
     */
    public String longestPalindrome(String s) {
        if (s.isEmpty()||s.length()==0){
            return "";
        }
        int len=s.length();
        if (len==1){
            return s;
        }
        int left=0,right=0;//记录最长回文串的左右区间
        for (int i=len;i>0;i--){
            for (int j=0;j<i;j++){
                if (isPlalindrome(s.substring(j,i))){
                    if (i-j>right-left){//判断当前区间是否大于存储的最大区间
                        left=j;
                        right=i;
                    }
                }
            }
        }
        return s.substring(left,right);
    }

    /**
     * 判断一个字符串是否是回文串
     * @param str 需要进行判断的字符串
     * @return 返回该整数是否为回文串
     */
    public static boolean isPlalindrome(String str) {
        char[] array=str.toCharArray();
        int left=0,right=array.length-1;
        while(left<right)
        {
            if(array[left++]!=array[right--])//如果两位值的字符不相同 则不对称
                return false;
        }
        return true;
    }
```

**解法二(中心拓展法):**

```java
 	/**
     * 中心拓展法:选择一个字符作为中心 向两边查找回文串,但是查找过程
     * 中需要将长度为奇数的回文串和长度为偶数的回文串分开考虑
     * @param s
     * @return
     */
    public String longestPalindrome(String s) {
        if (s.isEmpty()||s.length()==0){
            return "";
        }
        int len=s.length();
        if (len==1){
            return s;
        }
        String result="";
        String str;
        for (int i=0;i<len-1;i++){
            str=getLongestPalindrome(s,i,i);//奇数位的回文子串
            if (str.length()>result.length()){
                result=str;
            }
            str=getLongestPalindrome(s,i,i+1);//偶数位的回文子串
            if (str.length()>result.length()){
                result=str;
            }
        }
        return result;
    }

    /**
     * 从中心向两端寻找回文子串
     * @param s
     * @param i
     * @param j
     * @return
     */
    public static String getLongestPalindrome(String s, int i, int j) {
        int len=s.length();
        int left=i,right=j;
        while (left>=0&&right<=len-1&&s.charAt(left)==s.charAt(right)){
            left--;
            right++;
        }
        return s.substring(left+1,right);
    }
```

**解法三(动态规划法)：**

```java
	/**
     * 动态规划：判断i与j之间的子串是否是回文串,即
     * dp[i][j]=(s.charAt(i)==s.charAt(j))&&dp[i+1][j-1]
     * @param s
     * @return
     */
    public static String longestPalindrome(String s) {
        if (s.isEmpty()||s.length()==0){
            return "";
        }
        int len=s.length();
        if (len==1){
            return s;
        }
        String result="";
        boolean[][] dp=new boolean[len][len];//表示i到j之间的字符串是否回文
        for (int i=0;i<len;i++){
            dp[i][i]=true;
        }
        for (int i=0;i<len-1;i++){//判断长度为2的回文串
            if (s.charAt(i)==s.charAt(i+1)){
                dp[i][i+1]=true;
                result=s.substring(i,i+1+1);
            }
        }
        for (int j=3;j<=len;j++){//判断长度为3到len的回文串
            for (int k=0;k<=len-j;k++){
                int i=k+j-1;
                if (s.charAt(k)==s.charAt(i)&&dp[k+1][i-1]){
                    dp[k][i]=true;
                    if (i-k>result.length()){
                        result=s.substring(k,i+1);
                    }
                }else {
                    dp[k][i]=false;
                }
            }
        }
        return result;
    }
```

