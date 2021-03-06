LeetCode\_R
================
Xingyan “David” Wang
2021-04-09

## Introduction

This will be a self practice documentation

Questions are obtained from LeetCode webiste at: <https://leetcode.com/problemset/all/>

Instead of using some specific packages, I will try to use basic R to solve these problems.

## Question 1: Two Sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

``` r
twoSum<-function(nums, target){
  allsum<-matrix(NA, ncol = length(nums), nrow = length(nums))
  for(i in 1:length(nums)){
    for(j in (i):length(nums)){
      allsum[i, j] = nums[i] + nums[j]
    }
  }
  diag(allsum) = NA
  loc<-which(allsum==target, arr.ind = T)
  return(loc)
}

# test examples
twoSum(nums = c(2,7,11,15), target = 9)
```

    ##      row col
    ## [1,]   1   2

``` r
twoSum(nums = c(3,2,4), target = 6)
```

    ##      row col
    ## [1,]   2   3

``` r
twoSum(nums = c(3,3), target = 6)
```

    ##      row col
    ## [1,]   1   2

Finshed time: 3/16/2021

## Question 2: Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list. You may assume the two numbers do not contain any leading zero, except the number 0 itself.

``` r
addTwoNumbers<-function(list1, list2){
  value1<-as.numeric(paste(rev(list1),collapse=""))
  value2<-as.numeric(paste(rev(list2),collapse=""))
  
  value3 <- value1+value2
  
  final<-rev(unlist(strsplit(as.character(value3), split = "")))
  
  return(final)
}

# test examples
addTwoNumbers(list1 = c(2,4,3), list2 = c(5,6,4))
```

    ## [1] "7" "0" "8"

``` r
addTwoNumbers(list1 = 0, list2 = 0)
```

    ## [1] "0"

``` r
addTwoNumbers(list1 = c(9,9,9,9,9,9,9), list2 = c(9,9,9,9))
```

    ## [1] "8" "9" "9" "9" "0" "0" "0" "1"

Finshed time: 3/16/2021

## Question 3: Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without repeating characters.

``` r
lengthOfLongestSubstring<-function(s){
  seperate<-unlist(strsplit(s, split = ""))
  
  if(length(seperate)==0){
    final<-0
  } else {
    max_length<-NULL
    for(i in 1:length(seperate)){
      j <- i
      
      unique_length<-length(unique(seperate[j]))
      overall_length<-length((seperate[j]))
      
      ix.next<-unique_length==overall_length
      
      while(ix.next ){
        j<-j+1
        unique_length<-length(unique(seperate[i:j]))
        overall_length<-length((seperate[i:j]))
        ix.next <- unique_length==overall_length & (j <= length(seperate))
      } 
      max_length<-c(max_length, j-i)
    }
    final<-seperate[which.max(max_length):(which.max(max_length)+max(max_length)-1)]
    final<-paste(final, collapse="")
    
  }
  return(final)
}

# test examples
lengthOfLongestSubstring(s = "abcabcbb")
```

    ## [1] "abc"

``` r
lengthOfLongestSubstring(s = "bbbbb")
```

    ## [1] "b"

``` r
lengthOfLongestSubstring(s = "pwwkew")
```

    ## [1] "wke"

``` r
lengthOfLongestSubstring(s = "")
```

    ## [1] 0

Finshed time: 3/23/2021

## Question 4: Median of Two Sorted Arrays

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

``` r
findMedianSortedArrays<-function(nums1, nums2){

  merged<-unique(c(nums1, nums2))
  final<-median(merged, na.rm = T)
  
  return(final)
}


findMedianSortedArrays(nums1 = c(1,3), nums2 = c(2))
```

    ## [1] 2

``` r
findMedianSortedArrays(nums1 = c(1,2), nums2 = c(3,4))
```

    ## [1] 2.5

``` r
findMedianSortedArrays(nums1 = c(0,0), nums2 = c(0,0))
```

    ## [1] 0

``` r
findMedianSortedArrays(nums1 = c(), nums2 = c(1))
```

    ## [1] 1

``` r
findMedianSortedArrays(nums1 = c(2), nums2 = c())
```

    ## [1] 2

Finshed time: 3/23/2021

## Question 5: Longest Palindromic Substring

Given a string s, return the longest palindromic substring in s. Note: A palindrome is a word, number, phrase, or other sequence of characters which reads the same backward as forward, such as madam or racecar.

