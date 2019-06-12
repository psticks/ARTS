##ARTS第三
### Algorithm
283. Move Zeroes
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Example:

Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
Note:

You must do this in-place without making a copy of the array.
Minimize the total number of operations.
题目叫做移动零，其实要移动的不止零，而是包括零和非零，那考虑非零元素挪到一起去，然后后面跟一串零就行了，按这个想法，写了好几次才AC,而且我严重依赖IDE
```java
class Solution {
    public void moveZeroes(int[] nums)
    {
        int countZero = 0;
        for (int i = 0; i < nums.length; i++)
        {
            if (nums[i] != 0)
            {
               nums[i - countZero] = nums[i];
            }
            else
            {
                countZero += 1;
            }
        }

        for (int i = nums.length-countZero; i < nums.length; i++)
        {
            nums[i] = 0;
        }
    }
}
```

看了解答，我这个思路是对的，但是不是时间复杂度最优解，因为零元素的填充也可以在一次循环中搞定，不需要再做countZero次的填充。

### Review
1.[]https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed
这篇文章看标题还以为总结了对付面试题的通用步骤，看下来才发现是针对每个题型的具体步骤总结，嗯，某种意义上的干货吧，不过这个不是真正的知识，更遑论智慧了。
这个就和高考数据物理不断的总结题型，为了应试教育考试的总结。

### Tip
有时候代码能验证证书链，而浏览器认为不安全，这是JDK证书列表和浏览器证书列表不同个缘故，学习了一个判断根证书是否在JDK受信任的根证书列表里的方法：

1.打开证书————详细信息————所有————指纹，把下面的字符串复制出来；

2.找到你的JDK的keytool工具，`keytool -list -keystore #JAVA_HOME/jre/lib/security/cacerts -storepass changeit`
windows下改下JDK

3.把2中证书列表复制出来和1对比。注意有大小写和分隔符的不同。
