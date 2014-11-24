---
layout: post
title: 快速排序(ANSI C 和 Python实现)
descripthon: "排序算法因其应用的广泛一直以来是计算机科学家探索的古老领域，其中快速排序以其接近于n(logn)的时间复杂度和最多log(2n)的空间复杂度公认为
目前最快的排序算法."
modified: 2014-10-14
tags: [algorithms]
---

### 前言


&emsp;&emsp;排序算法因其应用的广泛一直以来是计算机科学家探索的古老领域，其中快速排序以其接近于n(logn)的时间复杂度和最多log(2n)的空间复杂度公认为
目前最快的排序算法，他是由计算机科学家图灵奖获得者
[hoare](http://en.wikipedia.org/wiki/Tony_Hoare "hoare wiki")发明的，今天我准备通过ANSI C和Python
分别来实现该算法。


### 思路
1. 选取锚点，就是无序序列的划分点，存入临时变量，序列空出一个坑，后续步骤就是填这个坑。
2. 使用两个指针分别从左，从右遍历序列，小于锚点放左边，大于锚点放右边。
3. 当两指针下标相同后，将空出来的坑用提取出来的临时变量填上。
4. 将当前锚点作为划分点分为两个子序列，递归执行以上步骤。


###代码实现

####   1. ANSI C

        {% highlight c %}
        #include<stdio.h>
        int* quicksort(int list[],int left,int right){
             if(left < right){ 
                 int i = left,j = right;
                 int key = list[i];//以第一个元素作为锚点
                 while(i < j){ //遍历序列根据参考锚点大小换值
                     while(i < j && list[j] >= key){
                         j--;
                     }
                     if(i < j){ 
                         list[i++] = list[j]; 
                     }
                     while(i < j && list[i] < key){
                         i++;
                     }
                     if(i < j){ 
                         list[j--] = list[i];
                     }
                 }
                 list[i] = key;
                 quicksort(list,left,i-1);
                 quicksort(list,i+1,right);
             }   
         
             return list;
        }
        //测试代码
         int main(){
             int i=0,list[] = {2321,232,123,1923,898,100,92};
             int* result;
             result = quicksort(list,0,6);
             for(i =0;i<7;i++)
                 printf("%d,",result[i]);
         }
         {% endhighlight %}

####   2. Python

        {% highlight python %}
        def quicksort(lists,left,right):
        """Pivot point to start here""" 
        if left < right:
            i = left
            j = right
            key = lists[i]
            while i<j:
                while i<j and lists[j]>=key:
                    j -=1
                if i<j:
                    lists[i] = lists[j]
                    i -= 1
                while i<j and lists[i]<key:
                    i += 1
                if i<j:
                    lists[j] = lists[i]
                    j -= 1
            lists[i] = key
            quicksort(lists,left,i - 1)
            quicksort(lists,i + 1,right)
        """test code"""
        if __name__ == "__main__":
            lists = [2321,232,123,1923,898,100,92]
            print lists
            quicksort(lists,0,len(lists)-1)
            print lists
        {% endhighlight %}

###总结
&emsp;&emsp;快速排序虽然速度快，但是算法不稳定
（[算法稳定性](http://stackoverflow.com/questions/1517793/stability-in-sorting-algorithms "stackoverflow")），另外选择锚点的时候也有很多优化的地方，比如，
而且不同的选择会对结果效率有一定的影响。另外，也可以看一下这些比较算法的[动态过程](http://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)。

