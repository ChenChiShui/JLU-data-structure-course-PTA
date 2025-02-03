## **A 调皮的哈利（链表）**

分数 30

全屏浏览题目切换布局

作者 朱允刚

单位 吉林大学

贝蒂是个打字高手，打字时有不看屏幕的习惯。在一次贝蒂打字时，调皮的哈利常常趁贝蒂不注意按下Home键、End键、左右方向键和退格键。当Home键被按下时，输入光标会跳到文本最开头；当End键被按下时，输入光标会跳到文本末尾；当左/右方向键被按下时，输入光标会向左/右移动一位；当退格键被按下时，输入光标左面的一个字符会被删除。现给出贝蒂和哈利按键的字符串，其中'{'表示Home键，'}'表示End键，'<'表示左方向键，'>'表示右方向键，'#'表示退格键，其余字符均表示输入的内容，请输出屏幕上最终显示的文本。

![img.jpg](https://images.ptausercontent.com/c30b8eea-c661-4ec2-b4ec-fd35d6e9dd01.jpg)

输入一个字符串，长度不超过5×104，包含大小写字母、空格、下划线、{、}、<、>、#，表示贝蒂和哈利的按键序列。

输出为幕上最终显示的字符串。

```in
jlu_cc{i_love_}st
```

```out
i_love_jlu_ccst
```

```in
stre<<aaa
```

```out
staaare
```

```in
xxx>>>yyy##z<<>k
```

```out
xxxykz
```

```in
abcd{efghi}jklm{nopq}rs{t}uvwxyz
```

```out
tnopqefghiabcdjklmrsuvwxyz
```

### ANSWER

```c++
#include <iostream>
#include <string>
using namespace std;

struct Node {
    char data;
    Node* prev;
    Node* next;
    Node(char c) : data(c), prev(nullptr), next(nullptr) {}
};

int main() {
    string origin;
    getline(cin, origin);

    Node* head = new Node('*');
    Node* tail = head;
    Node* current = head;
    int trueLong = 0;

    for (auto& c : origin) {
        if (c == '{') {
            current = head;
        } else if (c == '}') {
            current = tail;
        } else if (c == '<') {
            if (current != head) {
                current = current->prev;
            }
        } else if (c == '>') {
            if (current != tail) {
                current = current->next;
            }
        } else if (c == '#') {
            if (current == head)
                ;
            else if(current == tail )
            {
                tail = current->prev;
                tail->next = nullptr;
                delete current;
                current = tail;
            }
            else
            {
                Node *temp = current->prev;
                current->prev->next = current->next;
                current->next->prev = current->prev;
                delete current;
                current = temp;
            }

        } else {
            Node* newNode = new Node(c);
            newNode->prev = current;
            newNode->next = current->next;

            if (current->next != nullptr) {
                current->next->prev = newNode;
            } else {
                tail = newNode;
            }

            current->next = newNode;
            current = newNode;
            trueLong++;
        }
    }

    string ans;
    current = head->next;
    while (current != nullptr) {
        ans.push_back(current->data);
        current = current->next;
    }
    cout << ans << endl;
    return 0;
}
```

## **B 括号匹配**

分数 20

全屏浏览题目切换布局

作者 朱允刚

单位 吉林大学

请编写程序判断一个包含“（”和“）”的括号序列是否匹配。如匹配则输出Match；如不匹配，计算出使该序列变为匹配序列所需添加的最少括号数目（只允许在该序列开始和结尾处添加括号），并输出经添加最少括号后得到的合法匹配序列。

输入为若干行，每行一个字符串，包含不超过105个括号。输入行数不超过10行。

对于输入的每个括号序列输出1行或2行信息。若输入的括号序列匹配，则输出Match。若不匹配，则输出分为2行，第1行为一个整数，表示将该序列变为匹配序列所需添加的最少括号数目，第2行为一个字符串，表示经添加最少括号后得到的合法匹配序列。

```in
(())()
)(
()))((
```

```out
Match
2
()()
4
((()))(())
```

### ANSWER

```c++
#include <bits/stdc++.h>
using namespace std;
int main(void)
{

    string origin ;
    string input;
    int deadInt = 0;
    while (getline(cin, input))
    {

        if(input.empty())
            break;
        stack<bool>s;

        int n = input.size();
        for(int i = 0; i < n;i++)
        {
            if(input[i] == ')' && !s.empty() && s.top() == true)
                s.pop();
            else if(input[i] == '(')
                s.push(true);
            else if(input[i] == ')')
                s.push(false);
        }
        if(s.empty())
            cout << "Match" << endl;
        else
        {
            cout << s.size() << endl;

            int last = s.size();
            int count = 0;
            while (!s.empty()) {
                if (s.top() == true) {
                    count++;
                }
                s.pop();
            }
            string matchedString = string(last - count, '(') + input;

            matchedString += string (count, ')');// 添加匹配的括号
            cout << matchedString << endl;
        }
    }
    return 0;
}
```

## **C 表达式求值（中缀表达式）**

分数 30

全屏浏览题目切换布局

作者 朱允刚

单位 吉林大学

给定一个中缀表达式，请编写程序计算该表达式的值。表达式包含+、-、*、\、^、(、)，所有运算均为二元运算，操作数均为正整数，但可能不止一位，不超过10位。运算结果为整数，值域为[−231,231)。除法运算结果若为小数则进行截尾取整。若除法运算中除数为0，则输出INVALID。幂运算须自行实现，不允许调用pow等系统函数。**测试数据保证幂运算中指数为非负，底数不为****0****。**

输入为多行，每行为一个长度不超过1000的字符串，表示中缀表达式。

对每个表达式输出一行：为一个整数（表达式的值）或为一个字符串INVALID。

```in
5+(10*2)-6
8*(999+1)
1+5/(1-1)
7*2^3
```

```out
19
8000
INVALID
56
```

### ANSWER

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef int ll;

ll oprToNums(char c, ll num2, ll num1, bool &isLegal)
{
    if (c == '+')
        return num1 + num2;
    if (c == '-')
        return num1 - num2;
    if (c == '*')
        return num1 * num2;
    if (c == '/')
    {
        if (num2 == 0)
        {
            if (isLegal)
                cout << "INVALID" << endl;
            isLegal = false;
            return 0;
        }
        return num1 / num2;
    }
    if (c == '^')
    {
        if(num2 < 0 || num1 == 0)
            exit;
        ll ans = 1;
        while (num2 > 0)
        {
            if (num2 % 2 == 1)
                ans *= num1;
            num1 *= num1;
            num2 /= 2;
        }
        return ans;
    }
}

int main(void)
{
    map<char, int> operLevel;
    operLevel['#'] = -1;
    operLevel['('] = 0;
    operLevel['+'] = 1;
    operLevel['-'] = 1;
    operLevel['*'] = 2;
    operLevel['/'] = 2;
    operLevel['^'] = 3;

    string input;
    bool isFirst = true;
    while (getline(cin, input))
    {
        isFirst = false;
        ll n = input.size();
        bool isLegal = true;
        stack<char> opr;
        opr.push('(');
        stack<ll> num;
        string temNum = "";
        for (ll i = 0; i < n; i++)
        {
            char c = input[i];
            if (c <= '9' && c >= '0')
            {
                temNum.push_back(c);
                if ((input[i + 1] > '9' || input[i + 1] < '0') || i + 1 == n)
                {
                    num.push(stoll(temNum));
                    temNum.clear();
                }
            }
            else if (c == '(')
            {
                opr.push(c);
                continue;
            }
            else if (c == ')')
            {
                while (opr.top() != '(')
                {
                    char nowOpr = opr.top();
                    opr.pop();
                    ll num1 = num.top();
                    num.pop();
                    ll num2 = num.top();
                    num.pop();
                    num.push(oprToNums(nowOpr, num1, num2, isLegal));
                }
                opr.pop();
            }
            else if (operLevel[c] > operLevel[opr.top()])
            {
                opr.push(c);
                continue;
            }
            else if (operLevel[c] <= operLevel[opr.top()])
            {
                while(operLevel[c] <= operLevel[opr.top()])
                {
                    int nowOpr = opr.top();
                    opr.pop();
                    ll num1 = num.top();
                    num.pop();
                    ll num2 = num.top();
                    num.pop();
                    num.push(oprToNums(nowOpr, num1, num2, isLegal));
                }
                opr.push(c);
            }
        }
        while (opr.top() != '(')
        {
            char nowOpr = opr.top();
            opr.pop();
            ll num1 = num.top();
            num.pop();
            ll num2 = num.top();
            num.pop();
            num.push(oprToNums(nowOpr, num1, num2, isLegal));
        }
        if (isLegal)
            cout << num.top() << endl;
    }
    return 0;
}
```

## **D EDG（KMP）**

分数 20

全屏浏览题目切换布局

作者 朱允刚

单位 吉林大学

2021年11月6日，英雄联盟全球总决赛打响，中国电子竞技战队Edward Gaming（EDG）以3:2力克韩国强敌DWG KIA（DK）战队，历史上首次夺得全球总冠军。一时间全网沸腾，大家纷纷在社交平台上直呼“edgnb”。现给定一段文本，请编写程序识别出连续的k个“edgnb”组成的字符串在该文本中出现了多少次。

第一行为1个整数T，表示数据组数。对于每组数据，第一行为1个字符串，表示给定的文本。第二行为1个整数k，含义如题目所述。（1≤T≤10。各组数据给定的字符串长度之和不超过105，且字符串中只包含a-z的小写字母。k≥1且k×5小于给定字符串长度）。

对于每组数据输出一行，为1个整数，表示所求的出现次数。

```in
5
xyzedgnbabcedgnb
1
xyzedgnbabcedgnb
2
defedgnbedgnbxyz
2
edgnbedgnbedgnb
2
fxedgnbedgnbedgnbedgnbmem
3
```

```out
2
0
1
2
2
```

### ANSWER

```c++
#include <bits/stdc++.h>
using namespace std;
int * getNext(string a)//kmp算法核心是构建next数组
{
    int n = a.size();
    int * next = new int[n];//将next存再堆中，防止内存清空
    int i = 1;
    next[0] = 0;//从1开始数，因为next的第一位一定是0
    int preFixLen = 0;// preFixLen是前缀长度，具体见上述视频
    while (i < n)
    {
        if(a[preFixLen] == a[i])//当前后缀元素相等
        {
            preFixLen++;//前缀长度增加1
            next[i] = preFixLen;
            i++;//指针向前挪1位
        }
        else//元素不相等
        {
            if(preFixLen == 0)
            {
                next[i] = 0;//前缀为空，则必不匹配
                i++;//指针前挪，不需要递推
            }
            else
            {
                preFixLen = next[preFixLen - 1];//递推，见视频07：36
            }
        }
    }
    return next;
}
int strStr(string s, string target)
{
    int n = s.size(), m = target.size();
    int i = 0;//主串字串的下标都放在循环外
    int j = 0;
    int ans = 0;
    int* next = getNext(target);
    // 思路见上述视频中的02：26
    while (i < n)
    {
        if(s[i] == target[j])//如果主串字串元素先等，则下表都加1
        {
            i++;
            j++;
        }
        else
        {
            if(j == 0)//如果字串下标为0，则直接主串下表加1
                i++;
            else
                j = next[j - 1];//用next数组判断字串下标的位置
            //此行为KMP的核心，可以多看几次视频研究
        }
        if( j == m)//如果j判断完，则说明找到匹配
            ans++;
    }
    return ans;
}
int main(void)
{
    int lines;
    cin >> lines;
    for(int line = 0; line < lines ;line++)
    {
        int longStr;
        string newOne;
        cin >> newOne;
        cin >> longStr;
        string copy;
        for(int longStr0 = 0; longStr0 < longStr; longStr0++)
        {
            copy.push_back('e');
            copy.push_back('d');
            copy.push_back('g');
            copy.push_back('n');
            copy.push_back('b');
        }

        int i = strStr(newOne, copy);
        cout << i << endl;
    }
    return 0;
}
```

