import edu.princeton.cs.algs4.In;
import edu.princeton.cs.algs4.MinPQ;
import edu.princeton.cs.algs4.StdOut;

import java.util.Comparator;

public class Solver {
    private MinPQ<Board> boardMinPQ;

    // find a solution to the initial board (using the A* algorithm)
    public Solver(Board initial) {
        if (initial == null) throw new IllegalArgumentException();
        boardMinPQ = new MinPQ<>(initial.dimension(), new MahattanComparator());
        boardMinPQ.insert(initial);
        Board delMin = boardMinPQ.delMin();
        while (!delMin.isGoal()) {
            for (Board neighbor : initial.neighbors()) {
                boardMinPQ.insert(neighbor);
            }
        }
        

    }

    private class MahattanComparator implements Comparator<Board> {
        public int compare(Board o1, Board o2) {
            return o1.manhattan() - o2.manhattan();
        }
    }

    // is the initial board solvable? (see below)
    public boolean isSolvable() {
        return false;
    }

    // min number of moves to solve initial board
    public int moves() {
        return 0;
    }

    // sequence of boards in a shortest solution
    public Iterable<Board> solution() {
        return null;
    }

    // test client (see below)
    public static void main(String[] args) {
        // create initial board from file
        In in = new In(args[0]);
        int n = in.readInt();
        int[][] tiles = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                tiles[i][j] = in.readInt();
        Board initial = new Board(tiles);

        // solve the puzzle
        Solver solver = new Solver(initial);

        // print solution to standard output
        if (!solver.isSolvable())
            StdOut.println("No solution possible");
        else {
            StdOut.println("Minimum number of moves = " + solver.moves());
            for (Board board : solver.solution())
                StdOut.println(board);
        }
    }

}

import edu.princeton.cs.algs4.Stack;

import java.util.Arrays;

public class Board {
    private int[][] tiles;
    private int[][] goal;
    private int dim;

    // create a board from an n-by-n array of tiles,
    // where tiles[row][col] = tile at (row, col)
    public Board(int[][] tiles) {
        if (tiles == null)
            throw new IllegalArgumentException();

        this.tiles = tiles.clone();
        this.dim = tiles.length;
        // init goal
        goal = new int[tiles.length][tiles[0].length];
        for (int i = 0; i < goal.length; i++) {
            for (int j = 0; j < goal.length; j++) {
                goal[i][j] = i * dim + j + 1;
            }
        }
        goal[tiles.length - 1][tiles[0].length - 1] = 0;
    }

    // string representation of this board
    public String toString() {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append(dim);
        stringBuilder.append("\n");

        for (int[] tile : tiles) {
            for (int i : tile) {
                stringBuilder.append(i);
                stringBuilder.append(" ");
            }
            stringBuilder.append("\n");
        }
        return stringBuilder.toString();
    }

    // board dimension n
    public int dimension() {
        return dim;
    }

    // number of tiles out of place
    public int hamming() {
        int num = 0;
        for (int i = 0; i < goal.length; i++) {
            for (int j = 0; j < goal.length; j++) {
                if (goal[i][j] != 0) {
                    if (goal[i][j] != tiles[i][j])
                        num++;
                }
            }
        }
        return num;
    }

    // sum of Manhattan distances between tiles and goal
    public int manhattan() {
        int manhattanDistance = 0;
        for (int i = 0; i < goal.length; i++) {
            for (int j = 0; j < goal.length; j++) {
                manhattanDistance += manhattan(tiles[i][j], i, j);
            }
        }
        return manhattanDistance;
    }

    // 给定一个数k，求到它的目标值的曼哈顿距离，根据k就知道他在i,j的哪个位置，k/n=p余数q,则第p-1行,q-1列（从0开始）
    private int manhattan(int k, int row, int col) {
        // 0不計算
        if (k == 0) {
            return 0;
        }

        int goalCol = 0;
        int goalRow = 0;
        if (k % dim == 0) {
            goalCol = dim - 1;
            goalRow = k / dim - 1;
        }
        else {
            goalCol = k % dim - 1;
            goalRow = (k - k % dim) / dim;
        }

        return Math.abs(goalCol - col) + Math.abs(goalRow - row);
    }

    // is this board the goal board?
    public boolean isGoal() {
        // TODO
        return Arrays.deepEquals(this.tiles, goal);
    }

    // does this board equal y?
    public boolean equals(Object y) {
        if (y == null)
            return false;
        if (y instanceof Board) {
            if (this.dim != ((Board) y).dim) {
                return false;
            }
            return Arrays.deepEquals(this.tiles, ((Board) y).tiles);
        }
        return false;
    }

    // all neighboring boards
    // 找到0 的位置，0 的四周都可以和0交换，成为一个新的naighbor
    public Iterable<Board> neighbors() {
        Stack<Board> boardStack = new Stack<>();
        for (int i = 0; i < tiles.length; i++) {
            for (int j = 0; j < tiles.length; j++) {
                if (tiles[i][j] == 0) {
                    if (i > 0)
                        boardStack.push(swap(i, j, i - 1, j));
                    if (j > 0)
                        boardStack.push(swap(i, j, i, j - 1));
                    if (i < dim - 1)
                        boardStack.push(swap(i, j, i + 1, j));
                    if (j < dim - 1)
                        boardStack.push(swap(i, j, i, j + 1));

                }
            }

        }
        return boardStack;
    }

    private Board swap(int oldCol, int oldRow, int newCol, int newRow) {
        int[][] newTiles = new int[dim][dim];
        for (int i = 0; i < newTiles.length; i++) {
            for (int j = 0; j < newTiles.length; j++) {
                newTiles[i][j] = tiles[i][j];
            }
        }

        int oldValue = newTiles[oldCol][oldRow];
        newTiles[oldCol][oldRow] = newTiles[newCol][newRow];
        newTiles[newCol][newRow] = oldValue;
        return new Board(newTiles);
    }

    // a board that is obtained by exchanging any pair of tiles
    public Board twin() {
        int[][] twin = tiles.clone();
        for (int i = 0; i < dim; i++) {
            for (int j = 0; j < dim; j++) {
                if (twin[i][j] != 0 && twin[i][j + 1] != 0) {
                    return swap(i, j, i, j + 1);
                }
            }
        }
        return new Board(twin);
    }

    // unit testing (not graded)
    public static void main(String[] args) {
        // int[][] tiles = new int[][] {
        //         { 1, 2, 3 },
        //         { 4, 5, 6 },
        //         { 7, 8, 0 }
        // };
        int[][] tiles = new int[][] { { 0, 1, 3 }, { 4, 2, 5 }, { 7, 8, 6 } };
        Board board = new Board(tiles);
        System.out.println("dimesion is: " + board.dimension());
        System.out.println(board);
        System.out.println("hamming distance is: " + board.hamming());
        System.out.println("manhatan distance is: " + board.manhattan());
        Iterable<Board> neighbors = board.neighbors();
        for (Board neighbor : neighbors) {
            System.out.println(neighbor);
        }
        System.out.println(board.twin());
        System.out.println(board.isGoal());

    }
}

