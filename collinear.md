/* *****************************************************************************
 *  Name:
 *  Date:
 *  Description:
 **************************************************************************** */

import edu.princeton.cs.algs4.In;
import edu.princeton.cs.algs4.StdDraw;
import edu.princeton.cs.algs4.StdOut;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class FastCollinearPoints {
    private int num;
    private LineSegment[] lineSegments;

    // finds all line segments containing 4 or more points
    public FastCollinearPoints(Point[] opoints) {
        if (opoints == null) {
            throw new IllegalArgumentException();
        }
        int numOfPoints = opoints.length;
        this.lineSegments = new LineSegment[opoints.length * opoints.length / 2];
        // 先把点按从左往右，从下往上的顺序拍好，这样后面算出重复的线段就直接丢弃
        // TODO 不要对入参修改，要再复制一份
        Point[] points = opoints.clone();
        Arrays.sort(points);
        Arrays.sort(points);
        // TODO 排序之后 计算是否有相等元素会简单很多
        for (int i = 0; i < points.length; i++) {
            if (points[i].compareTo(points[i + 1]) >= 0)
                throw new IllegalArgumentException();
        }

        // 选定一个点point，计算其他点相对它的斜率,按斜率从小到大排序
        for (int i = 0; i < numOfPoints; i++) {
            Point point = points[i];

            Point[] slopeOrderPoints = new Point[numOfPoints - 1];
            int newIdx = 0;
            for (int j = 0; j < numOfPoints; j++) {
                if (j != i) {
                    slopeOrderPoints[newIdx] = points[j];
                    newIdx++;
                }
            }

            Arrays.sort(slopeOrderPoints, point.slopeOrder());
            //排好之后，检查相鄰是否有相等的斜率
            int pointsNumInSeg = 0;
            Point start = null, end = null;
            boolean first = true;
            List<Point> group = new ArrayList<>();
            for (int j = 0; j < slopeOrderPoints.length - 1; j++) {
                if (point.slopeTo(slopeOrderPoints[j]) == point.slopeTo(slopeOrderPoints[j + 1])) {
                    group.add(points[j]);
                    pointsNumInSeg++;
                    end = slopeOrderPoints[j + 1];
                    if (first) {
                        start = slopeOrderPoints[j];
                        first = false;
                    }
                }
                else {
                    // 斜率相等的点至少有三个，加上point是四个，此时开始判断起点
                    if (pointsNumInSeg > 1) {
                        // point 是线段起点
                        if (point.compareTo(start) < 0) {
                            LineSegment lineSegment = new LineSegment(point, end);
                            start = null;
                            end = null;

                            lineSegments[num++] = lineSegment;
                        }
                    }
                    first = true;
                    // 把計數歸零
                    pointsNumInSeg = 0;
                    continue;
                }

                if (j == slopeOrderPoints.length - 2) {
                    if (pointsNumInSeg > 1) {
                        // point 是线段起点
                        if (point.compareTo(start) < 0) {
                            LineSegment lineSegment = new LineSegment(point, end);
                            start = null;
                            end = null;

                            // 把計數歸零
                            pointsNumInSeg = 0;
                            lineSegments[num++] = lineSegment;
                        }
                    }
                    first = true;
                }
            }
        }
    }

    // the number of line segments
    public int numberOfSegments() {
        return num;
    }

    public LineSegment[] segments() {
        LineSegment[] result = new LineSegment[num];
        for (int i = 0; i < num; i++) {
            LineSegment lineSegment = lineSegments[i];
            if (lineSegment != null)
                result[i] = lineSegment;
        }

        return result;
    }

    public static void main(String[] args) {

        // read the n points from a file
        In in = new In(args[0]);
        int n = in.readInt();
        Point[] points = new Point[n];
        for (int i = 0; i < n; i++) {
            int x = in.readInt();
            int y = in.readInt();
            points[i] = new Point(x, y);
        }

        // draw the points
        StdDraw.enableDoubleBuffering();
        StdDraw.setXscale(0, 32768);
        StdDraw.setYscale(0, 32768);
        for (Point p : points) {
            p.draw();
        }
        StdDraw.show();

        // print and draw the line segments
        FastCollinearPoints collinear = new FastCollinearPoints(points);
        for (LineSegment segment : collinear.segments()) {
            StdOut.println(segment);
            segment.draw();
        }
        StdDraw.show();
    }
}
/* *****************************************************************************
 *  Name:
 *  Date:
 *  Description:
 **************************************************************************** */

import java.util.Arrays;

public class BruteCollinearPoints {
    private int num;
    private LineSegment[] lineSegments;

    // finds all line segments containing 4 points
    public BruteCollinearPoints(Point[] points) {
        if (points == null) {
            throw new IllegalArgumentException();
        }

        this.num = 0;
        this.lineSegments = new LineSegment[points.length];
        for (Point point : points) {
            assertNotNull(point);
        }

        // 按坐标排序
        // TODO 不要对入参修改，要再复制一份
        Point[] clone = points.clone();
        Arrays.sort(clone);
        // TODO 排序之后 计算是否有相等元素会简单很多
        for (int i = 0; i < clone.length; i++) {
            if (clone[i].compareTo(clone[i + 1]) >= 0)
                throw new IllegalArgumentException();
        }

        for (int i = 0; i < clone.length; i++) {
            Point a = clone[i];
            for (int j = i + 1; j < clone.length && j != i; j++) {
                Point b = clone[j];
                double slopeab = a.slopeTo(b);
                for (int k = j + 1; k < clone.length && k != j && k != i;
                     k++) {
                    Point c = clone[k];
                    double slopebc = b.slopeTo(c);
                    for (int m = k + 1; m < clone.length && m != i && m != j && m != k; m++) {
                        Point d = clone[m];
                        if (Double.compare(slopeab, slopebc) == 0
                                && Double.compare(slopeab, a.slopeTo(d)) == 0) {
                            LineSegment lineSegment = new LineSegment(a, d);
                            lineSegments[num++] = lineSegment;
                        }
                    }
                }
            }
        }
    }

    private void assertNotNull(Point a) {
        if (a == null)
            throw new IllegalArgumentException();
    }

    // the number of line segments
    public int numberOfSegments() {
        return num;
    }

    public LineSegment[] segments() {
        LineSegment[] result = new LineSegment[num];
        for (int i = 0; i < lineSegments.length; i++) {
            LineSegment lineSegment = lineSegments[i];
            if (lineSegment != null)
                result[i] = lineSegment;
        }

        return result;
    }

    public static void main(String[] args) {
        Point[] points = new Point[9];
        points[0] = new Point(0, 0);
        points[1] = new Point(0, 1);
        points[2] = new Point(0, 2);
        points[3] = new Point(0, 3);
        points[4] = new Point(1, 4);
        points[5] = new Point(1, 1);
        points[6] = new Point(2, 2);
        points[7] = new Point(3, 3);
        points[8] = new Point(3, 3);

        BruteCollinearPoints bruteCollinearPoints = new BruteCollinearPoints(points);
        System.out.println(bruteCollinearPoints.numberOfSegments());
        LineSegment[] segments = bruteCollinearPoints.segments();
        for (LineSegment segment : segments) {
            if (segment != null)
                segment.draw();
        }
    }
}