``` r
longestPalindrome<-function(s){
  seperate<-unlist(strsplit(s, split = ""))
  
  if(length(seperate)==0){
    final<-0
  } else {
    max_length<-NULL
    for(i in 1:length(seperate)){
      start_string<-seperate[i]
      
      if(i==length(seperate)){
        end_string<-integer(0)
      } else {
        end_string<-which(seperate[i+1:length(seperate)]==start_string)+i
      }
      
      if(length(end_string)==0){
        max_length<-c(max_length, 1)
      } else {
        candidate<-seperate[i:end_string]
        candidate_rev<-rev(candidate)
        ix.pali<-sum(candidate ==  candidate_rev) == length(candidate)
        if(ix.pali){
          max_length<-c(max_length, length(candidate))
        } else {
          max_length<-c(max_length, 0)
        }
      }
    }
  
    final<-seperate[which.max(max_length):(which.max(max_length)+max(max_length)-1)]
    final<-paste(final, collapse="")
  }
  return(final)
}

longestPalindrome(s = "babad")
```

    ## [1] "bab"

``` r
longestPalindrome(s = "cbbd")
```

    ## [1] "bb"

``` r
longestPalindrome(s = "a")
```

    ## [1] "a"

``` r
longestPalindrome(s = "ac")
```

    ## [1] "a"

Finshed time: 3/23/2021

## Question 6: ZigZag Conversion

## Question 7: Reverse Integer

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range \[ − 2<sup>31</sup>, 2<sup>31</sup> − 1\], then return 0.

``` r
reverse<-function(x){
  if(x>=2^31-1 | x<=-2^31){
    final = 0
  } else {
    abs_x_rev<-as.numeric(paste(rev(unlist(strsplit(as.character(abs(x)), split = ""))), collapse=""))
    final<-sign(x)*abs_x_rev
  }
  return(final)
}

reverse(x=123)
```

    ## [1] 321

``` r
reverse(x=-123)
```

    ## [1] -321

``` r
reverse(x=120)
```

    ## [1] 21

``` r
reverse(x=0)
```

    ## [1] 0

Finshed time: 3/23/2021

## Question 8: String to Integer (atoi)

Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).

The algorithm for myAtoi(string s) is as follows:

1.  Read in and ignore any leading whitespace.

2.  Check if the next character (if not already at the end of the string) is '-' or '+'. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.

3.  Read in next the characters until the next non-digit charcter or the end of the input is reached. The rest of the string is ignored.

4.  Convert these digits into an integer (i.e. "123" -&gt; 123, "0032" -&gt; 32). If no digits were read, then the integer is 0. Change the sign as necessary (from step 2).

5.  If the integer is out of the 32-bit signed integer range \[ − 2<sup>31</sup>, 2<sup>31</sup> − 1\], then clamp the integer so that it remains in the range. Specifically, integers less than −2<sup>31</sup> should be clamped to −2<sup>31</sup>, and integers greater than 2<sup>31</sup> − 1 should be clamped to 2<sup>31</sup> − 1.
6.  Return the integer as the final result.

``` r
myAtoi<-function(s){
  seperate<-unlist(strsplit(s, split = ""))
  
  seperate_num<-as.numeric(seperate)
  
  sign_loc<-min(which(!is.na(seperate_num)))-1
  
  if(sign_loc>0){
    if(seperate[sign_loc] == "+"){
      sign_num<-1
    } else if (seperate[sign_loc] == "-"){
      sign_num<- -1
    } else {
      sign_num <- 1
    }
  } else{
    sign_num <- 1
  }
  
  num<-as.numeric(paste(seperate[which(!is.na(seperate_num))], collapse=""))
  final<-num*sign_num
  
  if(final>=2^31-1 ){
    final = 2^31-1
  } else if ( final<=-2^31) {
    final  = -2^31
  } else {
    final <- final
  }
  
  return(final)
}

myAtoi(s = "42" )
```

    ## [1] 42

``` r
myAtoi(s = "   -42")
```

    ## Warning in myAtoi(s = " -42"): NAs introduced by coercion

    ## [1] -42

``` r
myAtoi(s = "4193 with words" )
```

    ## Warning in myAtoi(s = "4193 with words"): NAs introduced by coercion

    ## [1] 4193

``` r
myAtoi(s = "words and 987" )
```

    ## Warning in myAtoi(s = "words and 987"): NAs introduced by coercion

    ## [1] 987

