#ARTS第一
## Algorithm
####771. Jewels and Stones

You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

Example 1:771


Input: J = "aA", S = "aAAbbbb"
Output: 3
Example 2:

Input: J = "z", S = "ZZ"
Output: 0
Note:

S and J will consist of letters and have length at most 50.
The characters in J are distinct.


我的解答：
```java
class Solution {
    public int numJewelsInStones(String J, String S) {
       char [] jArr = J.toCharArray();
        char[] sArr = S.toCharArray();
        int jewelsNum=0;
        for(char j: jArr)
        {
            for(char s: sArr)
            {
                if(s==j)
                {
                    jewelsNum++;
                }
            }
        }
        return jewelsNum;
    }
}
```

Runtime: 1 ms, faster than 97.67% of Java online submissions for Jewels and Stones.
Memory Usage: 34.2 MB, less than 100.00% of Java online submissions for Jewels and Stones.

LeetCode显示这个解答不是最快速的，考虑到在比较的过程中，已经确认的S中的是Jewelrys的在下个循环就不应该再次进行比较了。这里应该可以优化的。
尽管是暴力的循环遍历，但是由于使用了char数组进行操作，所以速度还是相当的给力，想象一下如果使用String的split转成数组，效率不会很高。

###Review

这段时间正好读了一篇推荐系统的论文[《RippleNet: Propagating User Preferences on the Knowledge Graph for Recommender Systems》](https://arxiv.org/abs/1803.03467v1)
出自微软亚洲研究院，介绍了将知识图谱作为一种边信息加入推荐系统中，丰富了用户偏好信息，从而使得学习的结果在给定的数据集上更佳。知识图谱的研究不少，但是将其应用到推荐系统的
尝试目前看还不多。我挺看好这个方向，知识图谱里面茫茫多的信息都是宝藏，要找到能利用起来的方法，还有很多文章可以做。


###Tips
微软的Visio画起UML图非常的方便和美观，可是商用版本要付费。个人使用的话，推荐一下PlantUML这个软件，和Visio不同，不是拖拖拽拽点点鼠标的编辑方式，而是像写代码一样，敲一些字母和符号，就能生成UML图啦！
它有软件，有网页版，Chrom插件和IDEA插件和各种其他插件

这里用下[在线网页](http://www.plantuml.com/plantuml)的例子：
```
@startuml
Bob -> Alice : hello
@enduml
```
显示图像

![](http://www.plantuml.com/plantuml/png/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000)



###Share
由于最近在看机器学习，推荐一个介绍嵌入embedding概念的文章吧，[Embedding从入门到专家必读的十篇论文](https://zhuanlan.zhihu.com/p/58805184)，
在刚开始看论文的时候，被这个词弄的一头雾水。只知道嵌入是个单射，是个数学上的概念，怎么能代表推荐系统中的用户和物品呢？看了你就能明白。