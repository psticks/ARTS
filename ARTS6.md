## Algorithm
Matrix 计算矩阵从左上角到右下角的元素，加起来最大值。元素相加的方向只能是向下或者向右。


看到这个问题，首先想到动态规划的方法，找到下一行和上一行的关系：
用《算法图解》中的方法，把网格画出来：如果已经知道了第i行的每个元素，从左上角到这个元素的和的最大值sum_ij,那么对于第i+1行的元素，
到a_i+1,j+1的最大值就是a_i+1,j+sumij+1和a_i,j+1+sumij+1中更大的一个。


实现的时候，为二维数组的行和列坑了一下，int[row][col] 代表有row个数组的数据，每个数组长col
```java
class Matrix {
    // We have imported the necessary tool classes.
    // If you need to import additional packages or classes, please import here.

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String s = scanner.nextLine();
        String[] size = s.split(" ");
        int row = Integer.parseInt(size[0]);
        int col = Integer.parseInt(size[1]);
        int[][] matrix = new int[row][col];
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                matrix[i][j] = scanner.nextInt();
            }
        }
        System.out.println(sum(matrix));
    }

    private static int sum(int[][] matrix) {
        int row = matrix.length;
        int col = matrix[0].length;
        int[][] sums = new int[matrix.length][matrix[0].length];
        sums[0][0] = matrix[0][0];
        for (int i = 1; i < row; i++) {
            sums[i][0] = matrix[i][0] + sums[i - 1][0];
        }
        for (int j = 1; j < col; j++) {
            sums[0][j] = matrix[0][j] + sums[0][j - 1];
        }
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < col; j++) {
                sums[i][j] = Math.max(sums[i - 1][j], sums[i][j - 1]) + matrix[i][j];
            }
        }
        return sums[row - 1][col - 1];
    }
}
```
最终实现没有拿到最快的运行时间，sum方法里面应该可以优化

## Review
对全栈的思考，文章分析了全栈的起，发展，和变化。提出现在的知识太深太多，一个人无法掌握api文档写作，持续集成，devops，测试开发等专门的知识，
提出的变化是需要一个全栈团队，而不是一个全栈的个人，招聘程序员的时候呢，也不是传统的要“前端”“后端”“全栈”，而是指定了一个全栈的团队的需要的技能。
这个团队还要定期的分享自己的技能给整个团队，以避免一个人的缺少形成技能的短板。团队的能力总是完整的。


我的思考：这个不是什么新的观点，甚至打个游戏5人团队都不会选一模一样的技能。在于把这个观点放在“全栈”这个概念上来说，显得新颖。
https://medium.com/better-programming/2020-001-full-stack-pronounced-dead-355d7f78e733

## Tips
主持会议的窍门：有問題立即記下來，不能決策的留給會後能決策的人，不耽誤會上的時間。需要跟蹤的記錄下責任人。这样自己，提问的人，其他的人都能得到好处。
