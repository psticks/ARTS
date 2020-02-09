/* *****************************************************************************
 *  Name:
 *  Date:
 *  Description:
 **************************************************************************** */

import edu.princeton.cs.algs4.Point2D;
import edu.princeton.cs.algs4.RectHV;


public class KdTree {
    private Node root;

    private class Node {
        public int num;
        private Point2D point;
        private Node left;
        private Node right;

    }

    // construct an empty set of points
    public KdTree() {
    }

    // is the set empty?
    public boolean isEmpty() {
        return size() == 0;
    }

    // number of points in the set
    public int size() {
        return size(root);
    }

    private int size(Node node) {
        if (node == null)
            return 0;
        else return node.num;
    }

    // add the point to the set (if it is not already in the set)
    public void insert(Point2D p) {
    }

    // does the set contain point p?
    public boolean contains(Point2D p) {
        if (p == null)
            throw new IllegalArgumentException();
        int cmp = root.point.compareTo(p);
        if (cmp < 0) return contains()
    }

    // draw all points to standard draw
    public void draw() {
    }

    // all points that are inside the rectangle (or on the boundary)
    // 怎么计算一个矩形中的点，那就是把矩形的四个点的坐标范围圈出来，把每个点横纵坐标去比较
    public Iterable<Point2D> range(RectHV rect) {
    }

    // a nearest neighbor in the set to point p; null if the set is empty
    public Point2D nearest(Point2D p) {
    }

    public static void main(String[] args) {

    }
}
/* *****************************************************************************
 *  Name:
 *  Date:
 *  Description:
 **************************************************************************** */

import edu.princeton.cs.algs4.Point2D;
import edu.princeton.cs.algs4.RectHV;
import edu.princeton.cs.algs4.Stack;

import java.util.TreeSet;

public class PointSET {
    private TreeSet<Point2D> points;
    private int num;

    // construct an empty set of points
    public PointSET() {
        points = new TreeSet<>();
    }

    // is the set empty?
    public boolean isEmpty() {
        return num == 0;
    }

    // number of points in the set
    public int size() {
        return num;
    }

    // add the point to the set (if it is not already in the set)
    public void insert(Point2D p) {
        if (!contains(p)) {
            points.add(p);
        }
    }

    // does the set contain point p?
    public boolean contains(Point2D p) {
        return points.contains(p);
    }

    // draw all points to standard draw
    public void draw() {
        if (points != null)
            for (Point2D point2D : points) {
                point2D.draw();
            }
    }

    // all points that are inside the rectangle (or on the boundary)
    // 怎么计算一个矩形中的点，那就是把矩形的四个点的坐标范围圈出来，把每个点横纵坐标去比较
    public Iterable<Point2D> range(RectHV rect) {
        Stack<Point2D> point2DStack = new Stack<>();
        for (Point2D point : points) {
            if (rect.contains(point))
                point2DStack.push(point);
        }
        return point2DStack;
    }

    // a nearest neighbor in the set to point p; null if the set is empty
    public Point2D nearest(Point2D p) {
        if (points == null || isEmpty()) return null;
        TreeSet<Point2D> sortedSet = new TreeSet<>(Point2D.R_ORDER);
        return sortedSet.first();
    }


    public static void main(String[] args) {

    }
}
