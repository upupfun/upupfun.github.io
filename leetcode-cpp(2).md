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

> 一次插一个节点，剩下的再放回优先队列

#### [\*天际线问题](https://leetcode.cn/problems/the-skyline-problem/)

**题目：**

城市的 **天际线** 是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的天际线。

每个建筑物的几何信息由数组 `buildings` 表示，其中三元组 `buildings[i] = [lefti, righti, heighti]` 表示：

- `lefti` 是第 `i` 座建筑物左边缘的 `x` 坐标。
- `righti` 是第 `i` 座建筑物右边缘的 `x` 坐标。
- `heighti` 是第 `i` 座建筑物的高度。

你可以假设所有的建筑都是完美的长方形，在高度为 `0` 的绝对平坦的表面上。

> 输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
> 输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]

<img src="https://assets.leetcode.com/uploads/2020/12/01/merged.jpg" alt="img" style="zoom: 67%;" />

**题解：**

首先对关键点(每个建筑的左右边缘)升序排列，然后依次找包含(左边缘小于等于，右边缘大于)某关键点的建筑物的最大高度。暴力方法就是对每个关键点都遍历每个建筑，找到最大高度；可以用优先队列优化，`push` 进左边缘符合的建筑，`pop` 出右边缘不符合的建筑，队列头元素始终是最大高度，这样只需遍历建筑一次。

```cpp
vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
    vector<int> boundaries;
    for (auto& building : buildings) {
        boundaries.emplace_back(building[0]);
        boundaries.emplace_back(building[1]);
    }
    // 对边缘排序
    sort(boundaries.begin(), boundaries.end());
    priority_queue<pair<int, int>> que;
    vector<vector<int>> ret;
    int n = buildings.size(), idx = 0;
    for (auto& boundary : boundaries) {
        // push
        while (idx < n && buildings[idx][0] <= boundary) {
            que.emplace(buildings[idx][2], buildings[idx][1]);
            idx++;
        }
        // pop
        while (!que.empty() && que.top().second <= boundary)
            que.pop();
        // 获取天际线高度
        int maxn = que.empty() ? 0 : que.top().first;
        if (ret.size() == 0 || maxn != ret.back()[1])
            ret.push_back({boundary, maxn});
    }
    return ret;
}
```

> 为什么能只遍历一次而不遗漏呢？

首先，`idx` 一直往前推，会不会漏掉包含后面的关键点的建筑？

不会。如果此建筑还没进 `que`，那么就还在后面，没漏掉；如果此建筑进了 `que`，那么就一定还没被 `pop`，没漏掉（假设被弹出了，因为被 `pop` 的条件是右边缘小于等于上一个关键点，又关键点是升序遍历的，所以右边缘肯定也小于等于此关键点，矛盾！）

其次，会不会有包含此关键点的建筑没被 `push`？

不会，因为 `buildings` 的左端是升序的，而 `push` 结束的条件是左端大于关键点，所以最后一个被 `push` 的建筑往后左端一定不符合。

### 双端队列

#### [滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

**题目：**

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

> 输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
> 输出：[3,3,5,5,6,7]

**题解：**

每当向右移动时，如果窗口左端就是队列左端，那么 `pop_front` ；把队列右边小于窗口右端的值全部 `pop_back`，再 `push_back(i)`，这样双端队列的最左端永远是当前窗口内的最大值。从左到右递减。

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> ans;
    for (int i = 0; i < nums.size(); ++i) {
        if (!dq.empty() && dq.front() == i - k)
            dq.pop_front();
        while (!dq.empty() && nums[dq.back()] < nums[i])
            dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1)
            ans.push_back(nums[dq.front()]);
    }
    return ans;
}
```

> 总觉得这种东西妙不可言，可能是水平不够🤣

### 哈希表

#### [两数之和](https://leetcode.cn/problems/two-sum/)

**题目：**

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** `target` 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。你可以按任意顺序返回答案。

> 输入：nums = [2,7,11,15], target = 9
> 输出：[0,1]

**题解：**

我们可以利用哈希表存储遍历过的值以及它们的位置，每次遍历到位置 i 的时候，查找哈希表里是否存在 target - nums[i]，若存在，则说明这两个值的和为 target。

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> hash;
    vector<int> ans;
    for (int i = 0; i < nums.size(); ++i) {
        int num = nums[i];
        auto pos = hash.find(target - num);
        if (pos != hash.end()) {
            ans.push_back(pos->second);
            ans.push_back(i);
            return ans;
        }
        else
            hash[num] = i;
    }
    return ans;
}
```

#### [最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

**题目：**

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

> 输入：nums = [100,4,200,1,3,2]
> 输出：4

**题解：**

