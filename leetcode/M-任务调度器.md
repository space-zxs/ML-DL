## 任务调度器

### 题目描述

[题目连接](https://leetcode.cn/problems/task-scheduler/description/)


### 思路
 - 审题可以两个相同任务之间有至少n的执行冷却时间，可以等待也可以执行其他任务
 - 当n==0 时，那么就是任务长度
 - 否则，一个初始的最小长度应该是任务最多的字母-1 *（n+1） + 1
 - 遍历剩下的字母，如果等于最大字母数，那么最后肯定要多一个数字
 - 如果空位不够插，那么不需要管空位，随便插就可以，这时候就是任务总数

---

```

class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        /**
          * 解题思路：
     * 1、将任务按类型分组，正好A-Z用一个int[26]保存任务类型个数
     * 2、对数组进行排序，优先排列个数（count）最大的任务，
     *      如题得到的时间至少为 retCount =（count-1）* (n+1) + 1 ==> A->X->X->A->X->X->A(X为其他任务或者待命)
     * 3、再排序下一个任务，如果下一个任务B个数和最大任务数一致，
     *      则retCount++ ==> A->B->X->A->B->X->A->B
     * 4、如果空位都插满之后还有任务，那就随便在这些间隔里面插入就可以，因为间隔长度肯定会大于n，在这种情况下就是任务的总数是最小所需时间
     */
        //两个相同的任务之间间隔n个任务
        if(n == 0) return tasks.size();
        vector<int> hash(26);
        //把字母出现的次数存储  
        for(int i = 0; i < tasks.size(); i++){
            hash[tasks[i] - 'A']++;
        }
        //排序，先处理出现次数最大的字母，方便确定长度
        sort(hash.begin(), hash.end());
        int minlen = (hash[25] - 1) * (n + 1) + 1;//确定长度
        for(int i = 24; i >= 0;i--){
            if(hash[i] == hash[25]) minlen++;
        }
        //加了间隔的长度肯定比tasks.size() 大
        return  minlen > tasks.size() ? minlen : tasks.size();
    }
};
```
