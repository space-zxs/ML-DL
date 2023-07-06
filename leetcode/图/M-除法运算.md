## 除法运算

### 题目描述
[题目连接](https://leetcode.cn/problems/evaluate-division/description/)


### 思路
  - 构建拓扑图
  - 宽度优先遍历
  - 转换字母为数字同时计算顶点个数
  - 使用二维数组存储边
  - 遍历查询数组
  - 使用队列，宽度遍历整个图，同时计算每个结点的值，更新
  - 知道遍历到终点
  - 加入res数组
  - 输出

    ---

    ```
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        //建立一个拓扑排序图
        unordered_map<string,int> vars;
        
        int nvras = 0;
        for(auto& equation:equations){
            if(!vars.count(equation[0])){
                vars[equation[0]] =nvras++;
            }
            if(!vars.count(equation[1])){
                vars[equation[1]] = nvras++;
            } 
        }
        vector<vector<pair<int,double>>> edges(nvras);
        for(int i = 0; i < equations.size(); i++){
            edges[vars[equations[i][0]]].push_back({vars[equations[i][1]],values[i]});
            edges[vars[equations[i][1]]].push_back({vars[equations[i][0]], 1.0/values[i]});
        }
        vector<double> res;
        for(auto& q: queries){
            double val = -1;
            if((vars.find(q[0]) != vars.end() && vars.find(q[1]) != vars.end())){
                int ia = vars[q[0]];
                int ib = vars[q[1]];
                if(ia == ib){
                    val= 1.0;
                }else{
                    queue<int> qu;
                    qu.push(vars[q[0]]);
                    vector<double> ratios(nvras,-1);
                    ratios[ia] = 1.0;
                    while(!qu.empty()&& ratios[ib] < 0){
                        int x = qu.front();
                        qu.pop();
                        for(auto v:edges[x]){
                            if(ratios[v.first] < 0){
                                ratios[v.first] = v.second * ratios[x];
                                qu.push(v.first);
                            }
                        }
                    }
                   val = ratios[ib];
                }
                
            }
            res.push_back(val);

        }
        return res;
    }
};

    ```
