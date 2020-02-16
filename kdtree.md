import edu.princeton.cs.algs4.Point2D;
import edu.princeton.cs.algs4.RectHV;
import edu.princeton.cs.algs4.StdDraw;

import java.awt.Color;
import java.util.HashSet;
import java.util.Set;

public class KdTree {
    private Node root;

    private class Node {
        private Point2D point;
        private Node left;
        private Node right;
        private int level;
        private RectHV rect;
    }

    // construct an empty set of points
    public KdTree() {
        root = null;
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
        else return size(node.left) + size(node.right) + 1;
    }

    // add the point to the set (if it is not already in the set)
    public void insert(Point2D p) {
        root = insert(root, p, 0, new RectHV(0, 0, 1, 1));
    }

    private Node insert(Node x, Point2D p, int level, RectHV rectHV) {
        if (x == null) {
            x = new Node();
            x.point = p;
            x.level = level;
            x.rect = rectHV;
            return x;
        }
        else {
            int cmp;
            // 偶数层
            if (x.level % 2 == 0) {
                cmp = Double.compare(p.x(), x.point.x());
                if (cmp < 0) x.left = insert(x.left, p, level + 1,
                                             new RectHV(x.rect.xmin(), x.rect.ymin(), x.point.x(),
                                                        x.rect.ymax()));
                if (cmp > 0) x.right = insert(x.right, p, level + 1,
                                              new RectHV(x.point.x(), x.rect.ymin(), x.rect.xmax(),
                                                         x.rect.ymax()));
            }
            else {
                cmp = Double.compare(p.y(), x.point.y());
                if (cmp < 0) x.left = insert(x.left, p, level + 1,
                                             new RectHV(x.rect.xmin(), x.rect.ymin(), x.rect.xmax(),
                                                        x.point.y()));
                if (cmp > 0) x.right = insert(x.right, p, level + 1,
                                              new RectHV(x.rect.xmin(), x.point.y(), x.rect.xmax(),
                                                         x.rect.ymax()));
            }
        }
        // 已經存在
        return x;
    }

    // does the set contain point p?
    public boolean contains(Point2D p) {
        if (p == null)
            throw new IllegalArgumentException();
        return contains(root, p);
    }

    private boolean contains(Node x, Point2D p) {
        if (x == null) return false;
        int cmp;
        // 偶数层
        if (x.level % 2 == 0) {
            cmp = Double.compare(p.x(), x.point.x());
        }
        else {
            cmp = Double.compare(p.y(), x.point.y());
        }
        if (cmp < 0) return contains(x.left, p);
        if (cmp > 0) return contains(x.right, p);
        return true;
    }

    // draw all points to standard draw
    public void draw() {
        draw(root);
    }

    private void draw(Node x) {
        if (x == null) return;
        x.point.draw();
        if (x.level % 2 == 0) {
            StdDraw.setPenColor(Color.RED);
            StdDraw.line(x.point.x(), x.rect.ymax(), x.point.x(), x.rect.ymin());
        }
        else {
            StdDraw.setPenColor(Color.BLUE);
            StdDraw.line(x.rect.xmax(), x.point.y(), x.rect.xmin(), x.point.y());
        }
        draw(x.left);
        draw(x.right);
    }

    // all points that are inside the rectangle (or on the boundary)
    public Iterable<Point2D> range(RectHV rect) {
        if (rect == null) throw new IllegalArgumentException();
        Set<Point2D> pointSet = range(root, rect);
        return pointSet;
    }

    private Set<Point2D> range(Node x, RectHV rect) {
        Set<Point2D> point2DS = new HashSet<>();
        if (rect.contains(x.point))
            point2DS.add(x.point);
        if (x != null && rect.intersects(x.rect)) {
            if (x.left != null && rect.intersects(x.left.rect))
                point2DS.addAll(range(x.left, rect));
            if (x.right != null && rect.intersects(x.right.rect))
                point2DS.addAll(range(x.right, rect));
        }
        return point2DS;
    }

    // a nearest neighbor in the set to point p; null if the set is empty
    public Point2D nearest(Point2D p) {
        if (p == null)
            throw new IllegalArgumentException();
        if (root == null) return null;
        return nearest(root, p);
    }

    private Point2D nearest(Node x, Point2D queryP) {
        Point2D nearest = x.point;
        double shortestDistance = queryP.distanceTo(x.point);
        if (x.left != null) {
            int cmp = Double.compare(shortestDistance, x.left.rect.distanceTo(queryP));
            if (cmp > 0) {
                nearest = nearest(x.left, queryP);
                shortestDistance = queryP.distanceTo(nearest);
            }
        }
        if (x.right != null) {
            int cmpR = Double.compare(shortestDistance, x.right.rect.distanceTo(queryP));
            if (cmpR > 0 && x.right != null) {
                nearest = nearest(x.right, queryP);
                shortestDistance = queryP.distanceTo(nearest);
            }
        }
        return nearest;
    }


    public static void main(String[] args) {

    }
}
import edu.princeton.cs.algs4.Point2D;
import edu.princeton.cs.algs4.RectHV;
import edu.princeton.cs.algs4.Stack;

import java.util.Comparator;
import java.util.TreeSet;

public class PointSET {
    private TreeSet<Point2D> points;

    // construct an empty set of points
    public PointSET() {
        points = new TreeSet<>();
    }

    // is the set empty?
    public boolean isEmpty() {
        return points.isEmpty();
    }

    // number of points in the set
    public int size() {
        return points.size();
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
        TreeSet<Point2D> sortedSet = new TreeSet<>(new Comparator<Point2D>() {
            public int compare(Point2D o1, Point2D o2) {
                return Double.compare(o1.distanceTo(p), o2.distanceTo(p));
            }
        });
        sortedSet.addAll(points);
        return sortedSet.first();
    }


    public static void main(String[] args) {

    }
}
