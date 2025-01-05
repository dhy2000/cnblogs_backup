# accoding-3956 简易计算器 题解

2022-03-06

## 解题思路

对于输入，多组输入数据，每组分别输入两个数和一个运算符。对于输入一行以空格隔开的两个数，可以采用 python 的内置函数 `map`，将输入的一行内容用空格隔开得到一个含有2项字符串的列表，通过 `map` 对每一项调用 `float` 函数转化为数值（注意这里不是整数）。
题目要求实现一个小计算器，在 python 中直接采用 `eval` 或者 `exec` 函数即可较方便地计算字符串中表达式的值，所以直接将两个数和运算符拼成字符串即可，无需像 C 语言一样采用 `if` 或 `switch` 判断运算符。
最终的保留小数采用 `round` 函数，然后输出结果即可。

## AC代码

```python
# This is a possible version.
T = int(input().strip())

for _T in range(T):
    a, b = map(float, input().strip().split(' '))
    op = input().strip()[0]
    result = eval("{a}{op}{b}".format(a=a,b=b,op=op))
    answer = round(result, 3)
    print(answer)
```

## 坑点分析

此题的坑点之一在于如何按照题目要求"至多保留3位小数点"并正确输出结果。（本人有多次结果错误的提交是错在了此坑点。当然，输出的要求也许对于其他人来说是很自然的操作）

本人与参加了上机的同学讨论后得知，此题采用 `print` 输出一个 `float` 类型的浮点数（采用 `round` 保留3位小数）是可以得到正确结果的。经过大量的<del>炸评测机</del>提交测试，得出了一种行为**可能**正确的保留小数的方法(一些具体的结果可能仍有区别)：

1. 首先以 `round` 保留到小数点后3位，而后去掉小数部分的后导零。
   
        注：采用 "%.3f" 来保留三位小数也是可以的。这两种近似方式的行为可能是不同的，但实测在本 OJ 上都能通过。采用 round 最保险。
        例：1.1234 -> 1.123, 3.4215 -> 3.422, 6.1825 -> 6.182, 3.7999 -> 3.8
        这一点是本人根据样例猜的，并通过提交验证了此猜测。在不同的系统或不同版本的 python 环境下可能保留结果不同，不一定是严格的四舍五入或者四舍六入五凑偶，也可能与浮点误差有关。

2. 如果近似后的小数部分全部为零，则输出结果应以 `".0"`(不包含引号) 作为小数部分，而不要输出一个整数。即：小数部分至少有 1 位。
   
        例：3.9998 -> 4.0, 1.0001 -> 1.0
        这一点是本人根据第一条样例猜的，同样经提交后才验证猜测正确。

3. 测试数据中的所有输出结果不存在科学计数法。

最稳妥的方式是采用 `round` 保留 3 位小数后直接 `print` 输出。其他可行的实现方式还有利用 `"%.3f"` 保留小数，再将保留过的字符串转化回浮点类型以去掉后导零和保证小数部分全零时输出 `.0` ，或者利用 `"%.3f"` 截取小数位数后采用字符串的方式处理小数部分（这里给出一个利用正则表达式贪婪匹配来处理的例子，也可以手写循环）

以下的代码给出了四种实现方式，其中第三种方式是错误的，错误原因出现在 `"%g"` 上，`"%g"`会根据数值的范围自动选择 `"%e"` 与 `"%f"` 中长度较短者且不输出后导零。而 `print` 在浮点数的有效数字过多时虽也会输出科学计数法，但二者的实现有区别，因此采用 `"%g"` 会得到错误的答案。因此本题不要轻易用 `"%g"`。

关于 `"%.3f"` 格式化输出与 `round` 结果是否不同以及答案中是否有科学计数法，本人在以下的测试代码中引入了对相应条件的判断，若满足某个条件则引入死循环从而使评测结果为 TLE，通过观察评测结果得知测试数据的特点。结论为测试数据采用两种保留小数的方式答案相同且结果均正确，测试数据的输出不含有科学计数法。

### 本部分小结

1. 推荐用 `round` ，其次是 `"%.3f"` 。
2. 不要用 `"%g"` 。

## 测试代码

```python
def solve1(raw: float): # correct
    return str(round(raw, 3))

def solve2(raw: float): # correct
    f3 = "%.3f" % raw # truncate the decimal part to 3 digits
    return str(float(f3))

def solve3(raw: float): # wrong answer
    f3 = "%.3f" % raw
    ans = "%g" % float(f3) # scientific notation may occur in "%g"
    if ans.find('.') == -1:
        ans += ".0"
    return str(ans)

def solve4(raw: float): # correct
    import re
    f3 = "%.3f" % raw
    real = f3[-3:]
    tmp = re.match(r"[0-9]*[1-9]", real)
    if tmp == None:
        real = "0"
    else:
        real = tmp.group(0)
    return f3[:-3] + real

if __name__ == '__main__':
    T = int(input().strip())
    flg = False
    for _T in range(T):
        a, b = map(float, input().strip().split(' '))
        op = input().strip()[0]
        res = eval("{a}{op}{b}".format(a=a,b=b,op=op))
        out = solve1(res)
        print(out)
    #     if 'e' in str(out): # detect scientific notation
    #         flg = True
        if solve1(res) != solve4(res): # detect the different behavior between round and %.3f
            flg = True

    if flg:
        while True:
            print('!!!') # make TLE
```

## 题目改进建议

为了提高题目质量以及优化同学们做题的体验，就本题而言提出一点拙见：由于浮点数在不同架构的计算机系统上的处理可能是有差异的，所以对于答案为浮点数的问题推荐采用 Special Judge 而不是传统的文本比较作为评测方式。如果出题人希望考察学生精确地保留到某位小数，则可以对保留小数的格式要求进行更详尽的说明(且不要产生歧义)以及多提供一些样例。当然，直接给出建议的实现方式（例如用 `round` 然后直接输出）也未尝不可，至少在评测机环境下能够保证标程和学生的程序行为是一致的。

另外，作为一道完整而严谨的编程题目，对于题目中涉及到的变量可以给出数据范围。
