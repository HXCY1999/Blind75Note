[top]

# Blind75Note

My second time doing these problem, so I wrote some note to help my understand these problem deeply.

# String

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

dic[s.charAt(i) - 'A']: put 26 character in certain place

dic[0]-A,dic[1]-B,dic[2]-C,,,dic[26]-Z

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

# Binary

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

