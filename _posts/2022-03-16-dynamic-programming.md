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

```Cpp
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

```Cpp
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

![/assets/images/Tabulation-vs-Memoization-1.png]