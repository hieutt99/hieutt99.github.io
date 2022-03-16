---
title:  "Dynamic Programming"
categories: "Computer Science"
toc: true
tags:
	- Algorithms
---



Dynamic Programming hay quy hoạch động hay DP là phương pháp nhằm giảm thời gian chạy trong các bài toán có tính chất ***overlapping subproblem*** và ***optimal substructure***. DP được chia làm 2 hướng tiếp cận chính là memoization hay top-down và tabulation hay bottom-up. 

# Memoization 
Ý tưởng của memoization là lưu lại các kết quả tính toán từ các function calls và trả cached results để tối ưu việc tính toán khi gặp các kết quả với inputs tương tự. 
Để dễ hiểu hơn, chúng ta có thể xét bài toán Fibonacci. Dãy Fibonacci là dãy vô hạn các số tự nhiên được bắt đầu với hai phần tử 0 và 1, các phần tử sau đó được thiết lập theo quy tắc phần tử sau bằng tổng hai phần tử đứng trước nó. Ta có công thức truy hồi của dãy Fibonacci như sau: 

$
F(n):=
	\begin{cases}
		1 & \text{khi n=1;}\\
		1& \text{khi n=2;}\\
		F(n-1)+F(n-2)& \text{khi n>2.}
	\end{cases}
$


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
### Full Binary Trees
https://leetcode.com/problems/all-possible-full-binary-trees/submissions/
note : các trường hợp n chẵn ko thể tạo được cây nhị phân hoàn chỉnh 
các trường hợp n lẻ thì tại state n là chỉnh hợp của 2 state trước đó có tổng số node cộng 1 bằng với số node của state n


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
https://leetcode.com/problems/count-substrings-that-differ-by-one-character/

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
https://leetcode.com/problems/count-sorted-vowel-strings/submissions/

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
https://leetcode.com/problems/count-square-submatrices-with-all-ones/submissions/

note: chưa phải cách nhanh nhất 

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
https://leetcode.com/problems/number-of-good-ways-to-split-a-string/submissions/
note: không chắc là có tối ưu được không nữa 

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

