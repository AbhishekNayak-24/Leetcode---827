# Leetcode---827
Making A Large Island
//Code in java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    private int n;
    private int[][] grid;
    private int[] directions = {0, 1, 0, -1, 0};

    public int largestIsland(int[][] grid) {
        this.n = grid.length;
        this.grid = grid;
        int index = 2;
        Map<Integer, Integer> areaMap = new HashMap<>();
        areaMap.put(0, 0);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    int area = dfs(i, j, index);
                    areaMap.put(index++, area);
                }
            }
        }

        int maxArea = areaMap.getOrDefault(2, 0);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    Set<Integer> seen = new HashSet<>();
                    int area = 1;
                    for (int k = 0; k < 4; k++) {
                        int x = i + directions[k];
                        int y = j + directions[k + 1];
                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            int indexNeighbor = grid[x][y];
                            if (indexNeighbor > 1 && seen.add(indexNeighbor)) {
                                area += areaMap.get(indexNeighbor);
                            }
                        }
                    }
                    maxArea = Math.max(maxArea, area);
                }
            }
        }

        return maxArea;
    }

    private int dfs(int i, int j, int index) {
        if (i < 0 || i >= n || j < 0 || j >= n || grid[i][j] != 1) {
            return 0;
        }
        grid[i][j] = index;
        int area = 1;
        for (int k = 0; k < 4; k++) {
            int x = i + directions[k];
            int y = j + directions[k + 1];
            area += dfs(x, y, index);
        }
        return area;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int[][] grid = {
            {1, 0},
            {0, 1}
        };
        System.out.println(solution.largestIsland(grid)); // Output: 3
    }
}
