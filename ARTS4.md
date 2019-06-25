## ARTS第四
### Algorithm
1089. Duplicate Zeros
Given a fixed length array arr of integers, duplicate each occurrence of zero, shifting the remaining elements to the right.

Note that elements beyond the length of the original array are not written.

Do the above modifications to the input array in place, do not return anything from your function.

 

Example 1:

Input: [1,0,2,3,0,4,5,0]
Output: null
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
Example 2:

Input: [1,2,3]
Output: null
Explanation: After calling your function, the input array is modified to: [1,2,3]
题目比较简单，遇到零，就把其他元素往数组末尾移动，最末尾的就“推出”了边界，被丢掉了。这个是不依靠IDE,直接在网页编写的一次提交就AC的，特意记录下，但是也没有一次成功
也标志着数组的esay分类，可以大概不用去看了，后面要开始别的分类的算法题演练。
```java
class Solution {
    public void duplicateZeros(int[] arr) {
        for(int i=0;i<arr.length;){
            if(arr[i] == 0)
            {
                if((i+1) <arr.length)
                {
                    for(int j = arr.length-1; j>i;j--)
                    {
                        arr[j] = arr[j-1];
                    }
                 
                    i=i+2;                                        

                }
                else
                {
                    break;
                }
                
            }
            else{
                 i= i+1;
            }
        }
    }
}
```
这个算法不是时间最优的。元素反复被移动，应该可以只记录一个引用移动他的位置。这次是为了AC太快实现了。

### Review
[结对编程心理学]https://medium.com/free-code-camp/the-psychology-of-pair-programming-86cb31f9abca

在结对编程的实践中，有一些总结，对我这个没怎么用过结对的人来说，是很新鲜的事。
1、两个人要依次交出键盘的控制权
2、听取两人以外，其他人的建议
3、把讨论的东西写下来，用白板或者记事本，保存上下文
4、有问题不要自己扛，记下来给另一个人

### Tip
 

### Share
