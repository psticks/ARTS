# ARTS第二
## Algorithm
####717. 1-bit and 2-bit Characters

We have two special characters. The first character can be represented by one bit 0. The second character can be represented by two bits (10 or 11).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.

Example 1:
Input: 
bits = [1, 0, 0]
Output: True
Explanation: 
The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.
Example 2:
Input: 
bits = [1, 1, 1, 0]
Output: False
Explanation: 
The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.
Note:

1 <= len(bits) <= 1000.
bits[i] is always 0 or 1.


我的解答：
```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
//從高位向低位，第一位是1 ，那就要用兩位，第一位是零，就用一位
        int len = bits.length;
        int [] a = new int[len -1];

        if (len - 2 >= 0) System.arraycopy(bits, 0, a, 0, len - 2);
        if (bits[len -1] ==1)
            return false;
        //必须末尾是0
        if (len == 1) {
            return bits[0] == 0;
        }
        else if (len == 2) {
            if (bits[0] == 1) {
                return false;
            }
            else return isOneBitCharacter(a);
        }
//1X1Y1Y1Y1Z...0 这样形式
        else  {
            if (bits[0] == 1) {
                int[] b = new int[len - 2];
                System.arraycopy(bits, 0, b, 0, len - 3);
                return isOneBitCharacter(b);
            } else {
                return isOneBitCharacter(a);
            }
        }
    }
}
```
开始我是用递归做的，提交之后很久都没出结果，可见递归的性能很差的。不能胜任在可接受的范围内。

遇到挫折之后自然是递归转循环啊
``` java 
public static boolean isOneBitCharacter(int[] bits) {
           //從高位向低位，第一位是1 ，那就要用兩位，第一位是零，就用一位
           int len = bits.length -1;
           int posCount = 0;
           if (bits[len] == 0) {
               if (len == 0)
                   return true;
               if (len ==1)
               {
                   if (bits[0] == 0)
                   return true;
               }
               while (posCount <= len - 2) {
                   if (bits[posCount] == 0) {
                       posCount = posCount + 1;
                   } else if (bits[posCount] == 1) {
                       posCount = posCount + 2;
                   }
               }
               if (posCount == len - 1) {
                   if (bits[posCount] == 1)
                   return false;
                   else return true;
               }
               if (posCount == len)
               {
                   return true;
               }
           }
           return false;
       }
   ```
这个结果好很多，但是写的不够简洁。也反复修改才成功，没想到简单的题目这么低的AC率。

### Review

关于在线教育是否有用的讨论
```https://medium.com/@derektng/what-if-online-education-simply-doesnt-work-there-s-evidence-it-may-not-302dc04c090```
美国大学生1/3都参与了在线课程,在线课程的本意是公平，更便宜的让人们平等的受教育，然而在线教育是不是有效呢，这个文章给出的数据不是肯定的回答，在线教育多是商业目的学校采用，课程收费也不少，课程的效果也没有比得上传统教学方式，找工作，在线的学历也不太受认可。

### Tips

这周总结的不是一个技术，而是工作方法的技巧。来源于工作的电脑老旧，运行很慢，又暂时不能更换，导致效率特别低，很多人加入了很多临时的任务，导致任务切换花了很多时间。那解決办法就是

1 推掉任务，有人找你做事，并不一定你才能解决，拒绝了他会找别人的

2 把在一个软件上做的工作放到一起，想好了再动手。
目前在老旧电脑上只能这么操作了

### Share
分享一个把技术变成产品的思路，打造自己的品牌，程序员是可以靠技术，不需要被雇佣而生活的
https://www.yuanrenxue.com/earn-money/earn-money-blogging.html
