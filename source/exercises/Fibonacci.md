---
title: 数组练习题-Fibonacci
date: 2019-04-10 09:48:22
categories:  #分类
    - structure
tags:   #标签
    - basic
    - 数据结构
    - 数组
description: 
    练习题.
---
```Java
	//执行用时 : 15 ms, 在Fibonacci Number的Java提交中击败了44.97% 的用户
	//内存消耗 : 32.2 MB, 在Fibonacci Number的Java提交中击败了87.19% 的用户
	public int fib(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        return fib(n-1)+fib(n-2);
    }
    //执行用时 : 0 ms, 在Fibonacci Number的Java提交中击败了100.00% 的用户
	//内存消耗 : 32.4 MB, 在Fibonacci Number的Java提交中击败了82.18% 的用户
	public int fib(int n) {
        if(n==1) return 1;
        if(n==0) return 0;
        int pre=1;
        int prepre=0;
        int result=0;
        for(int i=2;i<n+1;i++){
            result=pre+prepre;
            prepre=pre;
            pre=result;
        }
        return result;
    }



```