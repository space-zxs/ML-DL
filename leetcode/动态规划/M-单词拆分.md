  ## 单词拆分

  ### 思路

  - 动态规划
  - 设dp[i] 表示前i个字符中是否能由单词组成
  - dp[i] = dp[j] & check(s.substr(i-j));
  - 如果成立直接break。

----
```
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        //dp[i] 0...i-1   表示前i个能否被拆分成若干个单词

        // dp[0] = 0 dp[i] = dp[j] & s.substr(j,i-j)   
        // 用map存储单词
        unordered_map<string, int> mp;
        for(auto& word : wordDict){
            mp[word]++;
        }

        vector<int> dp(s.size() + 1);
        dp[0] = 1;
        for(int i = 1; i <= s.size(); i++){
            for(int j = 0; j <= i;j++){
                if(dp[j] && mp.find(s.substr(j, i-j)) != mp.end()){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.size()];
    }
};

```
