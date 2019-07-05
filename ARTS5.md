## ARTS第四
### Algorithm


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
}```

### Review
[臃肿的web的健身操]https://blog.codinghorror.com/an-exercise-program-for-the-fat-web/
Jeff Atwood的博文，文章首先批评了现在的浏览器越来越臃肿，一个是展示效果花样多，第二个也是罪魁祸首就是广告。
其次说到对付广告的办法，以Chrome浏览器为例，安装拦截广告插件可以拦截不少麻烦的广告。但是Google开始限制广告插件，所以这个方法不再有效。
最后，这里就引出了作者最近发现的一个拦截广告的好办法，使用[PI-hole]https://pi-hole.net/,在黑莓派上安装该软件，停掉路由器上的DHCP server,把黑莓派当做DHCP server，就可以拦截广告了。
