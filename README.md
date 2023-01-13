- [Blind75Note](#blind75note)
- [************String************](#------------string------------)
  * [3.Longest Substring Without Repeating Characters](#3longest-substring-without-repeating-characters)
    + [1.Brute Force,use hashset](#1brute-force-use-hashset)
    + [2.Sliding window](#2sliding-window)
  * [424.Longest Repeating Character Replacement](#424longest-repeating-character-replacement)
    + [1.sliding window](#1sliding-window)
  * [242.Valid Anagram](#242valid-anagram)
  * [49.Group Anagrams](#49group-anagrams)
    + [Approach 1: Categorize by Sorted String](#approach-1--categorize-by-sorted-string)
    + [Approach 2: Categorize by Count](#approach-2--categorize-by-count)
  * [20.Valid parentheses](#20valid-parentheses)
  * [Valid Palindrome](#valid-palindrome)
  * [5.Longest Palindromic Substring*](#5longest-palindromic-substring-)
    + [1.Calculate the max length](#1calculate-the-max-length)
    + [2.Calculate the start and end index](#2calculate-the-start-and-end-index)
  * [647Palindromic Substrings](#647palindromic-substrings)
  * [14.Longest Common Prefix](#14longest-common-prefix)
- [************Binary************](#------------binary------------)
  * [104.Maximum Depth of Binary Tree](#104maximum-depth-of-binary-tree)
  * [543.Diameter of Binary Tree](#543diameter-of-binary-tree)
  * [124.Binary Tree Maximum Path Sum](#124binary-tree-maximum-path-sum)
  * [100.Same Tree](#100same-tree)
  * [572.Subtree of Another Tree](#572subtree-of-another-tree)
  * [226.Invert Binary Tree](#226invert-binary-tree)
  * [102.Binary Tree Level Order Traversal](#102binary-tree-level-order-traversal)
- [************Array************](#------------array------------)
  * [1480.Running Sum of 1d Array](#1480running-sum-of-1d-array)
  * [724.Find Pivot Index](#724find-pivot-index)
    + [1.Brute Force](#1brute-force)
    + [2. Prefix Sum](#2-prefix-sum)
- [************Implementation/Simulation************](#------------implementation-simulation------------)
  * [202. Happy Number](#202-happy-number)
  * [1706.Where Will the Ball Fall](#1706where-will-the-ball-fall)
- [************Linked List************](#------------linked-list------------)
  * [234.Palindrome Linked List](#234palindrome-linked-list)
  * [19.Remove Nth Node From End of List](#19remove-nth-node-from-end-of-list)
    + [1.Method written by myself faster than 100%](#1method-written-by-myself-faster-than-100-)
    + [2.Two pointer](#2two-pointer)
- [************Binary Search************](#------------binary-search------------)
  * [33.Search in Rotated Sorted Array](#33search-in-rotated-sorted-array)
- [************BST************](#------------bst------------)
  * [94.Binary Tree Inorder Traversal](#94binary-tree-inorder-traversal)
    + [1.Recursive](#1recursive)
    + [2.STACK](#2stack)
  * [230.Kth Smallest Element in a BST](#230kth-smallest-element-in-a-bst)
    + [1.STACK](#1stack)
    + [2.Recursive](#2recursive)
  * [108.Convert Sorted Array to Binary Search Tree](#108convert-sorted-array-to-binary-search-tree)
  * [173.Binary Search Tree Iterator](#173binary-search-tree-iterator)
- [************Graph************](#------------graph------------)
  * [207.Course Schedule(bfs,topological sort)](#207course-schedule-bfs-topological-sort-)
  * [210.Course Schedule II(bfs)](#210course-schedule-ii-bfs-)
  * [269.Alien Dictionary](#269alien-dictionary)
  * [200.Number of Islands(dfs)](#200number-of-islands-dfs-)
  * [133.Clone Graph](#133clone-graph)
  * [128.Longest Consecutive Sequence](#128longest-consecutive-sequence)
    + [1.Set](#1set)
    + [2.sort method but O(nlogn)](#2sort-method-but-o-nlogn-)
  * [261.Graph Valid Tree](#261graph-valid-tree)
    + [1.Iterative Breadth-First Search.](#1iterative-breadth-first-search)
    + [2.Recursive Depth-First Search.](#2recursive-depth-first-search)
  * [323.Number of Connected Components in an Undirected Graph](#323number-of-connected-components-in-an-undirected-graph)
    + [1.DFS](#1dfs)
    + [2.union find](#2union-find)
  * [994.Rotting Oranges](#994rotting-oranges)
- [Dynamic Programming](#dynamic-programming)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Blind75Note

My second time doing these problem, so I wrote some note to help my understand these problem deeply.

# ************String************

## 3.Longest Substring Without Repeating Characters

```java
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### 1.Brute Force,use hashset

Use i , j to traverse the whole string.

From i to j see if there's repeat character, 

​		if no repetition, record the length of i to j as result,

​		if have repetition, we cannot let j++, because there should no repeating character, so we let i++

Time complexity O(n^3)

This method can't pass due to time limit 

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
       if(s.length() == 0) return 0;
        
        int res = 0;
        
        for(int i = 0; i < s.length();i++){
            for(int j = i; j < s.length();j++){
                if(checkRepetion(s,i,j)) res = Math.max(res,j-i+1);
                else
                    break;
            }
        }
        
        return res;
    }
    
    private boolean checkRepetion(String s,int start,int end){
        Set<Character> set = new HashSet<>();
        
        for(int i = start; i <= end; i++){
            if(set.contains(s.charAt(i)))
                return false;
            else set.add(s.charAt(i));

        }
        
        return true;
    }
}
```

The optimization method: O(n^2) passed

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
       if(s.length() == 0) return 0;
        
        int res = 0;
        
        for(int i = 0; i < s.length();i++){
            Set<Character> set = new HashSet<>();
            for(int j = i; j < s.length();j++){
                if(set.contains(s.charAt(j))) break;
                else{
                    set.add(s.charAt(j));
                    res = Math.max(res,j-i+1);
                }
                
            }
        }
        
        return res;
    }
    
}
```

### 2.Sliding window 

Firstly, create a set and put the non repeated character in the set, record the current length;

when meet the repeated character, remove all the character before that repeated character.

Time complexity : O(2n) =*O*(*n*).   i-n times j-n times. so 2n

**The essence of this method is similar to the one above**

![image-20221204100354826](https://s2.loli.net/2022/12/05/1xOhi7aNuE3KAU6.png)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        
        int i = 0; int j = 0;
        
        Set<Character> set = new HashSet<>();
        
        int res = 0;
        
        while(i < s.length()){
            char currentChar = s.charAt(i);
            if(!set.contains(currentChar)){
                set.add(currentChar);
                res = Math.max(res,set.size());
                i++;
            }else{
                set.remove(s.charAt(j));
                j++;
            }
        }
        
        return res;
    }
    
}
```

## 424.Longest Repeating Character Replacement

```java
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

### 1.sliding window 

The method is quite similar to the question above 

Use left and right pointer to form a sliding window.

**if(the length of the window -  Most frequently occurring letters <= Key)**

​	the length of the window is the result; but we need to find the max

​	then let right bound++, to see if the next window meet this condition.

**else** 

​	there is no need to let right boundary++, the result is the same, so let left boundary++. at this time the window size has reduced by 1. so we need to let right boundary++ to keep finding the max result.

The ending condition is right boundary < s.length(). at that time we've already got the max result



There are many details need to be mind in this problem. See the comment

**dic[s.charAt(i) - 'A']:** put 26 character in certain place

**dic[0]-A,dic[1]-B,dic[2]-C,,,dic[26]-Z**

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int left = 0; 
        int right = 0;
        // to record the number of 26-characters
        int[] dic = new int[26];
        
        int res = 0;
        
        int maxcount = 0;
        while(right < s.length()){
            //record the current character frequency
            dic[s.charAt(right) - 'A']++;
            //this shows the number of Most frequently occurring letters
             maxcount = Math.max(maxcount, dic[s.charAt(right) - 'A']);
            
            if(right - left + 1 - maxcount <= k){
                res = Math.max(res,right - left + 1);
                right++;
            }else{
                //move the window forward but need to remove the number recorded 
                dic[s.charAt(left) - 'A']--;
                left++;
                right++;
            }
            
        }
        
        return res;
    }
}
```

## 242.Valid Anagram

put all the character of string s in a dictionary

then traversal the String t, correspondingly minus the index value

**This method can also be used to judge if a Long String can contain a Short Sub-String**

just change the judgement statement:  s:abba  t:bba

**for(int i : dic)  if(i < 0) return false;**
        

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        
        int[] dic = new int[26];
        
        for(int i = 0; i < s.length(); i++){
            dic[s.charAt(i) - 'a']++;
        }
        
        for(int i = 0; i < t.length();i++){
            dic[t.charAt(i) - 'a']--;
        }
        
        for(int i : dic){
            if(i != 0) return false;
        }
        
        return true;
    }
}
```

## 49.Group Anagrams

```java
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

### Approach 1: Categorize by Sorted String

![image-20221206101838680](https://s2.loli.net/2022/12/07/wPQ7LkB1McI8UeZ.png)

This method used many API, many details 

key point is to sort the string and put it in the map as key 

Time Complexity: O(NKlog⁡K) 

N-length of the strs

k-length of the s, it used a sort so O(klogk) 

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs == null || strs.length == 0) return new ArrayList<>();

        Map<String,List<String>> map = new HashMap<>();

        for(String s : strs){ // s:"eat"
            char[] c = s.toCharArray(); // c:['e','a','t']
            Arrays.sort(c); //c:['a','e','t']
            String key = String.valueOf(c);//c:"aet"
            if(!map.containsKey(key)){
                map.put(key,new ArrayList<>());  //("aet",[])
            }
            map.get(key).add(s);//("aet",["eat"])
        }
        System.out.print(map.values());
        return new ArrayList<>(map.values());
        
    }
}
```

### Approach 2: Categorize by Count

Put every string's character in a dictionary just like the problem242.

If the value of dictionary is same, the string must also be same.

But we cannot only judge according to the number, for example "bddddddddd" and "bbbbbbbbbbc" the diction would be the same.

To avoid this, we can append “#” or"*" or something else to maintain every number is right.

![image-20221206105557401](https://s2.loli.net/2022/12/07/mBGMbgENtjuaJlx.png)

Time Complexity: O(NK)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs == null || strs.length == 0) return new ArrayList<>();
        
        Map<String,List<String>> map = new HashMap<>();

        for(String s : strs){
            char[] c = s.toCharArray();
            int[] dic = new int[26];
            for(char ch : c) dic[ch - 'a']++;
    
            StringBuffer sb = new StringBuffer();
            for(int i = 0 ; i < dic.length; i ++){
                sb.append("#");
                sb.append(dic[i]);
            }
            String key = sb.toString();

            if(!map.containsKey(key)) map.put(key,new ArrayList<>());
            map.get(key).add(s);
        }

        return new ArrayList<>(map.values());
    }
}
```

## 20.Valid parentheses

```java
Example 1:

Input: s = "()"
Output: true
Example 2:

Input: s = "()[]{}"
Output: true
Example 3:

Input: s = "(]"
Output: false
```

Traversal the string

If meet a left "(" or "[" or "{", use stack to push a right part "]", ")", "}"

If not push the top of the stack to see if it meets the current character.

Here to mind if still traversal the string but the stack is already empty, return false; for example : "()))" or "))))" 

If after traversing the string the stack is empty, that's valid. if not , invalid.

```java
class Solution {
    public boolean isValid(String s) {
        
        Stack<Character> stack = new Stack<>();

        for(int i = 0; i < s.length();i++){
            if(s.charAt(i) == '('){
                stack.push(')');
            }else if(s.charAt(i) == '['){
                stack.push(']');
            }else if(s.charAt(i) == '{'){
                stack.push('}');
            }else if(stack.isEmpty()){
                return false;
            }else if(stack.pop() != s.charAt(i)){
                return false;
            }
        }
        return stack.isEmpty();

    }

    
}
```

125. ## Valid Palindrome

I don't think its a good problem, but this problem help me to practise some API about Character and StringBuffer/StringBuilder

**Example 1:**

```java
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```java
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**

```java
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

```java
class Solution {
    public boolean isPalindrome(String s) {
        
        StringBuilder sb = new StringBuilder();

        //put digital or character in a StringBuilder
        for(int i = 0; i < s.length(); i++){
            if(Character.isLetterOrDigit(s.charAt(i)))
            sb.append(Character.toLowerCase(s.charAt(i)));
        }

        //record the sb to compare with reverse one 
        StringBuilder sb1 = new StringBuilder(sb);
        
        sb.reverse();
        
        // if use stringbuilder to compare it always return false 
        // it has not overwrite the equals method
        if(sb.toString().equals(sb1.toString())) return true;
        else return false;
    }
}
```

## 5.Longest Palindromic Substring*

***This question is worth doing many times**

**Example 1:**

```java
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

 The method of this problem is quite unique, and there're also many details and boundary conditions to be care, I think its harder than  some normal medium problem 

**Traversal every character in the String, and extend by centre, But the length of Palindromic Substring can be odd or even, so we need to extend twice. see the code. This is easy to understand.** 

**But there's also a detail to be care, we need to find the longest, so we define a MaxLength variable, if the current length > MaxLength, we update the MaxLength value.**

![image-20221207120041640](https://s2.loli.net/2022/12/08/RBHTpoUn2i6yuJg.png)

### 1.Calculate the max length

```java
class Solution {
    int left, maxLength;

    public String longestPalindrome(String s) {
        if(s.length() <= 1) return s;

        for(int i = 0 ; i < s.length() - 1; i++){
            extendByCentre(s,i,i); // the palindromic substring length is odd
            extendByCentre(s,i,i+1);// palindromuc substring length is oven
        }

        return s.substring(left,left + maxLength);

    }

    private void extendByCentre(String s, int i, int j){
        while( i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j) ){
            i--;j++;
        }
		
        //if there's longer substring
        //update the maxLength and start point
        if(maxLength <= j - i - 1){
             maxLength  = j - i -1 ;
             left = i + 1;
        }
       
    }
}
```

### 2.Calculate the start and end index

The method below is similar but it calculate the start index and end index 

Its quite complex especially many index and details to be care, I prefer the method above, just calculate the max length

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.length() <= 1) return s;
        int max = 0;
        int start = 0;
        int end = 0;
        for(int i = 0 ; i < s.length() - 1; i++){
           int len1 = extendByCentre(s,i,i); // the palindromic substring length is odd
           int len2 = extendByCentre(s,i,i+1);// palindromuc substring length is oven
			//recore the max len of one character
            max = Math.max(len1,len2);
            //if there's longer substring
            //update the star and end index
            if(end - start < max ){
                start = i - (max - 1)/2 ;//here
                end = i + max/2;
            }
        }
      return s.substring(start, end + 1);

    }

    private int extendByCentre(String s, int i, int j){
        while( i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j) ){
            i--;j++;
        }
        return j - i - 1;
    }
}
```

## 647Palindromic Substrings

**Example 1:**

```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2:**

```java
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

The method is the same as the problem 5 just one above



```java
class Solution {
    int count;
    public int countSubstrings(String s) {
        
        //must from 0, beacuse the first character is also a palindromic string
        for(int i = 0 ; i < s.length(); i++){
            extendByCentre(s,i,i);//this can count a palindromic string
            extendByCentre(s,i,i+1);
        }

        return count;
    }

    private void extendByCentre(String s, int i , int j){
        while(i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)){
            count++;
            i--;
            j++;
        }
    }
}
```

## 14.Longest Common Prefix

```java
int a1 = "abc".indexOf("c"); //2
int a2 = "abc".indexOf("ab");//0
```

```java
class Solution {
    
public String longestCommonPrefix(String[] strs) {
        
    if(strs == null || strs.length == 0) return "";
    
    String prefix = strs[0];
    
    for(int i = 0; i < strs.length;i++){
        while(strs[i].indexOf(prefix) != 0){          
            prefix = prefix.substring(0,prefix.length() - 1);
        }
    }
    
    return prefix;
    
    }
}
```



# ************Binary************

## 104.Maximum Depth of Binary Tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```java
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

Return the maximum height of left root or right root

```java
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
    int count;
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;

        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        return Math.max(left,right) +1;

    }
}
```

## 543.Diameter of Binary Tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

Same as 104, just record each node's maximum height value of left+right node

```java
class Solution {
    int max=0;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
        findHeight(root);
        return max;
    }
    public int findHeight(TreeNode root){
        if(root == null) return 0;

        int left = findHeight(root.left);
        int right = findHeight(root.right);

        max = Math.max(max,right + left);

        return Math.max(left,right) + 1;

    }
}
```

## 124.Binary Tree Maximum Path Sum

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

Similar as 124.  Not difficult see the code

```java
class Solution {
    int res = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        findMaxPath(root);
        return res;
 	}

    private int findMaxPath(TreeNode root){
        if(root == null) return 0;
        
        //compare to 0, because if sub node is under 0. 
        //just not add them,leave current node as path
        int left = Math.max(findMaxPath(root.left),0);
        int right = Math.max(findMaxPath(root.right),0);

        res = Math.max(res,left + right + root.val);

        return Math.max(left,right) + root.val;
    }
}
```



## 100.Same Tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

```java
Input: p = [1,2], q = [1,null,2]
Output: false
```

```java
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

        if(p == null && q == null) return true;

        if(p == null || q == null) return false;

        if(p.val != q.val) 
            return false;
        else    //  if p.val == q.val check the sub tree
            return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
                
    }
}

```

## 572.Subtree of Another Tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

Just check if there is same tree

```java
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if(root == null && subRoot == null) return true;
        if(root == null) return false;
        
        if(isSame(root,subRoot)) 
            return true;
        else 
            return isSubtree(root.left,subRoot) || isSubtree(root.right,subRoot);
    }
    
    private boolean isSame(TreeNode root, TreeNode subRoot){
        if(root == null && subRoot == null) return true;
        
        if(root == null || subRoot == null) return false;
        
        if(root.val != subRoot.val)
            return false;
        else
            return isSame(root.left,subRoot.left) && isSame(root.right,subRoot.right);
    }
}
```



## 226.Invert Binary Tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

![image-20221212112211391](https://s2.loli.net/2022/12/13/KA1R4rNc9jdLkXW.png)

Just simply swap each node's left root value and right root value

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        // return typr is TreeNode, but nothong to return so just return null
        if(root == null) return null;

        TreeNode tempRoot = root.left;
        root.left = root.right;
        root.right = tempRoot;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }    
}

```

## 102.Binary Tree Level Order Traversal

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```



![image-20221215155852384](https://s2.loli.net/2022/12/16/mhRBFbK6Y4iI1UA.png)



```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null) return new ArrayList<>(new ArrayList<>());
        
        Queue<TreeNode> q = new LinkedList<>();
        
        q.add(root);
        
        List<List<Integer>> list = new ArrayList<>();
        
        while(!q.isEmpty()){
            
            List<Integer> subList = new ArrayList<>();
            
            int size = q.size();
            
            for(int i = 0; i < size;i++){
               TreeNode node = q.peek();
                
                if(node.left !=null ){
                   q.offer(node.left);
                }
                
                if(node.right != null){
                    q.offer(node.right);
                }
                subList.add(q.poll().val);
        
            }
            list.add(subList);
            
        }
        return list;
    }                                                                   
}
```





# ************Array************

## 1480.Running Sum of 1d Array

**Example 1:**

```java
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```

Kind of like a small DP question

```java
class Solution {
    public int[] runningSum(int[] nums) {
        int[] res = new int[nums.length];
        
        res[0] = nums[0];
        
        for(int i = 1; i < nums.length; i++){
            res[i] = res[i-1] + nums[i];
        }
        
        return res;
    }
}
```

## 724.Find Pivot Index

### 1.Brute Force 

 Calculate the left sum and then calculate the right sum,O(n^2)

```java
class Solution {
    public int pivotIndex(int[] nums) {
        
        for(int i = 0 ; i < nums.length; i++){
            int sumLeft = 0;
        
            int sumRight = 0;
            
            for(int j = 0; j < i ; j++){
                sumLeft = sumLeft + nums[j];
            }
            
            for(int k = i + 1; k < nums.length; k++){
                sumRight = sumRight + nums[k];
            }
          
            if(sumLeft == sumRight) return i;
        }
        
        return -1;
        
    }
}
```

### 2. Prefix Sum

Firstly calculate the sum of the whole array.

Then minus the left part and compare it to the right part. O(n)

```java
class Solution {
    public int pivotIndex(int[] nums) {
        
        int leftSum = 0; 
        
        int rightSum = 0;
        
        int sum = 0;
        
        for(int n : nums) sum = sum + n;
        
        for(int i = 0; i < nums.length; i++){
            rightSum = sum - nums[i];
            
            sum = sum - nums[i];
            
            //rightSum = sum - nums[i] - leftSum;
                
            if(leftSum == rightSum) return i;
            
            leftSum = leftSum + nums[i];
        }
        
        return -1;
    }
}
```

# ************Implementation/Simulation************

## 202. Happy Number

**Example 1:**

```java
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

Firstly we iterate to update the value of n, and calculate its sum, if sum == 1 return true. 

Secondly, we use a hash map to store the sum, if there's a repeat sum, there must be a loop, so we return false,n is not a happy number.

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<>();
        while(true){
            n = calculateSum(n);
            
            if(set.contains(n)) 
                return false;
            else
                set.add(n);
            
            if(n == 1) return true;
            
        }
    }
    //calculate the sum 
    //this method should be familiar
    private int calculateSum(int n){
        int temp = 0;
        int sum = 0;
            while(n > 0){
                temp = n % 10;
                sum = sum + temp*temp;
                n = n/10;
            }
        return sum;
    }
}
```

Another edition

```java
 public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<>();
        
        while( n != 1 && !set.contains(n)){
            set.add(n);            
            n = calculateSum(n);  
        }
  
        return n == 1;
 }
```

## 1706.Where Will the Ball Fall

```java
class Solution {
    public int[] findBall(int[][] grid) {
        
        int row = grid.length;
        
        int col = grid[0].length;
        
        int[] res = new int[col];
        
        for(int j = 0 ; j < col; j++ ){//control ball
            int x = 0; int y =j;
            while(x < row){//control ball at which row
                if(grid[x][y] == 1){
                    if(y == col -1) 
                        break;
                    else if(grid[x][y+1] == -1)
                        break;
                    else
                        x++;y++;
                }
                else{
                    if(y == 0) 
                        break;
                    else if(grid[x][y-1] == 1)
                        break;
                    else
                        x++;y--;
                }
            }
            
            if(x == row)
                res[j] = y;    
            else
                res[j] = -1;
            
        }
        
        return res;
    }
}
```

# ************Linked List************

## 234.Palindrome Linked List

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```java
Input: head = [1,2]
Output: false
```

**Find the middle point of the linked list. ** Use fast and slow pointer

**Then reverse.**   null pre pointer used

 **Compare.** while loop, after reverse the length is already the same so just compare the vaule

Pretty interesting many small methods used

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        
        ListNode mid = findMid(head);
        
        ListNode reversedHead = reverse(mid);
        
        return isSame(head,reversedHead);
    }
    
    private ListNode findMid(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        if(fast != null) //odd length
            slow = slow.next;

        return slow;
    }
    
    private ListNode reverse(ListNode head){
        ListNode pre = null;
        while(head != null){
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
    
    private boolean isSame(ListNode head, ListNode reverseHead){
        while(head != null && reverseHead != null){
            if(head.val == reverseHead.val){
                head = head.next;
                reverseHead = reverseHead.next;
            }else{
                return false;
            }    
        }
        return true;
        
    }
     
}
```

## 19.Remove Nth Node From End of List

### 1.Method written by myself faster than 100%

reverse-->remove-->reverse

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
         head = reverse(head);
        
         head = remove(head,n);
        
         head = reverse(head);
        
        return head;
    }
    
    private ListNode reverse(ListNode head){
        ListNode pre = null;
        while(head != null){
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
    
    private ListNode remove(ListNode head, int n){   
        if(n == 1){
            head = head.next;
            return head;
        }
        ListNode temp = head;
        int count = 1;
        while(temp != null){
            if(count == n-1){
                temp.next = temp.next.next;
                break;
            }else{
                count++;
                temp=temp.next;
            }
        }
        return head;
    }
}
```

### 2.Two pointer

Use fast and slow two pointer, let faster pointer move forward n steps, then fast and slow move forward together until fast.next == null. Then remove.

**return dummy.next** because the first node may be deleted

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0) ;
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;
        
        for(int i = 0 ; i < n; i++){
            fast = fast.next;
        }
        while(fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        
        return dummy.next;
    }
}
```

# ************Binary Search************

## 33.Search in Rotated Sorted Array

```java
class Solution {
    public int search(int[] nums, int target) {
        //binary search can only used in sorted arrays
        //so we need to search in two arrays seperately
        //so we firstly find the peek point, the turing point 
        //then divide the array into two sorted arrays
        
        int peekIndex = findPeekIndex(nums);
        
        if(target >= nums[0] && target <= nums[peekIndex]){
            return binarySearch(nums,0,peekIndex,target);
        }else{
            return binarySearch(nums,peekIndex+1,nums.length-1,target);
        }

    }
    
    private int findPeekIndex(int[] nums){
        int left = 0;
        int right = nums.length-1;
        if(nums[left] <= nums[right]) return right;
        
        while(left <= right){
            int mid = (left + right)/2;
            if(nums[mid] > nums[mid+1]) return mid;
            else if(nums[left] <= nums[mid]) left = mid + 1;
            else right = mid -1;
        }
        
        return nums.length - 1;
    }
    
    private int binarySearch(int[] nums, int left, int right, int target){
        
        while(left <= right){
            int mid = (left + right)/2;
            if(nums[mid] == target) return mid;
            else if(nums[mid] > target) right = mid - 1;
            else left = mid + 1;
            
        }
        return -1;
    }
}
```

# ************BST************

## 94.Binary Tree Inorder Traversal

### 1.Recursive

```java
class Solution {
    List<Integer> list = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null) return list;
        
        inorderTraversal(root.left);
        list.add(root.val);
        inorderTraversal(root.right);
        
        return list;
        
    }
}
```

### 2.STACK

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        
        TreeNode curr = root;
        
        while(curr != null || !stack.isEmpty()){
            while(curr != null){
                stack.push(curr);
                curr = curr.left;
            }
            
            curr = stack.pop();
            list.add(curr.val);
            curr = curr.right;
        }
        
        return list;
        
        
    }
}
```

## 230.Kth Smallest Element in a BST

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

**One thing to keep in mind: The in-order traversal of a BST in an ordered array!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!**

### 1.STACK

```JAVA
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        int count = 0;
        while(curr != null || !stack.isEmpty()){
            while(curr != null){
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            count++;
            if(count == k) return curr.val;
            curr = curr.right;   
        }
        return 0;
    }
}
```

### 2.Recursive

```java
class Solution {
    List<Integer> list = new ArrayList<>();
    public int kthSmallest(TreeNode root, int k) {
       
        inorderTraversal(root);
        
        return list.get(k-1);
    }
    
    private void inorderTraversal(TreeNode root){
        if(root == null) return ;
        
        inorderTraversal(root.left);
        list.add(root.val);
        inorderTraversal(root.right);
        
    }
}
```



## 108.Convert Sorted Array to Binary Search Tree

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums == null) return null;
        return consturctTreeFromArray(nums, 0, nums.length-1);
        
    }
    
    private TreeNode consturctTreeFromArray(int[] nums, int left, int right){
        if(left > right) return null;
            
        int mid = (left + right)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = consturctTreeFromArray(nums,left,mid-1);
        root.right = consturctTreeFromArray(nums,mid+1,right);
        
        return root;
        
    }
}
```

## 173.Binary Search Tree Iterator

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```java
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False
```

```java
class BSTIterator {
    
    List<Integer> list = new ArrayList<>();
    int index;

    public BSTIterator(TreeNode root) {
        inorder(root);
    }
    public void inorder(TreeNode root){
        if(root == null) return;
        inorder(root.left);
        list.add(root.val);
        inorder(root.right);
    }
    
    public int next() {
        return list.get(index++);
    }
    
    public boolean hasNext() {
        return index < list.size();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

# ************Graph************

## 207.Course Schedule(bfs,topological sort)

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

![image-20230104102242669](https://s2.loli.net/2023/01/05/goz9avGE8ADFTcl.png)

**This is a typical topological sort problem**

https://www.bilibili.com/video/BV1pV4y1K75T/?spm_id_from=333.788.recommend_more_video.-1&vd_source=2611113609157526cc6cbc12a16fb322

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
          int[] indegree = new int[numCourses];
        //every vertex's indegree++ (every edges)
        for(int i = 0; i < prerequisites.length;i++){
            indegree[prerequisites[i][0]]++;
        }
        //next setp is to decrease the indegree(start from indegree=0)
        //when the all the indegree = 0, there is no cycle
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < indegree.length; i++){
            if(indegree[i] == 0) queue.offer(i); //start point
        }
        
        while(!queue.isEmpty()){
            int currentVertex = queue.poll();
            for(int i = 0 ; i < prerequisites.length; i++){
                if(prerequisites[i][1] == currentVertex){
                    indegree[prerequisites[i][0]]--;
                    if(indegree[prerequisites[i][0]] == 0) 
                        queue.offer(prerequisites[i][0]);
                }
            }
        }
        //Iterate through the indegree[] 
        //to see if there are any elements that are not 0
        //if so, there would be a cycle
        for(int n : indegree){
            if(n != 0) return false;
        }
        return true;
        
    }
}
```

## 210.Course Schedule II(bfs)

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        
        int[] indegree = new int[numCourses];
        //traversal prerequisites
        for(int i = 0; i < prerequisites.length; i++){
            indegree[prerequisites[i][0]]++;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        //traversal indegree[] add 0-value element into queue
        for(int i=0 ; i < indegree.length; i++){
            if(indegree[i] == 0)
                queue.offer(i);
        }
        
        int[] coureOrder = new int[numCourses];
        int index = 0;//index of courseOrder
        //next step pop queue and decrase indegree(edges)
        while(!queue.isEmpty()){
            int currentCourse = queue.poll();
            coureOrder[index++] = currentCourse;
            //traversal prerequisites
            for(int i = 0; i < prerequisites.length; i++){
                if(prerequisites[i][1] == currentCourse){
                    indegree[prerequisites[i][0]]--;
                    if(indegree[prerequisites[i][0]] == 0)
                        queue.offer(prerequisites[i][0]);
                }            
            }
            
        }
        
        for(int n : indegree){
            if(n != 0) 
                return new int[]{};
        }
        return coureOrder;
    }
}
```

## 269.Alien Dictionary







## 200.Number of Islands(dfs)

A simple dfs problem

```java
class Solution {
    public int numIslands(char[][] grid) {
        
        int count = 0;
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        
        for(int i = 0; i < grid.length;i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == '1' && !visited[i][j]){
                    count++;
                    dfs(grid,i,j,visited);
                }
            }
        }
        
        return count;

    }
    
    public void dfs(char[][] grid, int i, int j, boolean[][] visited){
        
        if(i<0 || i>=grid.length || j<0 || j>=grid[0].length || grid[i][j]=='0'
          || visited[i][j])  return;
        
        visited[i][j] = true;
        
        dfs(grid,i+1,j,visited);
        dfs(grid,i-1,j,visited);
        dfs(grid,i,j+1,visited);
        dfs(grid,i,j-1,visited);

    }
}

```

another edition 

```java
        int count = 0;
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length;j++){
                if(grid[i][j] == '1'){
                    dfs(grid,i,j);
                    count++;
                }
            }
        }
        return count;
        
        
    }
    public void dfs(char[][] grid,int i, int j){
        if(i < 0 || j < 0 || i > grid.length-1 || j > grid[0].length-1 || grid[i][j] == '0') 
            return;
        
        grid[i][j] = '0';
        dfs(grid,i + 1,j);
        dfs(grid,i - 1,j);
        dfs(grid,i,j + 1);
        dfs(grid,i,j - 1);
```

## 133.Clone Graph

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png)

```java
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/
class Solution {
    Node[] visited = new Node[101];
    
    public Node cloneGraph(Node node) {
        if(node == null) return null;
        
        Node copy = new Node(node.val);
        
        dfs(copy,node);
        
        return copy;
        
    }
    
    public void dfs(Node copy, Node node){
        visited[copy.val] = copy;
        
        for(Node neighbor : node.neighbors){
            
            if(visited[neighbor.val] == null){
                Node newNode = new Node(neighbor.val);
                copy.neighbors.add(newNode);
                dfs(newNode,neighbor);
            }else{
                copy.neighbors.add(visited[neighbor.val]);
                
            }
         }       
    }
}
```



## 128.Longest Consecutive Sequence

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

### 1.Set

```java
class Solution {
    public int longestConsecutive(int[] nums) {

         if(nums.length == 0 || nums == null) return 0;
        
         //put all the nums in the set 
         //then find the beginning number then traversal the beginning number
         Set<Integer> set = new HashSet<>();
         for(int n : nums) set.add(n);
         int res = 1;
         int count = 1;

        //O(n+n)
         for(int num : set){
             if(!set.contains(num - 1)){//find left 
                 count = 1;
                 while(set.contains(num + 1)){
                     count++;
                     num++;
                 }
                 res = Math.max(res,count);
             }
         }
         return res;
    }
}

```

### 2.sort method but O(nlogn)

```java
// //         if(nums.length == 0 || nums == null) return 0;
        
// //         Arrays.sort(nums); //nlogn
        
// //         // for(int n : nums) System.out.print(n);
        
// //         int count = 1;
// //         int res = 1;
// //         for(int i = 0; i < nums.length - 1; i++){
// //             if(nums[i+1] == nums[i]) continue;
// //             if(nums[i+1] - nums[i] == 1){
// //                 count++;  
// //                 res = Math.max(res,count);
// //             }else{
// //                 count = 1;
// //             }
// //         }
// //           return res;
```



## 261.Graph Valid Tree

You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer n and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.

Return `true` *if the edges of the given graph make up a valid tree, and* `false` *otherwise*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg)

```
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
Output: false
```

### 1.Iterative Breadth-First Search.

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        
        // if not satisfied, must have a circle
        if(edges.length != n-1) return false;
        
        //then we just need to check there's isolate node
        
        List<List<Integer>> adjacencyList = new ArrayList<>();
        
        for(int i = 0; i < n; i++){
            adjacencyList.add(new ArrayList<>());
        }
        
        for(int[] edge : edges){
            adjacencyList.get(edge[0]).add(edge[1]);
            adjacencyList.get(edge[1]).add(edge[0]);
        }
        
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        
        Set<Integer> set = new HashSet<>();
        
        while(!queue.isEmpty()){
            int node = queue.poll();
            if(!set.contains(node)) set.add(node);
            
            for(int neighbour : adjacencyList.get(node)){
            
                if(set.contains(neighbour)) continue;
                
                set.add(neighbour);
            
                queue.add(neighbour);
        }

    }
             return set.size() == n;

}
}
```

### 2.Recursive Depth-First Search.

```java
class Solution {
    List<List<Integer>> adjacencyList = new ArrayList<>();
    Set<Integer> visited = new HashSet<>();
    
    public boolean validTree(int n, int[][] edges) {
       if(edges.length + 1 != n) return false;
        
        
        for(int i =0; i < n; i++){
            adjacencyList.add(new ArrayList<>());
        }
        
        for(int[] edge : edges){
            adjacencyList.get(edge[0]).add(edge[1]);
            adjacencyList.get(edge[1]).add(edge[0]);
        }
        
        dfs(0);    

        return visited.size() == n;
}
    
    private void dfs(int node){
        
        if(visited.contains(node)) return;

        visited.add(node);
        
        for(int neighbor : adjacencyList.get(node)){
            
            dfs(neighbor);
            
        }
        
    }
}
```

## 323.Number of Connected Components in an Undirected Graph

A little bit like 261, use adjacency list to dfs or bfs

### 1.DFS

```java
class Solution {
        boolean[] visited;
        List<List<Integer>> adjacencyList = new ArrayList<>();
    public int countComponents(int n, int[][] edges) {
        visited = new boolean[n];
        for(int i = 0 ; i < n; i++){
            adjacencyList.add(new ArrayList<>());
        }
        for(int[] edge : edges){
            adjacencyList.get(edge[0]).add(edge[1]);
            adjacencyList.get(edge[1]).add(edge[0]);

        }
        
        int count = 0;
        
        for(int i = 0; i < adjacencyList.size(); i++){
            if(!visited[i]){
                count++;
                visited[i]=true;
                dfs(adjacencyList.get(i));
            }
        }
        return count;
    }
    
    public void dfs(List<Integer> nodes){
        for(int node : nodes){
            if(visited[node]) continue;
            visited[node] = true;
            dfs(adjacencyList.get(node));
        }
    }
}
```

another edition for dfs

```java
class Solution {
        boolean[] visited;
        List<List<Integer>> adjacencyList = new ArrayList<>();
    public int countComponents(int n, int[][] edges) {
        visited = new boolean[n];
        for(int i = 0 ; i < n; i++){
            adjacencyList.add(new ArrayList<>());
        }
        for(int[] edge : edges){
            adjacencyList.get(edge[0]).add(edge[1]);
            adjacencyList.get(edge[1]).add(edge[0]);

        }
        
        int count = 0;
        
        for(int i = 0; i < adjacencyList.size(); i++){
            if(!visited[i]){
                count++;
                dfs(i);
            }
        }
        return count;
    }
    
    public void dfs(int node){
        if(visited[node]) return;
        
        visited[node] = true;
        
        for(int neighbour : adjacencyList.get(node)){
            dfs(neighbour);
        }
    }
}
```

### 2.union find

```java
class Solution {
      
    int[] parent;
    
    public int countComponents(int n, int[][] edges) {
       
        parent = new int[n];
        for(int i = 0 ; i < n; i++){
            parent[i] = i;
        }
        
        for(int[] edge : edges){
            union(edge[0],edge[1]);
        }
        
        Set<Integer> set = new HashSet<>();
        
        //avoid cycle, so use find
        for(int i = 0 ; i < n; i++){
            set.add(find(i));
        }
        
        return set.size();
        
    }
    public int find(int x){
        if(parent[x] != x)
            return find(parent[x]);
        else 
            return x;
    }
    
    public void union(int node1, int node2){
        
        parent[find(node1)] = find(node2);
        
    }
    
}
```



## 994.Rotting Oranges

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<int[]> queue = new LinkedList<>();
        int freshNumber = 0;
        int minute = 0;
        
        for(int i = 0 ; i < grid.length; i++){
            for(int j = 0 ; j < grid[0].length; j++){
                if(grid[i][j] == 2) 
                    queue.offer(new int[]{i,j});
                else if(grid[i][j] == 1) 
                    freshNumber++; 
            }
        }
        
        if(freshNumber == 0) return 0;
        
        int[][] dirs = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
        
        while(!queue.isEmpty()){
            minute++;
            int size = queue.size();
            for(int i = 0; i < size; i++){
                int[] point = queue.poll();
                for(int[] dir : dirs){
                    int x = point[0] + dir[0];
                    int y = point[1] + dir[1];
                    
                    if(x < 0 || x > grid.length - 1 || 
                       y <0 || y > grid.length - 1 ||
                      grid[x][y] == 0 || grid[x][y] == 2) continue;
                    
                    grid[x][y] = 2;
                    queue.offer(new int[]{x,y});
                    freshNumber--;
                }
            }
        }
        
        return freshNumber == 0 ? minute -1 : -1；
        
    }
}
```

# Dynamic Programming