我们可以把所有数字放到一个哈希表，然后不断地从哈希表中任意取一个值，并删除掉其之前之后的所有连续数字，然后更新目前的最长连续序列长度。重复这一过程，我们就可以找到所有的连续数字序列。

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> hash;
    for (const int & num: nums)
        hash.insert(num);
    int ans = 0;
    while (!hash.empty()) {
        int cur = *(hash.begin());
        hash.erase(cur);
        int next = cur + 1, prev = cur - 1;
        while (hash.count(next))
            hash.erase(next++);
        while (hash.count(prev))
            hash.erase(prev--);
        ans = max(ans, next - prev - 1);
    }
    return ans;
}
```

#### [直线上最多的点数](https://leetcode.cn/problems/max-points-on-a-line/)

**题目：**

给你一个数组 `points` ，其中 `points[i] = [xi, yi]` 表示 **X-Y** 平面上的一个点。求最多有多少个点在同一条直线上。(点不重复)

> 输入：points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
> 输出：4

**题解：**

对每一个点，对不同斜率建立 `hash` 表。

```cpp
int maxPoints(vector<vector<int>>& points) {
    unordered_map<double, int> hash;
    int max_count = 0, same_x, same_y;
    for (int i = 0; i < points.size(); ++i) {
        same_x = same_y = 1;
        for (int j = i+1; j < points.size(); ++j) {
            if (points[i][1] == points[j][1])
                ++same_y;
            else if (points[i][0] == points[j][0])
                ++same_x;
            else {
                double dx = points[i][0] - points[j][0], dy = points[i][1] - points[j][1];
                ++hash[dx/dy];
            }
        }
        max_count = max(max_count, max(same_x, same_y));
        for (auto item : hash)
            max_count = max(max_count, 1+item.second);
        hash.clear();
    }
    return max_count;
}
```

### 多重集合和映射

#### [重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

**题目：**

给你一份航线列表 `tickets` ，其中 `tickets[i] = [fromi, toi]` 表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。

所有这些机票都属于一个从 `JFK`（肯尼迪国际机场）出发的先生，所以该行程必须从 `JFK` 开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

- 例如，行程 `["JFK", "LGA"]` 与 `["JFK", "LGB"]` 相比就更小，排序更靠前。

假定所有机票至少存在一种合理的行程。且所有的机票 必须都用一次 且 只能用一次。

> 输入：tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"]，["ATL","SFO"]]
> 输出：["JFK","ATL","JFK","SFO","ATL","SFO"]
> 解释：另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"] ，但是它字典排序更大更靠后。

**题解：**

本题可以先用哈希表记录起止机场，其中键是起始机场，值是一个多重集合，表示对应的终止机场。因为一个人可能坐过重复的线路，所以我们需要使用多重集合储存重复值。储存完成之后，我们可以利用栈来恢复从终点到起点飞行的顺序，再将结果逆序得到从起点到终点的顺序。

```cpp
vector<string> findItinerary(vector<vector<string>>& tickets) {
    vector<string> ans;
    if (tickets.empty())
        return ans;
    unordered_map<string, multiset<string>> hash;
    for (const auto & ticket: tickets)
        hash[ticket[0]].insert(ticket[1]);
    stack<string> s;
    s.push("JFK");
    while (!s.empty()) {
        string next = s.top();
        if (hash[next].empty()) {
            ans.push_back(next);
            s.pop();
        }
        else {
            s.push(*hash[next].begin());
            hash[next].erase(hash[next].begin());
        }
    }
    reverse(ans.begin(), ans.end());
    return ans;
}
```

> 妙哉！

### 前缀和与积分图

一维的前缀和，二维的积分图，都是把每个位置之前的一维线段或二维矩形预先存储，方便加速计算。如果需要对前缀和或积分图的值做寻址，则要存在哈希表里；如果要对每个位置记录前缀和或积分图的值，则可以储存到一维或二维数组里，也常常伴随着动态规划。

#### [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

**题目：**

Given an integer array `nums`, handle multiple queries of the following type:

Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**题解：**

建立一个与数组 nums 长度相同的前缀和数组 psum，表示 nums 每个位置之前前所有数字的和。cpp 中可以用 `partial_sum` 函数实现。

```cpp
class NumArray {
    vector<int> psum;
    public:
    NumArray(vector<int>& nums) {
        psum.resize(nums.size() + 1);
        partial_sum(nums.begin(), nums.end(), psum.begin() + 1);
    }
    int sumRange(int left, int right) {
        return psum[right+1] - psum[left];
    }
};
```

#### [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

**题目：**

Given a 2D matrix `matrix`, handle multiple queries of the following type:

Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the `NumMatrix` class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

You must design an algorithm where `sumRegion` works on `O(1)` time complexity.

**题目：**

类似于前缀和，我们可以把这种思想拓展到二维，即积分图（image integral）。我们可以先建立一个 intergral 矩阵，intergral\[i\]\[j\] 表示以位置 (0, 0) 为左上角、位置 (i-1, j-1) 为右下角的长方形中所有数字的和。计算可以用 dp

```cpp
class NumMatrix {
    vector<vector<int>> integral;
    public:
    NumMatrix(vector<vector<int>> matrix) {
        int m = matrix.size(), n = m > 0 ? matrix[0].size() : 0;
        integral = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                integral[i][j] = matrix[i-1][j-1] + integral[i-1][j] + integral[i][j-1] - integral[i-1][j-1];
            }
        }
    }
    int sumRegion(int row1, int col1, int row2, int col2) {
        return integral[row2+1][col2+1] - integral[row2+1][col1] - integral[row1][col2+1] + integral[row1][col1];
    }
};
```

#### [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

**题目：**

Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to* `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

> Input: nums = [1,2,3], k = 3
> Output: 2

**题解：**

本题同样是利用前缀和，不同的是这里我们使用一个哈希表 hashmap，其键是前缀和，而值是该前缀和出现的次数。在我们遍历到位置 i 时，假设当前的前缀和是 `psum`，那么 `hashmap[psum-k]` 即为以当前位置结尾、满足条件的区间个数。

```cpp
int subarraySum(vector<int>& nums, int k) {
    int count = 0, psum = 0;
    unordered_map<int, int> hashmap;
    hashmap[0] = 1; // 初始化很重要
    for (int i: nums) {
        psum += i;
        count += hashmap[psum-k];
        ++hashmap[psum];
    }
    return count;
}
```

> 妙哉！😘

### 练习

