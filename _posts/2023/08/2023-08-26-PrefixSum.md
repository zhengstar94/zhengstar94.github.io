---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Prefix Sum"
date: "2023-08-26"
categories: 
  - "Data Structure"
---



# Prefix Sum

## Conception

> Prefix sum is a technique commonly used in mathematics, algorithms, and other fields. It's used to quickly calculate the prefix sum of an array's elements, which is the sum of the elements from the first element to the current element. Prefix sum is a pre-processing that can enhance query efficiency and reduce time complexity during array processing.

{% include figure.liquid loading="eager" path="assets/img/2023/08/26/1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


**So how do we get the sum of the ranges from nums [2] to nums [4]?**

```java
sums[3] == sums[2] + nums[2];
sums[4] == sums[3] + nums[3];
sums[5] == sums[4] + nums[4];

sums[5] == sums[2] + nums[2] + nums[3] + nums[4];
// nums [2] to nums [4]
sums[5] - sums[2] == nums[2] + nums[3] + nums[4];
```

## Question
- You're given a list of integers nums Write a function that returns a boolean representing whether there exists a zero-sum subarray of nums.
- A zero-sum subarray is any subarray where all of the values add up to zero. A subarray is any contiguous section of the array. For the purposes of this problem, a subarray can be as small as one element and as long as the original array.

## Solve

You can tell by the code.<br>
if we can find

```java
sums[j] - sums[i] == 0, then sums[j] == sums[i]
```

then return 'true'

so we can tell the nums=[-5,-5,2,3,5,7],sums=[-5,-10,-8,-5,0,7]

```java
sums[0] == sums[3]  then:
```
```java
nums[1] + nums[2] + nums[3] = 0;
```

So we can get the second formula.
```java
// i < j
if sums[i] == sums[j];
then nums[i,j - 1] sum is 0
```

