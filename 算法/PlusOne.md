

如果追求算法的效率，就要考虑情况的特殊性。

本题的特殊性在于只加一

所以

- 进位只存在某位为9的情况
- 某位不为9会终止进位传递
- 首位进位只有 99999 情况

针对这些特殊情况，可以如下优化：

- 判断简单 `if (digits[i] == 9)`
- 某位不为9 立刻 `return`
- 进位构造新数组不用拷贝