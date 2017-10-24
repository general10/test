# 解题方案：

## 标签：搜索

### 题意：
给了你一个由数字组成的字符串

问你分割这个字符串能得到多少种合法ip

样例输入：25525511135

样例输出：255.255.11.135 255.255.111.35

### 思路：
首先我们先了解一下什么是合法ip

合法ip一共有三个点 分割出来四个数

每个数不能有前导零且<255


这里我们用搜索去解决这个问题 

自然就要有条件才能继续

那么现在我们把我们的要求写下来
```
num<256
```
而且要保证
```
th.size()>1&&th[0]!='0'
```

在这个数不是零时没有前导零

然后我们去寻找每一段合法的数字

并记录下来 然后去寻找这个可能合理的ip的下一部分

我们需要记录三个量 

- 当前可能合法ip

- 当前处理到的s的位数 

- 当前已经处理的几个数

用pair<string,pii>来维护这个数据

我们判定是不是已经处理了4个数而且已经处理到了最后一位来看是不是已经结束了

```
if(now.second.first==s.size()-1&&now.second.second==4)
```
这个是合法的条件

接下来我们只要让程序自己去找合法解就行了


### 代码：

```
vector<string> restoreIpAddresses(string s)
{
    vector<string>ans;
    ans.clear();
    queue<pair<string,pii>>q;//str len point
    while(!q.empty()) q.pop();
    q.push({"",{-1,0}});
    while(!q.empty())
    {
        pair<string,pii> now=q.front();
        q.pop();
        if(now.second.first==s.size()-1
         &&now.second.second==4)
        {
            ans.push_back(now.first.substr(0,now.first.size()-1));
            continue;
        }
        if(now.second.second>=4) continue;
        string th="";
        for(int i=now.second.first+1;i<s.size();i++)
        {
            th+=s[i];
            int num=0;
            for(auto i:th) num*=10,num+=i-'0';
            if(num<256)
            {
                if(th.size()>1&&th[0]=='0') break;
                string str=now.first;
                str+=th;
                str+=".";
                int len=i;
                int point=now.second.second+1;
                q.push({str,{len,point}});
            }
            else break;
        }
    }
    return ans;
}
```