``` r
myAtoi(s = "-91283472332" )
```

    ## Warning in myAtoi(s = "-91283472332"): NAs introduced by coercion

    ## [1] -2147483648

Finshed time: 3/23/2021

## Question 9: Palindrome Number

Given an integer x, return true if x is palindrome integer.

Note: An integer is a palindrome when it reads the same backward as forward. For example, 121 is palindrome while 123 is not.

``` r
isPalindrome<-function(x){
  if(x<0){
    final<-FALSE
  } else {
    rev_x<-as.numeric(paste(rev(as.numeric(unlist(strsplit(as.character(x), split = "")))), collapse = ""))
    if(x==rev_x){
      final<-TRUE
    } else {
      final<-FALSE
    }
  }
  return(final)
}

isPalindrome(x = 121)
```

    ## [1] TRUE

``` r
isPalindrome(x = -121)
```

    ## [1] FALSE

``` r
isPalindrome(x = 10)
```

    ## [1] FALSE

``` r
isPalindrome(x = -101)
```

    ## [1] FALSE

Finshed time: 3/23/2021

## Question 10: Regular Expression Matching

Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '\*' where:

'.' Matches any single character.

'\*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

``` r
isMatch<-function(s, a){
  
}
```

## Question 11: Container With Most Water

Given n non-negative integers *a*<sub>1</sub>, *a*<sub>2</sub>, ..., *a*<sub>*n*</sub> , where each represents a point at coordinate (*i*, *a*<sub>*i*</sub>). *n* vertical lines are drawn such that the two endpoints of the line i is at (*i*, *a*<sub>*i*</sub>) and (*i*, 0). Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

Notice that you may not slant the container.

``` r
maxArea<-function(){
  maxArea<-function(height){
  area<-matrix(NA, nrow = length(height), ncol = length(height))
  
  for(i in 1:length(height)){
    for(j in i:length(height)){
      
      depth<-min(c(height[i], height[j]))
      volumn<-depth*(j-i)
      area[i, j] <- volumn
      area[j, i] <- volumn
    }
  }
  final<-max(area)
  return(final)
}
maxArea(height = c(1,8,6,2,5,4,8,3,7))
maxArea(height = c(1,1))
maxArea(height = c(4,3,2,1,4))
maxArea(height = c(1,2,1))
}
```

Finshed time: 3/24/2021

## Question 12: Integer to Roman

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol Value I 1 V 5 X 10 L 50 C 100 D 500 M 1000

For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9.

X can be placed before L (50) and C (100) to make 40 and 90.

