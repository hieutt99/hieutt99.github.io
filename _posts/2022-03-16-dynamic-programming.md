---
title:  "Dynamic Programming"
categories: "Computer Science"
toc: true
tags:
    - Algorithms
---



Dynamic Programming hay quy hoạch động hay DP là phương pháp nhằm giảm thời gian chạy trong các bài toán có tính chất ***overlapping subproblem*** và ***optimal substructure***. DP được chia làm 2 hướng tiếp cận chính là memoization hay top-down và tabulation hay bottom-up. 

Bài blog này được viết với hy vọng ease the pain của việc học thuật toán Dynamic Programming. Lưu ý rằng trình độ của người viết cũng có hạn và cũng không có nhằm đến mục tiêu gì cao siêu ngoài việc viết 1 bài post với kỳ vọng giúp cho việc tiếp cận DP (ở mức cơ bản) dễ hơn và ghi lại những gì chính mình đã học. Bài viết sử dụng 2 ngôn ngữ là Python và C++. Lý do cho việc sử dụng C++ thì là vì nhiều người sử dụng nó trong competitive programming, chứ nói thật tôi cũng không thạo C++. Tuy nhiên thì theo tôn chỉ của ĐHBK thì làm việc khó thì các bạn mới học được nhìu so ... 

# Memoization 
Ý tưởng của memoization là lưu lại các kết quả tính toán từ các function calls và trả cached results để tối ưu việc tính toán khi gặp các kết quả với inputs tương tự. 
Để dễ hiểu hơn, chúng ta có thể xét bài toán Fibonacci. Dãy Fibonacci là dãy vô hạn các số tự nhiên được bắt đầu với hai phần tử 0 và 1, các phần tử sau đó được thiết lập theo quy tắc phần tử sau bằng tổng hai phần tử đứng trước nó. Ta có công thức truy hồi của dãy Fibonacci như sau: 

$$
F(n):=
    \begin{cases}
        1 & \text{khi n=1;}\\
        1& \text{khi n=2;}\\
        F(n-1)+F(n-2)& \text{khi n>2.}
    \end{cases}
$$


Problem có thể được giải quyết bằng cách giải cổ điển đó là sử dụng recursive algorithm. Tuy nhiên đặc điểm thường thấy của giải thuật đệ quy đó là khối lượng tính toán lớn và việc lặp tính toán các giá trị là nhiều. Do đó hiển nhiên là chúng ta có nhu cầu cải tiến thuật toán để khắc phục yếu tố computational cost. Và hướng tiếp cận có thể thấy là việc lặp các giá trị đầu vào mỗi khi gọi các hàm đệ quy. Trước hết chúng ta xét code đệ quy cho bài toán tìm dãy Fibonacci:

```python
## recursive
def fib(n):
    if (n<=2): 
        return 1 
    else:
        return fib(n-2)+fib(n-1)

fib(50)
```

Lý giải đơn giản cho việc lặp giá trị thì ví dụ chúng ta gọi hàm $fib(n)$ cho $n=5$ thì hàm sẽ gọi đến $fib(4)$ và $fib(3)$. Khi gọi đến hàm $fib(4)$ ta lại gọi $fib(3)$ và $fib(2)$. Như vậy có thể thấy chúng ta đã gọi giá trị $fib(3)$ đến 2 lần trong việc tính toán.

Thay vì tính toán lại các giá trị như $fib(3)$ vốn đã được call trước đó, chúng ta có thể khởi tạo 1 cached memory để chứa các giá trị đã tính toán rồi và kiểm tra nếu giá trị cần tính toán liệu đã được tính toán hay chưa. Code cho hướng tiếp cận này: 
```python
### memoization

def fib(n: int, memo={}):
    if n in memo: return memo[n]
    if n <= 2: return 1
    memo[n] = fib(n-2, memo)+fib(n-1, memo)
    return memo[n]

print(fib(50))
```

hay là code bằng ngôn ngữ C++:

```cpp
#include <bits/stdc++.h>
using namespace std;

// long long int for the sake of variable type range
vector<long long int> memo = {0,1,1};

long long int fib(int n){
    if (n<memo.size()){
        return memo[n];
    }
    else{
        memo.push_back(fib(n-1)+fib(n-2));
    }
    return memo[n];
}


int main(){
    long long int res = fib(50);
    printf("res: %lld", res);
    return 0;
}
```

# Tabulation 

So với hướng tiếp cận memoization vốn là rẽ nhánh từ recursive algorithm. Tabulation là hướng tiếp cận nhằm loại bỏ sự bất lợi của việc phải gọi quá nhiều hàm đệ quy, hay là một hướng tiếp cận khử đệ quy.

Cùng với bài toán dãy Fibonacci, chúng ta có thể nhìn theo hướng rằng tác vụ cần hoàn thành là điền vào bảng cho đến khi đạt đến giá trị n và cuối cùng là trả về giá trị n. Như vậy, trong quá trình tính toán sẽ không có việc thực hiện lặp tính toán mà các giá trị trước đó được tính toán hay biểu diễn bằng đệ quy đã được lưu trong bảng. 

