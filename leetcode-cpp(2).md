# leetcode cpp

## 目录

[TOC]

<div style="page-break-after: always;"></div>

## 数据结构

### STL

#### Sequence Containers

> 维持顺序的容器

- `vector`：动态数组
- `list`：双向链表
- `deque`：双端队列
- `array`：固定大小的数组
- `forward_list`：单向链表

#### Container Adaptors

> 基于其它容器实现的数据结构

- `stack`：栈：后入先出（LIFO）的数据结构
- `queue`：队列：先入先出（FIFO）的数据结构
- `priority_queue`：最大值先出的数据结构

#### Associative Containers

> 排好序的数据结构

- `set`：有序集合，不可重复
- `multiset`：可重复的 set
- `map`：有序表，在 set 的基础上加上映射关系，可以对每个元素 key 存一个
  值 value。
- `multimap`：可重复的 map

#### Unordered Associative Containers

> 对每个 Associative Containers 实现了哈希版本，即无序版本

- `unordered_set`：哈希集合
- `unordered_multiset`：可重复的哈希集合
- `unordered_map`：哈希表
- `unordered_multimap`：可重复的哈希表

关于 STL 的详细内容请参考：[C++ STL总结](https://wyqz.top/p/870124582.html) or https://zh.cppreference.com/

### 数组

#### [找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

**题目：**

给你一个含 `n` 个整数的数组 `nums` ，其中 `nums[i]` 在区间 `[1, n]` 内。请你找出所有在 `[1, n]` 范围内但没有出现在 `nums` 中的数字，并以数组的形式返回结果。

> 输入：nums = [4,3,2,7,8,2,3,1]
> 输出：[5,6]

**题解：**

利用数组这种数据结构建立 n 个桶，把所有重复出现的位置进行标记，然后再遍历一遍数组，即可找到没有出现过的数字。

进一步地，我们可以直接对原数组进行标记：把出现的数字对应的位置设为负数，最后仍然为正数的位置即为没有出现过的数。

```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    for (auto num : nums) {
        int pos = abs(num) - 1;
        if (nums[pos] > 0) 
            nums[pos] = -nums[pos];
    }
    vector<int> ans;
    for (int i = 0; i < nums.size(); ++i) {
        if (nums[i] > 0)
            ans.push_back(i+1);
    }
    return ans;
}
```

#### [旋转图像](https://leetcode.cn/problems/rotate-image/)

**题目：**

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[[7,4,1],[8,5,2],[9,6,3]]

**题解：**

方法一：

旋转的关键等式为：
$$
matrix[col][n-row-1]=matrix[row][col]
$$
为了防止原 $matrix[col][n-row-1]$ 被覆盖，需要把它先旋转过去，带入公式得：
$$
matrix[n-row-1][n-col-1]=matrix[col][n-row-1]
$$
为了防止 $matrix[n-row-1][n-col-1]$ 被覆盖，需要：
$$
matrix[n-col-1][row]=matrix[n-row-1][n-col-1]
$$
为了防止 $matrix[n-col-1][row]$ 被覆盖，需要：
$$
matrix[row][col]=matrix[n-col-1][row]
$$
又回到了起点，所以一次旋转四个即可，遍历矩阵左四分之一部分即可，代码如下：

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i = 0; i < (n+1)/2; ++i) {
        for (int j = 0; j < n/2; ++j) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[n-j-1][i];
            matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
            matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
            matrix[j][n-i-1] = temp;
        }
    }
}
```

方法二：

两次翻转实现旋转，代码如下：

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    // 水平翻转
    for (int i = 0; i < n / 2; ++i) {
        for (int j = 0; j < n; ++j) {
            swap(matrix[i][j], matrix[n - i - 1][j]);
        }
    }
    // 主对角线翻转
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
}
```

为什么？

> $$
> \begin{cases}
> matrix[n−row−1][col]=matrix[row][col]\\
> matrix[col][row]=matrix[row][col]
> \end{cases}
> $$
>
> 等价于
> $$
> matrix[col][n-row-1]=matrix[row][col]
> $$

#### [搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

**题目：**

编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

> 输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
> 输出：true

**题解：**

