## 剑指 Offer 10- I. 斐波那契数列
https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1

F(N) = F(N - 1) + F(N - 2), 其中 N > 1.

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入：n = 2
输出：1

示例 2：

输入：n = 5
输出：5

### 递归
```cpp
class Solution {
public:
    int fib(int n) {
        if(n == 0) return 0;
        if(n == 1 || n == 2) return 1;
        return (fib(n - 1) + fib(n - 2)) % 1000000007;
    }
};
```

### 递推 O(n)
```cpp
class Solution {
public:
    int fib(int n) {
        if(n == 0) return 0;
        if(n == 1 || n == 2) return 1;
        int a = 1, b = 1, c;
        for(int i = 2; i < n; i++){
            c = (a + b) % 1000000007;
            a = b;
            b = c;
        }
        return c;
    }
};
```

### 矩阵快速幂 O(logn)
```cpp
class Matrix2by2 {
public:
	long long m_00, m_01, m_10, m_11;
	Matrix2by2(long long m1, long long m2, long long m3, long long m4)
		:m_00(m1), m_01(m2), m_10(m3), m_11(m4) {}
	void print() {
		cout << m_00 << " " << m_01 << endl;
		cout << m_10 << " " << m_11 << endl;
	}
};

const int MOD = 1000000007;

//矩阵相乘(2x2)
Matrix2by2 MatrixMultiply(const Matrix2by2& M1, const Matrix2by2& M2) {
	long long m00 = ((M1.m_00 * M2.m_00) % MOD + (M1.m_01 * M2.m_10) % MOD) % MOD;
	long long m01 = ((M1.m_00 * M2.m_01) % MOD + (M1.m_01 * M2.m_11) % MOD) % MOD;
	long long m10 = ((M1.m_10 * M2.m_00) % MOD + (M1.m_11 * M2.m_10) % MOD) % MOD;
	long long m11 = ((M1.m_10 * M2.m_01) % MOD + (M1.m_11 * M2.m_11) % MOD) % MOD;
	return Matrix2by2(m00, m01, m10, m11);
}

//矩阵的快速幂运算(矩阵的n次方)
Matrix2by2 MatrixPower(Matrix2by2 M, int n) {
	if (n == 0) {
		return Matrix2by2(1, 0, 0, 1);
	}
	int ex = 1;
	Matrix2by2 matrix = M;
	while ((ex << 1) <= n) {
		matrix = MatrixMultiply(matrix, matrix);
		ex <<= 1;
	}
	return MatrixMultiply(matrix, MatrixPower(M, n - ex));
}

//计算斐波那契数列的第n项
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        if (n == 1 || n == 2) return 1;
        Matrix2by2 fibMatrix = MatrixPower(Matrix2by2(0, 1, 1, 1), n - 1);
        return fibMatrix.m_11;
    }
};
```
```