Code cho tabulation đối với bài toán dãy Fibonacci với ngôn ngữ Python:

```python 
### tabulation 
def fib(n: int):
    table = [0, 1]
    for i in range(2, n+1):
        table.append(table[i-1]+table[i-2])
    return table[n]

print(fib(50))
```

với ngôn ngữ C++:

```cpp
#include <bits/stdc++.h>
using namespace std;

long long int fib(int n){
    vector<long long int> table = {0,1};
    for (int i = 2; i<=n;i++){
        table.push_back(table[i-1] + table[i-2]); 
    }
    return table[n];
}

int main(){
    long long int res = fib(50);
    printf("res: %lld", res);
    return 0;
}
```

# So sánh giữa Memoization và Tabulation

![/assets/images/Tabulation-vs-Memoization-1.png](/assets/images/Tabulation-vs-Memoization-1.png)

# Một số ví dụ với Dynamic Programming 

## LeetCode

Các problem được lấy từ medium problems được gắn tag dynamic programming với định hướng rằng các problem này là vừa đủ (không quá khó hay quá dễ) để làm quen với việc nhìn các pattern của DP. 

### Full Binary Trees
<a href="https://leetcode.com/problems/all-possible-full-binary-trees/" target="_blank">Problem</a>

Quan sát:
- Full Binary Tree hay FBT thì số nốt chẵn sẽ không tạo được bất kì cây nào hợp lệ nên là nếu đầu vào chẵn là loại luôn 
- trừ base case $n=1$ ra thì xét các trường hợp lẻ lớn hơn 1, nhánh trái và phải của nó cũng phải là FBT thì cây đó mới hợp lệ. Từ đây ta thấy có mùi của DP rồi. 
- Xét các chỉnh hợp thì có thể thấy rằng ví dụ $n = 7 = 1 + 1 + 5 = 1 + 3 + 3 = 1 + 5 + 1$, hay giải thích dễ hiểu hơn thì ta đang xét hoán vị các cây con có cùng số node mà tổng số node + 1 thì bằng state đang xét. 