C can be placed before D (500) and M (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.

## Question 13:

## Question 14: Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

``` r
longestCommonPrefix<-function(strs){
  strs_split<-strsplit(strs, split = "")
  min_length<-min(unlist(lapply(strs_split, length)))
  
  
  
  common_length<-0
  for(i in 1:min_length){
    elements<-sapply(strs_split, `[`, i)
    if(length(unique(elements))==1){
      common_length<-common_length+1
    } else {
      break
    }
  }
  
  if(common_length==0){
    final<-""
  } else {
    final<-strs_split[[1]][1:common_length]
  }
  
  final<-paste(final, collapse = "")
  
  return(final)
}
longestCommonPrefix(strs = c("flower","flow", "flight"))
```

    ## [1] "fl"

``` r
longestCommonPrefix(strs = c("dog","racecar","car"))
```

    ## [1] ""

Finshed time: 3/29/2021

## Question 15: 3Sum

Given an integer array nums, return all the triplets \[nums\[i\], nums\[j\], nums\[k\]\] such that i != j, i != k, and j != k, and nums\[i\] + nums\[j\] + nums\[k\] == 0.

Notice that the solution set must not contain duplicate triplets.

``` r
threeSum<-function(nums){
  nums_length<-length(nums)
  if(nums_length<3){
    final<-NULL
  } else {
    index<-NULL
    for(i in 1:nums_length){
      for(j in 1:nums_length){
        for(k in 1:nums_length){
          sumall<-nums[i]+nums[j]+nums[k]
          if(sumall==0){
            index<-rbind(index, sort(c(i, j, k)))
          }
        }
      }
    }
    
    index<-unique(index)
    index<-index[!(index[, 1] == index[, 2] | index[, 2] == index[, 3]), ]
    
    final<-NULL
    for (s in 1:dim(index)[1]){
      final<-rbind(final, sort(nums[index[s, ]]))
    }
    final<-unique(final)
  }
  
  
  return(final)

}

threeSum(nums = c(-1,0,1,2,-1,-4))
```

    ##      [,1] [,2] [,3]
    ## [1,]   -1    0    1
    ## [2,]   -1   -1    2

``` r
threeSum(nums = c())
```

    ## NULL

``` r
threeSum(nums = 0)
```

    ## NULL

Finshed time: 3/29/2021

## Question 16: 3Sum Closest

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

``` r
threeSumClosest<-function(nums, target){
  nums_length<-length(nums)
  if(nums_length<3){
    final<-NULL
  } else {
    index<-NULL
    for(i in 1:nums_length){
      for(j in 1:nums_length){
        for(k in 1:nums_length){
          sumall<-nums[i]+nums[j]+nums[k]
          index<-rbind(index, c(sort(c(i,j,k)), sumall))
        }
      }
    }
    
    index<-unique(index)
    index<-index[!(index[, 1] == index[, 2] | index[, 2] == index[, 3]), ]
    
    final<-index[which.min(abs(index[, 4]-target)), 4]
    
    
    
  }
  
  return(final)
}


threeSumClosest(nums = c(-1,2,1,-4), target = 1)
```

    ## [1] 2

Finshed time: 3/29/2021

## Question 17: Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

``` r
letterCombinations<-function(digits){
  number<-c(2:9)
  letters<-c("abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz")
  match_order<-data.frame(number, letters)
  
  if(digits==""){
    final<-""
  } else {
    str_split<-unlist(strsplit(digits, split = ""))
    if(length(str_split)==1){
      possible_string<-unlist(strsplit(match_order[which(match_order[, 1]==str_split), 2], split = ""))
      final<-possible_string
    } else {
      possible_string<-list()
      for(i in 1:length(str_split)){
        mid_string<-str_split[i]
        possible_string[[i]]<-unlist(strsplit(match_order[which(match_order[, 1]==mid_string), 2], split = ""))
      }
      
      combination<-expand.grid(possible_string)
      
      cols <- colnames(combination)
      final<-apply(combination[ , cols] , 1 , paste , collapse = "" )
    }
  }
  return(final)
}

letterCombinations(digits = '23')
```

    ## [1] "ad" "bd" "cd" "ae" "be" "ce" "af" "bf" "cf"

``` r
letterCombinations(digits = '')
```

    ## [1] ""

``` r
letterCombinations(digits = '2')
```

    ## [1] "a" "b" "c"

Finshed time: 4/4/2021

## Question 18: 4Sum

Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

``` r
fourSum<-function(nums, target){
  nums_length<-length(nums)
  if(nums_length<4){
    final<-NULL
  } else {
    index<-NULL
    for(i in 1:nums_length){
      for(j in 1:nums_length){
        for(k in 1:nums_length){
          for (l in 1:nums_length){
            sumall<-nums[i]+nums[j]+nums[k]+nums[l]
            if(sumall==target){
              index<-rbind(index, sort(c(i, j, k, l)))
            }
          }
        }
      }
    }
    
    index<-unique(index)
    index<-index[!(index[, 1] == index[, 2] | index[, 2]==index[, 3] | index[, 3] == index[, 4] | index[, 1]==index[, 3] | index[, 1] == index[, 4] | index[, 2]==index[, 4]), ]
    
    final<-NULL
    for (s in 1:dim(index)[1]){
      final<-rbind(final, sort(nums[index[s, ]]))
    }
    final<-unique(final)
    
  }
  
  return(final)
}

fourSum(nums = c(1,0,-1,0,-2,2), target = 0)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]   -1    0    0    1
    ## [2,]   -2   -1    1    2
    ## [3,]   -2    0    0    2

``` r
fourSum(nums = c(), target = 0)
```

    ## NULL

Finshed time: 4/4/2021

## Question 19: Remove Nth Node From End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

``` r
removeNthFromEnd<-function(head, n){
  if(is.null(head)){
    final<-""
  } else {
    head_rev<-rev(head)
    head_rev_removed<-head_rev[-n]
    
    final<-rev(head_rev_removed)
  }
  return(final)
}


removeNthFromEnd(head = c(1,2,3,4,5), n = 2)
```

    ## [1] 1 2 3 5

``` r
removeNthFromEnd(head = c(), n = 2)
```

    ## [1] ""

``` r
removeNthFromEnd(head = c(1), n = 1)
```

    ## numeric(0)

``` r
removeNthFromEnd(head = c(1,2), n = 1)
```

    ## [1] 1

Finshed time: 4/4/2021

## Question 20: Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '\[' and '\]', determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of brackets.

2.  Open brackets must be closed in the correct order.

## Question 21: Merge Two Sorted Lists

Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

``` r
mergeTwoLists<-function(l1, l2){
  l<-c(l1, l2)
  l<-sort(l)
  
  return(l)
}

mergeTwoLists(l1 = c(1,2,4), l2 = c(1,3,4))
```

    ## [1] 1 1 2 3 4 4

``` r
mergeTwoLists(l1 = c(), l2 = c())
```

    ## NULL

``` r
mergeTwoLists(l1 = c(), l2 = c(0))
```

    ## [1] 0

Finshed time: 4/4/2021

## Question 22: Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

## Question 23: Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

``` r
mergeKLists<-function(lists){
  final<-sort(unlist(lists))
  return(final)
}

mergeKLists(lists = list(c(1,4,5),c(1,3,4),c(2,6)))
```

    ## [1] 1 1 2 3 4 4 5 6

``` r
mergeKLists(lists = list())
```

    ## NULL

``` r
mergeKLists(lists = list(c()))
```

    ## NULL

Finshed time: 4/5/2021

## Question 24: Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

``` r
swapPairs<-function(head){
  if(is.null(head) | length(head)==1){
    final<-head
  } else if(ceiling(length(head)/2)==length(head)/2){
    final<-rep(NA, length(head))
    odd_id<-seq(1, length(head), 2)
    even_id<-seq(2, length(head), 2)
    final[odd_id]<-head[even_id]
    final[even_id]<-head[odd_id]
  } else {
    final<-rep(NA, length(head))
    # the last elements will not swap
    final[length(head)]<-head[length(head)]
    new_head<-head[-length(head)]
    odd_id<-seq(1, length(new_head), 2)
    even_id<-seq(2, length(new_head), 2)
    final[odd_id]<-new_head[even_id]
    final[even_id]<-new_head[odd_id]
  }
  return(final)
}

swapPairs(head = c(1,2,3,4))
```

    ## [1] 2 1 4 3

``` r
swapPairs(head = c())
```

    ## NULL

``` r
swapPairs(head = c(1))
```

    ## [1] 1

``` r
swapPairs(head = c(1:5))
```

    ## [1] 2 1 4 3 5

Finshed time: 4/8/2021

## Question 25: Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

Follow up

Could you solve the problem in O(1) extra memory space?

You may not alter the values in the list's nodes, only nodes itself may be changed.

## Question 26: Remove Duplicates from Sorted Array

Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means a modification to the input array will be known to the caller as well.

``` r
removeDuplicates<-function(nums){
  final<-nums[!duplicated(nums)]
  return(list(length(final), final))
}
removeDuplicates(nums =c(1,1,2) )
```

    ## [[1]]
    ## [1] 2
    ## 
    ## [[2]]
    ## [1] 1 2

``` r
removeDuplicates(nums =c(0,0,1,1,1,2,2,3,3,4) )
```

    ## [[1]]
    ## [1] 5
    ## 
    ## [[2]]
    ## [1] 0 1 2 3 4

Finshed time: 4/8/2021

## Question 27:

## Question 28: Implement strStr()

Source: <https://leetcode.com/problems/implement-strstr/>

Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

## Question 29: Divide Two Integers

Source: <https://leetcode.com/problems/divide-two-integers/>

Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero, which means losing its fractional part. For example, truncate(8.345) = 8 and truncate(−2.7335) = −2.

Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: \[ − 2<sup>31</sup>, 2<sup>31</sup> − 1\]. For this problem, assume that your function returns 2<sup>31</sup> − 1 when the division result overflows.

``` r
divide <- function(dividend, divisor){
  if(divisor==0){
    print("Divisor can not be 0!")
    stop()
  }
  
  number <- dividend/divisor
  if(number >= 0){
    final<-floor(number)
  } else {
    final<-ceiling(number)
  }
  return(final)
}

divide(dividend = 10, divisor = 3 )
```

    ## [1] 3

``` r
divide(dividend = 7, divisor = -3 )
```

    ## [1] -2

``` r
divide(dividend = 0, divisor = 1 )
```

    ## [1] 0

``` r
divide(dividend = 1, divisor = 1 )
```

    ## [1] 1

Finshed time: 4/8/2021

## Question 30: Substring with Concatenation of All Words

Source: <https://leetcode.com/problems/substring-with-concatenation-of-all-words/>

You are given a string s and an array of strings words of the same length. Return all starting indices of substring(s) in s that is a concatenation of each word in words exactly once, in any order, and without any intervening characters.

You can return the answer in any order.

## Question 31: Next Permutation

Source: <https://leetcode.com/problems/next-permutation/>

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.

``` r
nextPermutation<-function(nums){
  if(length(nums)==0){
    final<-NA
  } else if (length(nums)==1){
    final = nums
  } else {
    all_combination<-expand.grid(rep(list(1:length(nums)), length(nums)))
    all_combination<-as.data.frame(all_combination)
    
    ix.keep<-rep(T, dim(all_combination)[1])
    for(i in 1:dim(all_combination)[2]){
      for(j in 1:dim(all_combination)[2]){
        if (i==j){
          ix.keep<-ix.keep
        } else {
          ix.mid<-all_combination[, i]!=all_combination[, j]
          ix.keep<-ix.keep & ix.mid
        }
      }
    }
    
    all_combination<-all_combination[ix.keep, ]
    
    value<-NULL
    for(k in 1:dim(all_combination)[2]){
      value<-cbind(value, nums[all_combination[, k]])
    }
    
    value<-as.data.frame(value)
    
    value$value<-as.numeric(apply(value, 1, paste, collapse = ""))
    value_sorted<-value[order(value$value), ]
    
    value_sorted<-unique(value_sorted)
    
    nums_value<-as.numeric(paste(nums, collapse = ""))
    
    if(nums_value==max(value_sorted$value)){
      final<-value_sorted[1, ncol(value_sorted)]
    } else {
      ix.loc<-which(value_sorted$value==nums_value)
      final<-value_sorted[ix.loc+1, ncol(value_sorted)]
    }
  }
  return(final)
}


nextPermutation(nums = c(1,2,3))
```

    ## [1] 132

``` r
nextPermutation(nums = c(3,2,1))
```

    ## [1] 123

``` r
nextPermutation(nums = c(1,1,5))
```

    ## [1] 151

``` r
nextPermutation(nums = c(1))
```

    ## [1] 1

Finshed time: 4/9/2021

## Question 32:

## Question 33: Search in Rotated Sorted Array

Source: <https://leetcode.com/problems/search-in-rotated-sorted-array/>

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is rotated at an unknown pivot index k (0 &lt;= k &lt; nums.length) such that the resulting array is \[nums\[k\], nums\[k+1\], ..., nums\[n-1\], nums\[0\], nums\[1\], ..., nums\[k-1\]\] (0-indexed). For example, \[0,1,2,4,5,6,7\] might be rotated at pivot index 3 and become \[4,5,6,7,0,1,2\].

Given the array nums after the rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

``` r
search<-function(nums, target){
  if(sum(grepl(target, nums), na.rm=T)>0){
    # minus one because the index should start from 0
    final<-which(nums==target)-1
  } else {
    final<--1
  }
  return(final)
}
search(nums = c(4,5,6,7,0,1,2), target = 0)
```

    ## [1] 4

``` r
search(nums = c(4,5,6,7,0,1,2), target = 3)
```

    ## [1] -1

``` r
search(nums = c(1), target = 0)
```

    ## [1] -1

Finshed time: 4/9/2021

## Question 34: Find First and Last Position of Element in Sorted Array

Source: &lt;&gt;

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return \[-1, -1\].

Follow up: Could you write an algorithm with O(log n) runtime complexity?

``` r
searchRange<-function(nums, target){
  if(sum(grepl(target, nums), na.rm=T)>0){
    # minus one because the index should start from 0
    final<-c(min(which(nums==target))-1, max(which(nums==target))-1)
  } else {
    final<-c(-1, -1)
  }
  return(final)
}

searchRange(nums = c(5,7,7,8,8,10), target = 8)
```

    ## [1] 3 4

``` r
searchRange(nums = c(5,7,7,8,8,10), target = 6)
```

    ## [1] -1 -1

``` r
searchRange(nums = c(), target = 0)
```

    ## [1] -1 -1

Finshed time: 4/9/2021

## Question 35: Search Insert Position

Source: <https://leetcode.com/problems/search-insert-position/>

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

## Question 36:
