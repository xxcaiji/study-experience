### 解题思路
利用双端队列可以加快遍历的速度，首先将对目前状况有优势的结点先遍历，不占优势的结点则后遍历。
典型的迷宫问题。
### 代码

    ```cpp
    class Solution {
        int index[4][2] = {{0 , 1} , {0 , -1} , {1 , 0} , {-1 , 0}};
        int tag[4] = {1 , 2 , 3 , 4};
    public:
        int minCost(vector<vector<int>>& grid) {
        int m = grid.size() , n = grid[0].size();
        vector<vector<bool>> cnt(m , vector<bool> (n , false));
        vector<vector<int>> rec(m , vector<int>(n , INT_MAX));
        deque<pair<int , int>> q;
        q.emplace_front(make_pair(0 , 0));
        rec[0][0] = 0;
        while(!q.empty()) {
            auto [y , x] = q.front();
            q.pop_front();
            if(cnt[y][x]) continue;
            cnt[y][x] = true;
            for(int i = 0; i < 4; i++) {
                int yy = y + index[i][0];
                int xx = x + index[i][1];
                if(yy < m && yy >= 0 && xx < n && xx >= 0) {
                    int t = grid[y][x] != tag[i];
                    if(rec[yy][xx] > rec[y][x] + t) {
                        rec[yy][xx] = rec[y][x] + t;
                    }
                    grid[y][x] == tag[i] ? q.emplace_front(make_pair(yy , xx)) : q.emplace_back(make_pair(yy , xx));
                }
            }
        }
        return rec[m-1][n-1];
    }
    };
    ```