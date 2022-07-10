# My Solution
## 741.摘樱桃
是一道一看我就不会做的hard题。  

勉强能看懂大佬三叶的答案，但对其中的部分代码存在一些疑问，简单记录一下。  

- 本题的基础建立在将来回的过程，等价为两个人同时从左上走到右下。如此一来对于樱桃的计数也必须去重。
- 此处对于f(k)(i1)(i2)的解释为：两个人都走了k步，第一个人在i1行，第二个人在i2行，此时的最大樱桃数。
```java
class Solution {
    static int N = 55, INF = Integer.MIN_VALUE;
    static int[][][] f = new int[2 * N][N][N];
    public int cherryPickup(int[][] g) {
        int n = g.length;
        for (int k = 0; k <= 2 * n; k++) {
            for (int i1 = 0; i1 <= n; i1++) {
                for (int i2 = 0; i2 <= n; i2++) {
                    f[k][i1][i2] = INF;
                }
            }
        }
        f[2][1][1] = g[0][0];
        for (int k = 3; k <= 2 * n; k++) {
            for (int i1 = 1; i1 <= n; i1++) {
                for (int i2 = 1; i2 <= n; i2++) {
                    // 根据定义，由于都走了k步骤，且当前在i1、i2行，容易得出纵坐标在k-i1、k-i2列
                    int j1 = k - i1, j2 = k - i2;
                    if (j1 <= 0 || j1 > n || j2 <= 0 || j2 > n) continue;
                    // 由于f中的行列是从1开始的，所以需要分别减去1才能取到数组中当前的位置
                    int A = g[i1 - 1][j1 - 1], B = g[i2 - 1][j2 - 1];
                    // 此时说明有一个人走不通了，需要换一条路径走
                    if (A == -1 || B == -1) continue;
                    int a = f[k - 1][i1 - 1][i2], b = f[k - 1][i1 - 1][i2 - 1], c = f[k - 1][i1][i2 - 1], d = f[k - 1][i1][i2];
                    int t = Math.max(Math.max(a, b), Math.max(c, d)) + A;
                    if (i1 != i2) t += B;
                    f[k][i1][i2] = t;
                }
            }
        }
        return f[2 * n][n][n] <= 0 ? 0 : f[2 * n][n][n];
    }
}
```

