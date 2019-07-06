## ARTS第五
### Algorithm
961. N-Repeated Element in Size 2N Array
Easy

206

139

Favorite

Share
In a array A of size 2N, there are N+1 unique elements, and exactly one of these elements is repeated N times.

Return the element repeated N times.

 

Example 1:

Input: [1,2,3,3]
Output: 3
Example 2:

Input: [2,1,2,5,3,2]
Output: 2
Example 3:

Input: [5,1,5,2,5,3,5,4]
Output: 5
 

Note:

4 <= A.length <= 10000
0 <= A[i] < 10000
A.length is even

```java
class Solution {
    public int repeatedNTimes(int[] A) {
        Map<Integer,Integer> countMap = new HashMap<>();
        for(int i = 0; i< A.length; i++)
        {
            if(countMap.get(A[i])!=null)
            {
                int iCount = countMap.get(A[i]);
                countMap.put(A[i],iCount+1);
            }
            else
            {
                countMap.put(A[i],1);
            }
        }
        
        int num = 0;
        for (Map.Entry<Integer, Integer> entry : countMap.entrySet())
        {
            if (entry.getValue() == A.length /2)
            {
                num= entry.getKey();
            }
        }
    return num;
    }
}
```
这个算法是在HashTable分类下找的，所以我的思路开始就钻进了使用HashTable。
当然实现的没有问题，AC的也越来越快了，提交后却发现时间不是最优的。
更优的解法关注了题目中的N+1个数，需要找的是重复N的数，也就是说其他的数都出现
且只出现一次。这个解使用了数据的分布的信息，进而优化。这里我把最优解也贴上吧，
```Approach 2: Compare
Intuition and Algorithm

If we ever find a repeated element, it must be the answer. Let's call this answer the major element.

Consider all subarrays of length 4. There must be a major element in at least one such subarray.

This is because either:

There is a major element in a length 2 subarray, or;
Every length 2 subarray has exactly 1 major element, which means that a length 4 subarray that begins at a major element will have 2 major elements.
Thus, we only have to compare elements with their neighbors that are distance 1, 2, or 3 away.
```


###  Review
[臃肿的web的健身操]https://blog.codinghorror.com/an-exercise-program-for-the-fat-web/
Jeff Atwood的博文，文章首先批评了现在的浏览器越来越臃肿，一个是展示效果花样多，第二个也是罪魁祸首就是广告。
其次说到对付广告的办法，以Chrome浏览器为例，安装拦截广告插件可以拦截不少麻烦的广告。但是Google开始限制广告插件，所以这个方法不再有效。
最后，这里就引出了作者最近发现的一个拦截广告的好办法，使用[PI-hole]https://pi-hole.net/,在黑莓派上安装该软件，停掉路由器上的DHCP server,把黑莓派当做DHCP server，就可以拦截广告了。

### Tip
迭代工作的时候，将大的工作内容分解成小块，有一个利器，就是思维导图，向同事学习了这个工具，能够很方便省时间的帮助分解大块工作为小块工作。

而个人项目管理使用Trello作为看板，可视化自己的工作。

笔记软件使用OneNote

这三个软件结合，最终目的是为了提高质量，提升效率。剩下来的时间用来学习新的技术。
### Share
接触了Jeff Atwood的博客看到了树莓派，对树莓派产生了兴趣，分享一下树莓派的网站http://shumeipai.nxez.com/