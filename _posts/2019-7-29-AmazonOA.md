---
layout:     post   				    # 使用的布局（不需要改）
title:      Amazon July OA - Min distance to remove the obstacle 				# 标题 
subtitle:   Breadth First Search #副标题
date:       2019-07-29				# 时间
author:     Ty 						# 作者
#header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: false 						# 是否归档
tags:								#标签
    - Algorithm
---
## 题目 Problem Description

You are in charge of preparing a recently purchased lot for Amazon’s building. The lot is covered with trenches and has a single obstacle that needs to be taken down before the foundation is prepared for the building. The demolition robot must remove the obstacle before the progress can be made on the building.
Write an algorithm to determine the minimum distance required for the demolition robot to remove the obstacle.

**Assumptions:**

1. The lot is flat, except the trenches and can be represented by a 2D grid.
2. The demolition robot must start at the top left corner of the lot, which is always flat, and
can move on block up, down, right, left
3. The demolition robot cannot enter trenches and cannot leave the lot.
4. The flat areas are indicated by 1, areas with trenches are indicated by 0, and the obstacle
is indicated by 9

**Input:**
The input of the function has 3 arguments:

1. numRows – number of rows
2. numColumns – number of columns
3. lot – 2d grid of integers

**Output:** <br/>
Return an integer that indicated the minimum distance traversed to remove the obstacle else return -1

**Constraints:**
1<= numRows, numColumns <= 1000

**Example:**
numRows = 3, <br>
numColumns = 3 <br>
lot = [<br>
&nbsp;&nbsp;&nbsp;&nbsp;[1, 0, 0], <br>
&nbsp;&nbsp;&nbsp;&nbsp;[1, 0, 0], <br>
&nbsp;&nbsp;&nbsp;&nbsp;[1, 9, 1] <br>
]

Output: 3

Explanation: Starting from the top-left corner, the demolition robot traversed the cells (0,0) -> (1,0)-> (2,0)->(2,1). The robot moves 3 times to remove the obstacle “9”

这是一道亚马逊7月份OA里出现的一道题，题目看起来很长，但实际上不难理解，就是说一个机器人要在一个2维迷宫（数组）里从**左上角**开始找东西，然后有些格子不能走（数组数字=0），能走的是数组值为1的格子。9为要找的东西，找到时返回从左上角到最后目标的最短路径，否则返回-1。机器人可以上下左右走。

The following solution was adapted from GeeksforGeeks https://www.geeksforgeeks.org/shortest-distance-two-cells-matrix-grid/

## Approach: Breadth First Search 广度优先搜索

BFS在对于找最短路径的题目中非常有用，简单来说先建立一个Queue，以及一个array来记录已经访问过的格子。如果当前的格子不是最后目标则将它上下左右所有没有访问过的格子放入queue中。在最后的算法中，我额外建立一个数据结构叫Node用来记录当前格子的横轴坐标及从起点出发到当前的距离，**每次放入新的node到quueu中时，相应的距离要加1。**

> **时间复杂度 time complexity:** O(n*m), n和m为数组的行和列的长度。

```java

import java.util.*;

class Solution {
    // Define a new data structure
    class Node {
        public int x;
        public int y;
        public int dist;

        Node(int x, int y, int dist) {
            this.x = x;
            this.y = y;
            this.dist = dist;
        }
    }
    public int shortestDistance(int[][] grid) {
        // set up visited array and queue.
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        Queue<Node> q = new LinkedList<>();
        q.add(new Node(0,0,0));
        visited[0][0]=true;
        while (!q.isEmpty()) {
            Node top = q.poll();
            visited[top.x][top.y] = true;

            if (grid[top.x][top.y]==9) {
                return top.dist-1;
            }
            // down
            if (top.x+1 < grid.length &&
                grid[top.x+1][top.y] !=0 &&
                !visited[top.x+1][top.y]) {
                q.add(new Node(top.x+1, top.y, top.dist+1));
            }

            // up
            if (top.x-1 > 0 &&
                grid[top.x-1][top.y] !=0 &&
                !visited[top.x-1][top.y]) {
                q.add(new Node(top.x-1, top.y, top.dist+1));
            }

            // left
            if (top.y-1 > 0 &&
                grid[top.x][top.y-1] !=0 &&
                !visited[top.x][top.y-1]) {
                q.add(new Node(top.x, top.y-1, top.dist+1));
            }

            // right
            if (top.y+1 < grid[0].length &&
                grid[top.x][top.y+1] !=0 &&
                !visited[top.x][top.y+1]) {
                q.add(new Node(top.x, top.y+1, top.dist+1));
            }
        }
        return -1;
    }
}

```