类似二分查找，从左下角开始，如果当前值大于目标值，那么向上走，如果当前值小于目标值，向右走。

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size(), n = matrix[0].size();
    if (m == 0)
        return false;
    int i = m - 1, j = 0;
    while(i >= 0 && j < n) {
        if (matrix[i][j] == target)
            return true;
        else if (matrix[i][j] > target)
            i--;
        else
            j++;
    }
    return false;
}
```

#### [最多能完成排序的块](https://leetcode.cn/problems/max-chunks-to-make-sorted/)

**题目：**

给定一个长度为 `n` 的整数数组 `arr` ，它表示在 `[0, n - 1]` 范围内的整数的排列。

我们将 `arr` 分割成若干 **块** (即分区)，并对每个块单独排序。将它们连接起来后，使得连接的结果和按升序排序后的原数组相同。

返回数组能分成的最多块数量。

> 输入: arr = [1,0,2,3,4]
> 输出: 4
> 解释:
> 我们可以把它分成两块，例如 [1, 0], [2, 3, 4]。然而，分成 [1, 0], [2], [3], [4] 可以得到最多的块数。对每个块单独排序后，结果为 [0, 1], [2], [3], [4]

**题解：**

从左往右遍历，同时记录当前的最大值，每当当前最大值等于数组位置时，我们可以多一次分割。

为什么可以通过这个算法解决问题呢？如果当前最大值大于数组位置，则说明右边一定有小于数组位置的数字，需要把它也加入待排序的子数组；又因为数组只包含不重复的 0 到 n，所以当前最大值一定不会小于数组位置。

```cpp
int maxChunksToSorted(vector<int>& arr) {
    int n = arr.size();
    int max = arr[0], ans = 0;
    for (int i = 0; i < n; ++i) {
        if (arr[i] > max)
            max = arr[i];
        if (max == i)
            ans++;
    }
    return ans;
}
```

### 栈和队列

#### [用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

**题目：**

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**题解：**

我们可以用两个栈来实现一个队列：因为我们需要得到先入先出的结果，所以必定要通过一个额外栈翻转一次数组。这个翻转过程既可以在插入时完成，也可以在取值时完成。

```cpp
class MyQueue {
    private:
    stack<int> in, out;
    void in2out() {
        if (out.empty()) {
            while (!in.empty()) {
                out.push(in.top());
                in.pop();
            }
        }
    }
    public:
    MyQueue() { }
    void push(int x) {
        in.push(x);
    }
    int pop() {
        in2out();
        int x = out.top();
        out.pop();
        return x;
    }
    int peek() {
        in2out();
        return out.top();
    }
    bool empty() {
        return in.empty() & out.empty();
    }
};
```

#### [最小栈](https://leetcode.cn/problems/min-stack/)

**题目：**

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

**题解：**

新开一个栈 `min_s` ，栈顶是最小值，有序栈。`push` 时，若小于栈顶元素，同时压入 `min_s` 中；`pop` 时，若等于栈顶元素，同时弹出 `min_s` 栈顶。

```cpp
class MinStack {
    private:
    stack<int> s, min_s;
    public:
    MinStack() {}
    void push(int x) {
        s.push(x);
        if (min_s.empty() || min_s.top() >= x)
            min_s.push(x);
    }
    void pop() {
        if (!min_s.empty() && min_s.top() == s.top())
            min_s.pop();
        s.pop();
    }
    int top() {
        return s.top();
    }
    int getMin() {
        return min_s.top();
    }
};
```

> 思考：为什么能保证 `min_s` 栈顶元素始终是最小值？

考虑 `min_s` 的变化，`push` 的是比栈顶元素更小的值，一定是新的最小值；`pop` 之后的栈顶值是原来的次小值，原来的最小值去掉后当然是新的最小值。

#### [有效的括号](https://leetcode.cn/problems/valid-parentheses/)

**题目：**

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 每个右括号都有一个对应的相同类型的左括号。

> 输入：s = "()[]{}"
> 输出：true

**题解：**

典型的栈应用，先进后出。

```cpp
bool isValid(string s) {
    stack<char> str;
    for (auto e : s) {
        if (e == '(' || e == '{' || e == '[')
            str.push(e);
        else {
            if (str.empty())
                return false;
            else {
                char e1 = str.top();
                if ((e == ')' && e1 == '(') || 
                    (e == '}' && e1 == '{') || 
                    (e == ']' && e1 == '['))
                    str.pop();
                else
                    return false;
            }
        }
    }
    return str.empty();
}
```

### 单调栈

#### [每日温度](https://leetcode.cn/problems/daily-temperatures/)

**题目：**

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

> 输入: temperatures = [73,74,75,71,69,72,76,73]
> 输出: [1,1,4,2,1,1,0,0]

**题解：**

维持一个有序栈，栈顶是当前最低温度对应的日期，碰到比它更低的就 `push`；碰到比他高的就计算天数之差，然后 `pop`，继续和栈顶比较。

```cpp
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> ans(n);
    stack<int> day;
    for (int i = 0; i < n; ++i) {
        while (!day.empty()) {
            int j = day.top();
            if (temperatures[i] <= temperatures[j])
                break;
            else {
                ans[day.top()] = i - j;
                day.pop();
            }
        }
        day.push(i);
    }
    return ans;
}
```

### 优先队列

优先队列（priority queue）可以在 O(1) 时间内获得最大值，并且可以在 O(log n) 时间内取出最大值或插入任意值。优先队列常常用 `堆（heap）` 来实现。堆是一个完全二叉树，其每个节点的值总是大于等于子节点的值。实际实现堆时，我们通常用一个数组而不是用指针建立一个树。这是因为堆是完全二叉树，所以用数组表示时，位置 i 的节点的父节点位置一定为 `(i-1)/2`，而它的两个子节点的位置又一定分别为 `2i+1` 和 `2i+2`。

以下是堆的实现方法，其中最核心的两个操作是上浮和下沉：如果一个节点比父节点大，那么需要交换这个两个节点；交换后还可能比它新的父节点大，因此需要不断地进行比较和交换操作，我们称之为上浮；类似地，如果一个节点比父节小，也需要不断地向下进行比较和交换操作，我们称之为下沉。如果一个节点有两个子节点，我们总是交换最大的子节点。

```cpp
vector<int> heap;
// 获得最大值
void top() {
    return heap[0];
}
// 插入任意值：把新的数字放在最后一位，然后上浮
void push(int k) {
    heap.push_back(k);
    swim(heap.size() - 1);
}
// 删除最大值：把最后一个数字挪到开头，然后下沉
void pop() {
    heap[0] = heap.back();
    heap.pop_back();
    sink(0);
}
// 上浮
void swim(int pos) {
    while (pos > 0 && heap[(pos-1)/2] < heap[pos])) {
        swap(heap[(pos-1)/2], heap[pos]);
        pos = (pos - 1) / 2;
    }
}
// 下沉
void sink(int pos) {
    while (2 * pos + 1 <= N) {
        int i = 2 * pos + 1;
        if (i < N && heap[i] < heap[i+1]) 
            ++i;
        if (heap[pos] >= heap[i]) 
            break;
        swap(heap[pos], heap[i]);
        pos = i;
    }
}
```

另外，正如我们在 STL 章节提到的那样，如果我们需要在维持大小关系的同时，还需要支持查找任意值、删除任意值、维护所有数字的大小关系等操作，可以考虑使用 set 或 map 来代替优先队列。

#### [合并 k 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

**题目：**

给你一个链表数组，每个链表都已经按升序排列。请你将所有链表合并到一个升序链表中，返回合并后的链表。

> 输入：lists = [[1,4,5],[1,3,4],[2,6]]
> 输出：[1,1,2,3,4,4,5,6]

**题解：**

把所有的链表存储在一个优先队列中，每次提取所有链表头部节点值最小的那个节点插入链表中，直到所有链表都被提取完为止。需要设置一个虚拟头结点，方便代码书写。

```cpp
struct Comp {
    bool operator() (ListNode* l1, ListNode* l2) {
        return l1->val > l2->val;
    }
};
ListNode* mergeKLists(vector<ListNode*>& lists) {
    if (lists.empty()) 
        return nullptr;
    priority_queue<ListNode*, vector<ListNode*>, Comp> q;
    for (ListNode* list: lists) {
        if (list)
            q.push(list);
    }
    ListNode* dummy = new ListNode(0), *cur = dummy;
    while (!q.empty()) {
        cur->next = q.top();
        q.pop();
        cur = cur->next;
        if (cur->next)
            q.push(cur->next);
    }
    return dummy->next;
}
```

#### [天际线问题](https://leetcode.cn/problems/the-skyline-problem/)

**题目：**

城市的 **天际线** 是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的天际线。

每个建筑物的几何信息由数组 `buildings` 表示，其中三元组 `buildings[i] = [lefti, righti, heighti]` 表示：

- `lefti` 是第 `i` 座建筑物左边缘的 `x` 坐标。
- `righti` 是第 `i` 座建筑物右边缘的 `x` 坐标。
- `heighti` 是第 `i` 座建筑物的高度。

你可以假设所有的建筑都是完美的长方形，在高度为 `0` 的绝对平坦的表面上。

> 输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
> 输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]

**题解：**