Từ quan sát thì ta đưa ra thuật toán: tại state $i$ thì các cây của $i$ được form bằng chỉnh hợp của các state trước đó với tổng số node cộng $1$ bằng state $i$. Điều này có thể thực hiện iter $j$ từ $1$ đến state trước $i$ và bên còn lại là $i-j-1$

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> allPossibleFBT(int n) {
        vector<vector<TreeNode*>> dp(n+1);
        if (n % 2 == 0)  return vector<TreeNode*>();

        dp[1].push_back(new TreeNode(0));
        for (int i = 3; i<=n;i+=2){
            for(int j = 1; j<i;j+=2){
                for (int k = 0; k<dp[j].size(); k++){
                    for (int l = 0; l<dp[i-j-1].size(); l++){
                        TreeNode* root = new TreeNode(0);
                        root->left = dp[j][k];
                        root->right = dp[i-j-1][l];
                        dp[i].push_back(root);
                    }
                }
            }
        }


        return dp[n];
    }
};
```

### Substrings that differ by one character
<a href="https://leetcode.com/problems/count-substrings-that-differ-by-one-character/" target="_blank">Problem</a>

Quan sát: gần giống bài toán substring nhưng thêm điều kiện là khác nhau 1 character. Như vậy thì khả năng là ta cần chia nhỏ ra xét substring trước và xét điều kiện khác 1 character sau. 

note: cần đếm số substring tại state (i, j). sau đó thì nếu nhận thấy trường hợp kí tự khác nhau thì tổng số substring từ (i-1, j-1) cộng 1 ra số string diff 1 character thỏa mãn . nếu nó giống nhau thì số string diff 1 character kế thừa từ state (i-1, j-1) hay nói chính xác hơn là nó vẫn giống và trước đấy có substring thì nó tiếp tục giống với số lượng substring thỏa mãn ko đổi, còn nếu nó ko có thì nó là 0 (không đổi).


```cpp
class Solution {
public:
    int countSubstrings(string s, string t) {
        int m = s.length();
        int n = t.length();
        vector<vector<int>> count_subs(m+1, vector<int>(n+1, 0));
        vector<vector<int>> count_diffs(m+1, vector<int>(n+1, 0));

        int cnt = 0;
        for (int i=1;i<=m;i++){
            for (int j=1;j<=n;j++){
                if (s[i-1] == t[j-1]){
                    count_subs[i][j] = count_subs[i-1][j-1] + 1;
                    count_diffs[i][j] = count_diffs[i-1][j-1];
                    cnt += count_diffs[i][j];
                }
                else{
                    count_subs[i][j] = 0;
                    count_diffs[i][j] = count_subs[i-1][j-1]+1;
                    cnt += count_diffs[i][j];
                }
            }
        }
        return cnt;
    }
};
```

### Count sorted vowel strings 
<a href="https://leetcode.com/problems/count-sorted-vowel-strings/" target="_blank">Problem</a>

Quan sát: ta có 5 vowels và có thể thấy là mỗi state tương ứng với độ dài của các string thì chỉ là ta thêm 1 trong 5 vowels vào các string đã có của state trước với điều kiện là string đó hợp lệ. Vậy thì tổng số có thể tạo được là tổng số string ở state cuối có thể tạo được của 5 vowels. Tuy nhiên có 1 lưu ý là vowels cần được xếp theo thứ tự và sẽ không thể thêm vowel có thứ tự sau vào string đang có kí tự đầu là vowel đứng trước. Do đó đối với mỗi state, số lượng string được tạo cho mỗi vowels là tổng của state trước nhưng chỉ tính từ vowel tương ứng đến cuối. 

note: lưu ý bài này chỉ cần đếm, nếu như form tất cả các trường hợp của string thì sẽ gây TLE. ngược lại chỉ đếm và cộng thì kết quả sẽ rất nhanh . đây có lẽ là cách làm nhanh nhất trong cái submission rồi. 


```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        vector<vector<int>> dp(n+1);
        dp[1] = {1, 1, 1, 1, 1};

        int cnt = 0;
        for(int i = 2;i<=n;i++){
            for (int j = 0; j<5; j++){
                cnt = 0;
                for (int k = j; k<5;k++){
                    cnt += dp[i-1][k];
                }
                dp[i].push_back(cnt);
            }
        }

        int total = 0;
        for (int i = 0; i < 5; i++){
            total += dp[n][i];
        }
        return total;
    }
};
```

###  Count square submatrices with all ones

<a href="https://leetcode.com/problems/count-square-submatrices-with-all-ones/" target="_blank">Problem</a>

Quan sát: để nó là 1 hình vuông có cạnh là i thì trước hết nó phải chứa đâu đó 1 hình vuông có cạnh là i-1 đã, với điều kiện là i lớn hơn 1. 

Thuật toán: đầu tiên phải xác định là base case là cạnh 1. Ta tạo ra mảng quy hoạch $dp$ chứa giá trị vào góc phải dưới của hình vuông là boolean liệu ô đó có phải là hình vuông hay ko. Xác định điều đó tất nhiên là phải duyệt qua cả 4 góc. Tuy nhiên nhìn bài toán theo hướng đệ quy thì chúng ta đang thu hẹp bài toán dưới mô hình là các hình vuông sẽ chỉ có 4 điểm 0/1 được kế thừa sau mỗi state và ma trận quy hoạch sẽ thu hẹp lại dần vào hướng phải dưới.  

note: đây là cách làm cá nhân và nó chắc chưa phải cách nhanh nhất 

```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int max_side = min(n, m);
        vector<vector<int>> dp(m, vector<int>(n, 0));

        int cnt = 0;
        for (int i = 0; i<max_side; i++){
            for (int j = i; j<m;j++){
                for(int k = i; k<n;k++){
                    if (i>0){
                        dp[j][k] = matrix[j-1][k-1] && matrix[j-1][k] && matrix[j][k-1] && matrix[j][k];
                        cnt += dp[j][k];
                    }
                    else{
                        cnt += matrix[j][k];
                        dp[j][k] = matrix[j][k];
                    }
                }
            }
            matrix = dp;
        }
        return cnt;
    }
};
```


### Number of good ways to split a string

<a href="https://leetcode.com/problems/number-of-good-ways-to-split-a-string/" target="_blank">Problem</a>

note: để tối ưu cần sử dụng hash table.

```cpp
class Solution {
public:
    int numSplits(string s) {
        vector<char> left;
        vector<int> left_count;
        vector<char> right;
        vector<int> right_count;
        
        int check; 
        for (int i = 0; i<s.length(); i++){
            check = 0;
            for(int j = 0; j<right.size(); j++){
                if (right[j] == s[i]){
                    right_count[j]+=1;
                    check = 1;
                    break;
                }
            }
            if (!check) {
                right.push_back(s[i]);
                right_count.push_back(1);
            }
        }

        int cnt = 0;
        for (int i = 0; i<s.length()-1; i++){
            check = 0;
            for(int j = 0; j<left.size();j++){
                if (left[j] == s[i]){
                    left_count[j]+=1;
                    check = 1;
                    break;
                }
            }
            if (!check){
                left.push_back(s[i]);
                left_count.push_back(1);
            }
            
            for(int j = 0; j< right.size();j++){
                if (right[j] == s[i]){
                    right_count[j] -=1;
                    if (right_count[j] == 0){
                        right_count.erase(right_count.begin()+j);
                        right.erase(right.begin()+j);
                    }
                }
            }
            if (right.size() == left.size()) cnt+=1;
        }
        return cnt;
    }
};

```

