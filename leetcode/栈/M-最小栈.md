## 最小栈

### 题目描述

[题目链接](https://leetcode.cn/problems/min-stack/description/)


### 思路

- 主要是如何维护每次的最小值
- 还要在o1的时间内
- 可以在使用一个栈维护最小值
- 两个栈相对应

  ---

  ```
class MinStack {
public:
    stack<int> st;
    //在设置一个栈，维护最小值
    stack<int> index;
    MinStack() {
        index.push(INT_MAX);
    }
  
    
    //如何在常数时间内维护一个最小值
    void push(int val) {
        st.push(val);
        index.push(min(val,index.top()));
    }
    
    void pop() {
        index.pop();
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return index.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */

  ```
