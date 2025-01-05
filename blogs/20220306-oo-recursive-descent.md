# OO-表达式解析之递归下降法

2022-03-06

搬运自 2021 年（去年）面向对象研讨课本人的分享，本帖内容由去年的研讨课 PPT 修订而成，并补充了一些新内容。

在今年的第一单元训练指导书中，课程组已经官方给出了递归下降法的教程，但该教程篇幅较短且没有给出代码。本帖可作为课程组官方指导书的补充，可供同学们参考。

以下为正文。

------

## 什么是递归下降

在本单元 OO 作业中所涉及的表达式，是由一系列 EBNF 描述的形式化表述来定义的语法规则推导而成的语句。语法规则的每一条称为一个产生式，位于产生式左侧的符号称为非终结符（例如 `表达式`，`项` 等），与之相对的，语法规则中没有出现在产生式左侧的符号称为终结符（例如 `x`, `+`, `1` 等）。

[递归下降](https://zh.wikipedia.org/wiki/%E9%80%92%E5%BD%92%E4%B8%8B%E9%99%8D%E8%A7%A3%E6%9E%90%E5%99%A8)是编译原理中的一种自顶向下的语法分析方法，其做法是为每一个非终结符（即语法规则中每个位于产生式左侧的符号）编写一个可递归调用的分析子程序(方法)，通过方法之间的相互调用来处理嵌套的语法层次关系。

有了形式化表述的语法规则，我们便可以利用递归下降法来编写解析程序，并且解析程序的结构与形式化表述中定义的语法规则是基本一致的，这使得表达式解析部分的代码架构很清晰，并且十分易于扩展（当后续作业新增或修改了语法规则，只需为新增的语法规则编写相应的解析方法，找到要修改的语法规则的解析方法并修改其内部的解析逻辑即可）

## 再做《表达式求值》

在大一的数据结构课程中，我们已经学习并实践过多次《表达式求值》这一编程题目，在当时普遍采用的是双栈的方法，维护一个操作数栈和一个运算符栈。这种方法存在一定的局限性，当表达式的语法规则愈发复杂（例如引入乘方，自定义函数，一元运算符等），解析程序的复杂程度也随之增大，且可读性由于判断的增多而有所下降。（这种方法在 OO 的作业中仍然是可行的，但是随着第二次第三次作业的迭代，这种方法的可维护性可能会变差）

在此，让我们用递归下降法回顾表达式求值，通过比较简单的语法来进行引入。

考虑如下版本的表达式求值:

- 只涉及整数
- 支持 `+`, `-`, `*`, `/` 运算（优先级同运算符的数学含义）
- 允许括号以及嵌套括号
- 整数和括号前均可以带 1 个符号

（除法同 C 语言中的整数除法，保证运算过程中不出现除以零）

### 问题解决步骤

1. 明确语法
  - 分析表达式的语法成分及其关系
  - 给出以 BNF 或 EBNF 描述的形式化定义
  （在本次 OO 作业中，形式化定义已经由指导书给出，无需做该步骤）
2. 为语法规则的每一个非终结符编写解析函数
  - 一个函数只解析自己负责的一个非终结符（或者说，一条产生式）
  - 遇到下一级子成分时，调用相应成分的解析函数
  - 定义函数的返回值
    - 本题要求求值, 也就是解析完每个非终结符都对应一个数值，所以每个函数的返回值都是一个 `int`
    - 如果需要构建表达式树，则每个解析函数都返回一个指向表达式树节点的指针（Java 中就是对象变量）
  - 语法错误 (Wrong Format) 的判定
    - 语法分析无法继续进行（在当前位置遇到的符号不满足语法规则）
    - 语句解析应当结束而未结束

### 描述语法规则

采用 EBNF 描述上述表达式的语法规则（以下产生式中用 `->` 表示非终结符的定义）: 

    Expr    -> Term     | Expr ('+'|'-') Term
    Term    -> Factor   | Term ('*'|'/') Factor
    Factor  -> ['+'|'-'] ( Number | '(' Expr ')' )
    Number  -> Digit { Digit }
    Digit   -> '0'|'1'|'2'|'3'|...|'9'

其中

- `{}`: 重复, 允许存在 0 个或多个
- `[]`: 可选, 允许存在 0 个或 1 个
- `()`: 分组, 用来控制产生式的优先级
- `|`: 或，从多个选择中选择一个
- 单引号包裹的字符(串)为终结符

描述同一种语言的形式化表述语法规则可以不唯一（如果题目没有出），例如:

- 表达式 `Expr` 与项 `Term` 的产生式除了左递归形式以也可改写成循环形式
  - `Expr -> Term { ('+'|'-') Term }`
- 因子 `Factor` 和其左部的符号可以拆分
  - `Factor -> Sign UnsignedFactor`
  - `Sign -> '+' | '-'`
  - `UnsignedFactor -> Number | '(' Expr ')'`
- 无符号整数 `Number` 也可以直接当作一整个终结符
  - 需要通过[词法分析](https://zh.wikipedia.org/wiki/%E8%AF%8D%E6%B3%95%E5%88%86%E6%9E%90)达到

### 消除左递归

有了形式化表述的语法规则，就可以马上开始编写递归下降解析器了吗？此时还是不行的。在上面这个例子中，表达式 `Expr` 和项 `Term` 的产生式存在**左递归**。左递归就是指产生式形如 `A -> A b` 的形式，例如 `Expr -> Expr '+' Term`。

如果语法规则中存在左递归，是不能直接采用递归下降法来分析的，因为这样的解析程序会产生无限递归（递归下降解析通常是贪心匹配的，而如果对于上述左递归的 `Expr` 而言，解析器无法确定产生式右部的 `Expr` 的右边界，也就无法确定递归什么时候停止）。

为了能够采用递归下降法进行解析，我们就需要对语法规则进行**改写**: 将含有左递归的产生式改写成右递归或者循环形式（利用 `{}` 符号描述），从而消除左递归。

例如, 对 `Expr` 的产生式，有如下两种改写方法:

- `Expr -> Term | Term ('+' | '-') Expr`
- `Expr -> Term { ('+' | '-') Term }`

这两种改写方法都是正确的，相比之下更加推荐采用循环的形式。如果采用右递归的形式，则相应的递归下降解析程序中就会产生大量的压栈行为(函数递归调用)，如果表达式较长则有栈溢出的危险。（例如 `x+x+x+x+...+x`，其中 `x` 的个数在使得表达式长度不超过上限的前提下尽可能多）

这里小小地延伸一下，为什么指导书上给出的语法规则用左递归来描述 `表达式` 和 `项`？

根据个人理解，根据左递归语法规则推导出的语法树，其层次与真实的运算顺序是一致的。在数学意义下，`+`, `*` 等二元运算符是从左到右结合的，位于左部的运算符优先被运算，相应的语法层次也就对应了左递归形式。

### 编写解析程序

这里给出 C 语言实现上述表达式求值的代码:

```c
#include <stdio.h>
#include <stdlib.h>

/*
  Expr    -> Term     | Expr ('+'|'-') Term
  Term    -> Factor   | Term ('*'|'/') Factor
  Factor  -> ['+'|'-'] ( Number | '(' Expr ')' )
  Number  -> unsigned number
*/

// 这些函数之间相互调用, 在 C 语言中需要预先声明
// 表达式求值, 每个解析子程序都返回一个数值
int Expr();
int Term();
int Factor();
int Number();


#define MAX_LEN 100 // 表达式最大长度

char buffer[MAX_LEN + 5]; // 输入缓冲区
int pos; // 正在解析的位置下标

#define chr (buffer[pos]) // 取出当前字符
#define nxt (pos++) // 向后移动1个字符

#define WF do{puts("WRONG FORMAT!"); exit(1);}while(0)

// Expr -> Term | Expr ('+'|'-') Term
int Expr() {
    int ans = Term();
    while (chr == '+' || chr == '-') {
        if (chr == '+') {
            nxt;
            ans += Term();
        } else {
            nxt;
            ans -= Term();
        }
    }
    return ans;
}

// Term -> Factor | Term ('*'|'/') Factor
int Term() {
    int ans = Factor();
    while (chr == '*' || chr == '/') {
        if (chr == '*') {
            nxt;
            ans *= Factor();
        } else {
            nxt;
            ans /= Factor();
        }
    }
    return ans;
}

// Factor -> ['+'|'-'] ( Number | '(' Expr ')' )
int Factor() {
    int sign = 1; // positive
    // ['+' | '-']
    if (chr == '+') nxt;
    else if (chr == '-') {
        nxt;
        sign = -1; // negative
    }
    // Number | '(' Expr ')'
    // 语法规则存在"或"关系 => 向前预读 1 个符号, 判断接下来解析的方向是单个数字 Number 还是括号 '(' Expr ')'
    if (chr == '(') {
        nxt;
        int inner = Expr();
        if (chr == ')') nxt; // expect ')' here
        else WF;
        return sign * inner;
    } else if (chr >= '0' && chr <= '9') {
        return sign * Number();
    } else WF; // either '(' or Number
}

int Number() {
    int ans = 0;
    while (chr >= '0' && chr <= '9') {
        ans = (ans << 3) + (ans << 1) + (chr - '0');
        nxt;
    }
    return ans;
}

int main() {
    while (scanf("%s", buffer) != EOF) {
        pos = 0;
        int ans = Expr();
        if (chr != 0) WF; // not reach end
        printf("%d\n", ans);
    }
    return 0;
}
```

以上代码可以通过[accoding-303 表达式求值](https://accoding.buaa.edu.cn/problem/303/index)的评测。

采用 Java 实现递归下降法，程序结构和上述 C 语言代码是相近的。

### 复杂度分析

时间复杂度: $O(n)$

- 被解析的字符串从左到右遍历一遍，无回溯情况下为线性时间复杂度
- 语法规则中"或"关系的分支数量影响常数，分支数越多常数越大。

空间复杂度: $O(n)$

- 对应递归过程中栈的深度，也是语法树的高度
- 将右递归改成循环可优化空间复杂度

## OO 表达式作业

通过相对简单的《表达式求值》理解了递归下降的思想，我们就可以回到 OO 的表达式作业了。

### 语法规则-准备

由于作业中给出了表达式语法规则的形式化表述，因此不需要过多理解表达式的语法，只需根据规则照做。

在开始递归下降之前，我们需要检查语法规则中是否存在左递归的产生式，如果存在（例如 `表达式` 和 `项`），我们需要将其改写成循环形式。

除此以外，也可以根据个人习惯进行一些不影响语法正确性的改写，例如作业中定义的"空白项"可以当成一整个符号(终结符)，从而不再需要"空白字符"。（对于空白字符，其 parse 方法不需要返回任何内容，作用仅仅是跳过这些空白字符，返回值类型设定为 `void` 即可）

### 解析器的编写

如果需要解析输入的字符串得到表达式树，递归下降解析器就相当于工厂模式中的工厂，其输入是一个字符串，构建出来的产品就是表达式树对象。

每个非终结符 XXXX 都对应一个 `parseXXXX` 方法，该方法返回解析该语法成分得到的结果。

- 语法树节点，或者计算结果的数值等，根据实际情况确定。
- 对于空白项，直接跳过，不需要返回值

对于存在"或"关系的语法成分产生式，在分析时需要确定下一步分析的方向（也就是从若干个分支中选择一个分支进行），为了避免低效率的回溯，我们需要采用"预读"的方式，提前读取下一个字符来判断下一步的走向（在上面 C 语言代码中，解析 `Factor` 时就需要预读）。由于 Java 中的迭代器 `Iterator` 只能用 `next` 方法单向前进（甚至不能像 C++ 的 `*it` 一样停在原地读出当前值），因此在编写递归下降解析器时不推荐使用 Java 的迭代器。最简单地，维护一个整数下标，指向当前解析到的位置即可。


在这里给出一个 Java 递归下降**建立表达式树**的代码模板:

```java
/* 表达式树节点类定义 */

// 抽象节点基类
public abstract class TreeNode {
    // ......
}

// 叶子节点(数字)
public class NumberNode extends TreeNode {
    // ......
    public NumberNode(int number) { /* ... */ }
}

// 加法节点
public class AddNode extends TreeNode {
    // ......
    public AddNode(TreeNode left, TreeNode right) { /* ... */ }
}

// 减法 SubNode, 乘法 MulNode, 除法 DivNode 的定义略

/* 递归下降解析器 */
public class ExprBuilder {
    private final String expr; // 待解析的表达式
    private int pos; // 解析到的下标

    public ExprBuilder(String expr) {
        this.expr = expr;
        this.pos = 0;
    }

    // parseXXXX 表示解析 XXXX 语法成分的方法
    // 只有 Expr 是语法的顶层符号，因此该方法为 public
    // 抛出异常表示待解析的表达式格式错误
    public TreeNode parseExpr() throws Exception {
        // 用 expr.charAt(pos) 取出当前位置的字符
        // ......
    }

    private TreeNode parseTerm() throws Exception { /* ... */ }

    private TreeNode parseFactor() throws Exception { /* ... */ }

    private TreeNode parseNumber() throws Exception { /* ... */ }
}
```

可以看到，采用递归下降法编写的解析程序是相当简洁的，架构清晰，可读性良好且高度易于扩展。

## 递归下降法的优势

1. 正确性
  
   每个非终结符的解析函数(方法)都严格遵循产生式的定义，只需读代码+简单的测试即可保证解析器的正确性。

2. 可读性与可扩展性

   三次作业的迭代开发，对于和上次作业中推导规则相同的非终结符（例如表达式和项）无需修改；只需修改语法有改变或者新增的非终结符的解析。
   
   从此告别冗长难读的"阴间"大正则。（如需词法分析或匹配函数名/数字/变量，只需少量非常简单的正则表达式，难度绝不会超过 Pre 作业中的正则表达式）
  
3. 格式检查

   当解析过程中读出的下一个字符无法满足当前产生式的任何一个方向，导致语法分析无法继续进行，则抛出异常表示 WRONG FORMAT。内层方法抛出的异常会逐级抛到顶层最终向外抛出。

最后，祝同学们都能顺利完成第一单元的 OO 之旅！

## 附录

在此附上两道采用了递归下降法实现的编程题参考代码。

CCF-CSP 2019年12月第3题 化学方程式

```cpp
/* CCF CSP 201912-3 Chemistry Equation */

/* 
Recursive descend
BNF: 
<equation> ::= <expr> "=" <expr> // 2H2+O2=2H2O
<expr> ::= <coef> <formula> | <coef> <formula> "+" <expr> // 2H2+O2, 2H2O
<coef> ::= <digits> | ""    // 
<digits> ::= <digit> | <digits> <digit>
<digit> ::= "0" | "1" | ... | "9"
<formula> ::= <term> <coef> | <term> <coef> <formula>   // H2O CO2 Ca(OH)2 Ba3(PO4)2
<term> ::= <element> | "(" <formula> ")" // H, Ca, (OH), (PO4)
<element> ::= <uppercase> | <uppercase> <lowercase> // H, Ca, O, P, C
<uppercase> ::= "A" | "B" | ... | "Z"
<lowercase> ::= "a" | "b" | ... | "z"

11
H2+O2=H2O
2H2+O2=2H2O
H2+Cl2=2NaCl
H2+Cl2=2HCl
CH4+2O2=CO2+2H2O
CaCl2+2AgNO3=Ca(NO3)2+2AgCl
3Ba(OH)2+2H3PO4=6H2O+Ba3(PO4)2
3Ba(OH)2+2H3PO4=Ba3(PO4)2+6H2O
4Zn+10HNO3=4Zn(NO3)2+NH4NO3+3H2O
4Au+8NaCN+2H2O+O2=4Na(Au(CN)2)+4NaOH
Cu+As=Cs+Au
*/

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <string>
#include <map>
#include <cassert>
using namespace std;
typedef long long ll;

#define MAXN 1000
char input[MAXN + 5];
int offset;

#define RST (offset = 0)
#define CHR (input[offset])
#define NXT (offset++)

struct ElementSet {
    map<string, int> content;
    ElementSet() {}
    ElementSet(string element, int coef) {content[element] = coef;}
    void merge(const ElementSet &s) {
        map<string, int>::const_iterator it1 = s.content.begin(), ie1 = s.content.end();
        while (it1 != ie1) {
            if (this->content.count(it1->first)) {
                this->content[it1->first] += it1->second;
            }
            else {
                this->content[it1->first] = it1->second;
            }
            it1++;
        }
    }
    void times(const int coef) {
        map<string, int>::iterator it1 = this->content.begin(), ie1 = this->content.end();
        while (it1 != ie1) {
            this->content[it1->first] *= coef;
            it1++;
        }
    }
    bool equals(const ElementSet &b) {
        map<string, int>::const_iterator it1 = this->content.begin(), ie1 = this->content.end();
        map<string, int>::const_iterator it2 = b.content.begin(), ie2 = b.content.end();
        while (it1 != ie1 && it2 != ie2) {
            if (it1->first != it2->first) return false;
            if (it1->second != it2->second) return false;
            it1++; it2++;
        }
        return (it1 == ie1) && (it2 == ie2);
    }
};

bool Equation();
void Expr(ElementSet &es); 
int Coef();
// Digits
// Digit
void Formula(ElementSet &es);
void Term(ElementSet &es);
string Element();
// uppercase
// lowercase

bool Equation() {
    ElementSet left, right;
    Expr(left);
    assert(CHR == '=');
    NXT;
    Expr(right);
    return left.equals(right);
}

void Expr(ElementSet &es) {
    int coef = Coef();
    ElementSet formula;
    Formula(formula);
    formula.times(coef);
    es.merge(formula);
    while (CHR == '+') {
        NXT;
        formula.content.clear();
        coef = Coef();
        Formula(formula);
        formula.times(coef);
        es.merge(formula);
    }
}

int Coef() {
    if (CHR >= '0' && CHR <= '9') {
        int ret = 0;
        while (CHR >= '0' && CHR <= '9') {
            ret = (ret << 3) + (ret << 1) + (CHR - '0');
            NXT;
        }
        return ret;
    }
    else return 1;
}

void Formula(ElementSet &es) {
    ElementSet term;
    Term(term);
    int coef = Coef();
    term.times(coef);
    es.merge(term);
    while (CHR && CHR != '+' && CHR != ')' && CHR != '=') { // !!!
        term.content.clear();
        Term(term);
        coef = Coef();
        term.times(coef);
        es.merge(term);
    }
}

void Term(ElementSet &es) {
    if (CHR >= 'A' && CHR <= 'Z') {
        string ele = Element();
        es.merge(ElementSet(ele, 1));
    }
    else if (CHR == '(') {
        NXT; // '('
        ElementSet formula;
        Formula(formula);
        NXT; // ')'
        es.merge(formula);
    }
    else assert(0);
}

string Element() {
    assert(CHR >= 'A' && CHR <= 'Z');
    char ele[3] = {CHR, 0, 0};
    NXT;
    if (CHR >= 'a' && CHR <= 'z') {ele[1] = CHR; NXT;}
    return string(ele);
}


int main()
{
    // freopen("test.in", "r", stdin);
    int n; scanf("%d", &n);
    while (n--) {
        scanf("%s", input);
        RST;
        printf("%c\n", Equation() ? 'Y' : 'N');
    }
    return 0;
}
```

[accoding-4395 追寻表达式中的真理](https://accoding.buaa.edu.cn/problem/4395/index)

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/********** Formula ***********
 * <EXPR> ::= <TERM> {"+"|"-" <TERM>}
 * <TERM> ::= <FACTOR> {"*"|"/" <FACTOR>}
 * <FACTOR> ::= (<NAME> [<FUNC> | <METHOD> {<METHOD>}]) | ("(" <EXPR> ")" {<METHOD>})
 * <FUNC> ::= "(" <EXPR> {"," <EXPR>} ")"
 * <METHOD> ::= "." <NAME> <FUNC>
 * <NAME> ::= [IDENTIFIER]
 *******************************/ 

#define WF puts("WRONG FORMAT");

#define MAX_EXPR_LENGTH 125
char buf[MAX_EXPR_LENGTH];
char *p_str = buf;

static int count = 0;
#define SAVE (++count)

typedef struct Val {
    unsigned char type; // 0: variable, 1: temporary result
    union {
        char var;
        int res;
    } val;
} Val;
void disp(const Val *v) {
    if (v->type == 0) {
        putchar(v->val.var);
    } else {
        printf("%d", v->val.res);
    }
}
Val ValVar(char var) {
    Val r;
    r.type = 0;
    r.val.var = var;
    return r;
}
Val ValRes(int res) {
    Val r;
    r.type = 1;
    r.val.res = res;
    return r;
}


Val Expr();
Val Term();
Val Factor();
Val Func(char name, const Val* self);
Val Method(const Val *self);
Val Name();

int main()
{
    gets(buf);
    Expr();
    return 0;
}


Val Expr(){
    if (!(*p_str)) WF;

    Val term = Term();
    Val res = term;
    while ((*p_str) && ((*p_str) == '+' || (*p_str) == '-')) {
        char op = *(p_str++);
        term = Term();
        printf("%c ", op);
        disp(&res); putchar(' ');
        disp(&term);
        res = ValRes(SAVE);
        putchar('\n');
    }
    
    return res;
}

Val Term(){
    if (!(*p_str)) WF;

    Val factor = Factor();
    Val res = factor;
    while ((*p_str) && ((*p_str) == '*' || (*p_str) == '/')) {
        char op = *(p_str++);
        factor = Factor();
        printf("%c ", op);
        disp(&res); putchar(' ');
        disp(&factor);
        res = ValRes(SAVE);
        putchar('\n');
    }
    
    return res;
}

Val Factor(){
    if (!(*p_str)) WF;
    Val ret;
    if ((*p_str) == '(') {
        p_str++;
        Val expr = Expr();
        if (!(*p_str) || (*p_str) != ')') WF;
        p_str++; // ')'
        ret = expr;
    } else {
        Val name = Name();
        if (*p_str && (*p_str) == '(') {ret = Func(name.val.var, NULL);}
        else ret = name;
    }

    while ((*p_str) && (*p_str) == '.') {
        ret = Method(&ret);
    }
    return ret;
}

Val Func(char name, const Val* self){
    // self has already calculated
    Val *args = malloc(121 * sizeof(Val)); 
    int argc = 0;
    if (!(*p_str) || (*p_str) != '(') WF;
    p_str++; // '('
    
    if (self != NULL) {
        args[argc++] = *self;
    }
    Val expr = Expr();
    args[argc++] = expr;
    while (*p_str && (*p_str) == ',') {
        p_str++;
        expr = Expr();
        args[argc++] = expr;
    }
    if (!(*p_str) || (*p_str) != ')') WF;
    p_str++;
    printf("%c", name);
    for (int i = 0; i < argc; i++) {
        putchar(' ');
        disp(&args[i]);
    }
    putchar('\n');
    free(args);
    return ValRes(SAVE);
}

Val Method(const Val *self){
    if (!(*p_str) || (*p_str) != '.') WF;
    p_str++;
    Val name = Name();
    return Func(name.val.var, self);
}
Val Name(){
    if (!(*p_str) || (*p_str) < 'a' || (*p_str) > 'z') WF;
    char name = *(p_str++);
    return ValVar(name);
}
```