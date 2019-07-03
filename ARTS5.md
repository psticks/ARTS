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
