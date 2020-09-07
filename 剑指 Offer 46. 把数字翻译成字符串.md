## 剑指 Offer 46. 把数字翻译成字符串
给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
 

提示：

0 <= num < 231

```cpp
class Solution {
public:
    int ans;
    map<int, char> m;
    string str;
    void dfs(std::string s, int k){
        if(k >= str.length()) {
            cout << s << endl;
            ans++;
            return;
        }
        //取当前index的一位数字
        dfs(s + m[str[k] - '0'], k + 1);
        //取两位数字
        int num_2 = (str[k] - '0') * 10 + (str[k + 1] - '0');
        if(str.length() - k >= 2 && str[k] - '0' != 0 && num_2 < 26){
            dfs(s + m[num_2], k + 2);
        }
    }
 
    int dp(){
        vector<int> arr_dp(str.length() + 1);
        arr_dp[0] = 1;
        arr_dp[1] = 1;
        for(int i = 1; i < str.length(); i++){
            if((str[i - 1] - '0') * 10 + (str[i] - '0') < 26
                && (str[i - 1] - '0') * 10 + (str[i] - '0') >= 10){
                    arr_dp[i + 1] = arr_dp[i] + arr_dp[i - 1];
                }
            else{
                arr_dp[i + 1] = arr_dp[i];
            }
        }
        return arr_dp[str.length()];
    }

    int translateNum(int num) {
        str = std::to_string(num);
        // for(int i = 0; i < 26; i++){
        //     m.insert(pair<int, char>(i, (char)('a' + i)));
        // }
        // ans = 0;
        // dfs("", 0);
        // return ans;
        return dp();
    }
};
```
