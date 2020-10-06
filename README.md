# 2020.10.6———//让这个变量等于两个字符串的最大相同前缀
## 题目
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:
```
输入: ["flower","flow","flight"]
输出: "fl"
```
示例 2:
```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```
说明:

所有输入只包含小写字母 a-z 。
## 解题
### 解法一 横向扫描
#### 语言
java
#### 思路
从前往后依次进行比对，求出前两个的最大相同前缀，然后把得出的结果再与后面的进行比对，依次类推。
![](https://assets.leetcode-cn.com/solution-static/14/14_fig1.png)
#### 代码
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs==null || strs.length==0){
            return "";
        }//这是为了区别没有字符串的特殊情况
        String prefix = strs[0];
        int count = strs.length;
        for(int i=1;i<count;i++){
            //让这个变量等于两个字符串的最大相同前缀
            prefix = longestCommonPrefix(prefix,strs[i]);
            if(prefix.length()==0){
                break;//如果最大前缀已经为零那就直接推出循环
            }
        }
        return prefix;
    }

    public String longestCommonPrefix(String str1,String str2){
        int length = Math.min(str1.length(),str2.length());
        int index = 0;
        while(index<length && str1.charAt(index)==str2.charAt(index)){
            index++;
        }
        return str1.substring(0,index);//截取那一段长度的字符串
    }
}
```
### 解法二 横向扫描
#### 语言
java
#### 思路
就是通过纵向的对比每个字符串的相同位置的字符来确定，具体如图
![](https://assets.leetcode-cn.com/solution-static/14/14_fig2.png)
#### 代码
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs==null||strs.length==0){
            return "";
        }
        int length = strs[0].length();//保存第一行的字符串长度
        int count = strs.length;//保留一共有几行字符串
        for(int i=0;i<length;i++){//遍历第一行的字符串中的每一个
            char c = strs[0].charAt(i);//让c为第一行中的某个字符
            for(int j = 1;j<count;j++){//遍历其他行
                if(i==strs[j].length()||strs[j].charAt(i)!=c){
                    //如果长度已经不够了或者已经出现了不相等的字符串那就输出结果
                    return strs[0].substring(0,i);
                }
            }
        }
        return strs[0];//如果已经退出循环，那就是整个就是最大相同前缀
    }
}
```
### 解法三 分治法
#### 语言
java
#### 思路
从中间分一半，然后进行判断,具体如图
![](https://assets.leetcode-cn.com/solution-static/14/14_fig3.png)
#### 代码
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs==null||strs.length==0){
            return "";
        }else{
            return longestCommonPrefix(strs,0,strs.length-1);//返回这个函数
        }
    }
    public String longestCommonPrefix(String[] strs,int start,int end){
        if(start==end){
            return strs[start];//加入单字符情况
        }else{
            int mid = (end - start)/2+start;//计算中点
            String lcpLeft = longestCommonPrefix(strs,start,mid);
            String lcpRight = longestCommonPrefix(strs,mid+1,end);
            return commonPrefix(lcpLeft,lcpRight);
        }
    }
    public String commonPrefix(String lcpLeft,String lcpRight){
        int minLength = Math.min(lcpLeft.length(),lcpRight.length());
        for(int i=0;i<minLength;i++){
            if(lcpLeft.charAt(i)!=lcpRight.charAt(i)){
                return lcpLeft.substring(0,i);//遍历得出最大相同前缀
            }
        }
        return lcpLeft.substring(0,minLength);//退出了循环那就是全部都是相同前缀
    }
}
```
