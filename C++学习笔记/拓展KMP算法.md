 #### Z算法也称为拓展KMP算法

    class Solution {
    public:
        //Z算法
        long long sumScores(string s) {
            long long ans = 0;
            int n = s.size();
            vector<long long> z(n , 0);
            for(int i = 0 , l = 0,  r = 0 ; i < n; i++) {
                if(i <= r && z[i - l] < r - i + 1) {
                    z[i] = z[i - l];
                }else{
                    z[i] = max(0 , r - i + 1);
                    while(i + z[i] < n && s[z[i]] == s[i + z[i]]) ++z[i];
                }
                if (i + z[i] - 1 > r) l = i, r = i + z[i] - 1;
                ans += z[i];
            }
            return ans + n;
        }
    };