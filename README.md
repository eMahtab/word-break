# Word Break
## https://leetcode.com/problems/word-break

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

1. The same word in the dictionary may be reused multiple times in the segmentation.
2. You may assume the dictionary does not contain duplicate words.
```
Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".

Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.

Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## Initial Thought Process : Will not work
Try to match dictionary words in the given string, if the dictionary word matches, update the given string by removing the matched dictionary word.
Since we can use same dictionary word multiple times to create the given string, try to find a match of the dictionary word in the given string as many times as we can.
If the given string becomes an empty string after update, it means we can create the given string from the given dictionary words, so we return true.   

But soon we realize, that this approach is not going to work.
```
String s "cars"  ,  wordDict = ["car","ca","rs"]

After `car` is matched the updated string will become just `s` and our program will return false, since there is no single `s` in wordDict.
```

# Implementation 1 : Recursive (Time Limit Exceeded 😀)
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s == null)
            return false;
        // Add the dictionary words to a set for faster lookup
        Set<String> set = new HashSet<>();
        for(String str : wordDict)
            set.add(str);
        
        return helper(s, set);
    }
    
    private boolean helper(String s, Set<String> set) {
        if(s.length() == 0)
            return true;
        for(int i = 0; i < s.length(); i++) {
            String s1 = s.substring(0, i+1);
            if(set.contains(s1) && helper(s.substring(i+1), set))
                return true;
        }
        return false;
    }
}
```

# Implementation 2 : Memoization
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s == null)
            return false;
        // Add the dictionary words to a set for faster lookup
        Set<String> set = new HashSet<>();
        for(String str : wordDict)
            set.add(str);
        Map<String,Boolean> map = new HashMap<>();
        return helper(s, set, map);
    }
    
    private boolean helper(String s, Set<String> set, Map<String, Boolean> map) {
        if(s.length() == 0)
            return true;
        if(map.containsKey(s))
            return map.get(s);
        for(int i = 0; i < s.length(); i++) {
            String s1 = s.substring(0, i+1);
            if(set.contains(s1) && helper(s.substring(i+1), set, map)) {
                map.put(s, true);
                return true;
            }
        }
        map.put(s, false);
        return false;
    }
}
```

# Implementation 3 : Dynamic Programming
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        boolean[] isWordBreak = new boolean[s.length()+1];
        isWordBreak[0] = true;
        
        for(int i = 1; i <= s.length(); i++) {
            for(int j = 0; j < i; j++) {
                if(isWordBreak[j] && wordDictSet.contains(s.substring(j,i))) {
                    isWordBreak[i] = true;
                    break;
                }
            }
        }
        return isWordBreak[s.length()];
    }
}
```

# References :
1. https://www.youtube.com/watch?v=hLQYQ4zj0qg (Very good explanation)
2. https://www.youtube.com/watch?v=YxtQUbKbdUs
