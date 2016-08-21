---
layout: post
title: 最长递增子序列
category: Tech
tags: algorithm
keywords: 动态规划
description: .
---

```C++
/*
* @Author: Gxn
* @Date:   2016-07-15 18:34:53
* @Last Modified by:   Gxn
* @Last Modified time: 2016-08-21 20:22:09
*/
#include <iostream>
#include <vector>
#include <ctime>
#include <cstdlib>
using namespace std;

#define MAX  20 
vector<int> longestIncreasSequesence(vector<int> ints);

int main()
{
  //========================数据初始化===================================================

  vector<int> ints;
  srand((unsigned )time(0));
  for (int i = 0; i < MAX; ++i)
  {
    /* code */
    ints.push_back(int(rand()%MAX));
    cout<<ints[i]<<' ';
  }
  cout<<endl;
  //=========================调用算法求解====================================================
  
  vector <int> result = longestIncreasSequesence(ints);
 
  //========================结果输出显示===================================================

  cout<<"最大递增子序列为(长度"<<result.size()<<"):"<<' ';
  for (std::vector<int>::reverse_iterator i = result.rbegin(); i != result.rend(); ++i)
  {
   cout<< *i<<" ";
  }
  cout<<endl;
  //=====================================================================================

    
  return 0;
}


vector<int> longestIncreasSequesence(vector<int> ints)
{
  int len=ints.size();
  int c[len];      //c[i] 记录第i号元素 在ints[0:i-1]中以ints[i]为结尾的递增序列结尾的最长子序列的长度
 
  int max=1;
  int k;
  int sequence[len]; //记录以元素为递增子序列尾的次小数据的下标,前驱数组下标
  
  //========================算法的核心===================================================
  c[0]=1;
  sequence[0]=0;

  for (int i = 1; i < len; ++i)
  {
    c[i]=1;
 
    for (int j = 0; j < i; ++j) // 查找c[0:i-1] 在ints[0:i-1] 中满足以ints[i]作为递增结尾并且最长的长度值
    {

        if(ints[j]<ints[i] && c[j]>c[i]-1)
        {  
           
          c[i]=c[j]+1;
          sequence[i]=j;
          if(max < c[i])
          {
            max=c[i];
            k=i;
          }
        }
      
    }
    
  }
  
  //==================================================================================
 
  vector<int> seq(1,ints[k]);
  for (int i = 0; i < max-1; ++i)
  {
      k=sequence[k];
 
      seq.push_back(ints[k]);
  }



 
       return seq ;
}

```
