#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)（哈希+滑动窗口）

##### 字节跳动

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```



```cpp
class Solution{
public:
    int lengthOfLongestSubstring(string s){
        if (s.length() == 0)
            return 0;
        unordered_map<char, int> uo_map;
        int start = 0;
        int maxlen = 0;
        for (int i = 0; i < s.length(); i++){
            if (uo_map.find(s[i]) == uo_map.end()){
                //没有出现过
                uo_map.insert(make_pair(s[i], i));
            }
            else{
                start = max(start, uo_map[s[i]] + 1);//重复元素已经不在滑动窗口范围内
                uo_map[s[i]] = i;
            }
            maxlen = max(maxlen, i - start + 1);
        }
        return maxlen;
    }
};
```





#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

##### 富途

难度中等2571收藏分享切换为英文关注反馈

给你一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        if (nums.size() < 3) {
            return res;
        }
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size() - 2; i++) {
            if (i != 0 && nums[i] == nums[i - 1]) continue;
            int target = 0 - nums[i];
            int p = i + 1, q = nums.size() - 1;
            while (p < q) {
                if (nums[p] + nums[q] > target) {
                    q--;
                }
                else if (nums[p] + nums[q] < target) {
                    p++;
                }
                else if (nums[p] + nums[q] == target) {
                    res.push_back({ nums[i], nums[p], nums[q] });
                    p++;
                    q--;
                    while (p < nums.size() && nums[p] == nums[p - 1]) {
                        p++;
                    }
                    while (q >= 0 && nums[q] == nums[q + 1]) {
                        q--;
                    }
                }
            }
        }
        return res;
    }
};
```



#### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)（模拟）

##### 滴滴

斐波那契蛇

给定一个包含 *m* x *n* 个元素的矩阵（*m* 行, *n* 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

**示例 1:**

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```

**示例 2:**

```
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```



```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        //上下左右边界
        int up_border = 0;
        int down_border = matrix.size() - 1;
        if(down_border < 0) return res;
        int left_border = 0;
        int right_border = matrix[0].size() - 1;
        //遍历
        while(1){
            for(int i = left_border; i <= right_border; i++){
                res.push_back(matrix[up_border][i]);
            }
            up_border++;
            if(up_border > down_border || left_border > right_border) break;
            for(int i = up_border; i <= down_border; i++){
                res.push_back(matrix[i][right_border]);
            }
            right_border--;
            if(up_border > down_border || left_border > right_border) break;
            for(int i = right_border; i >= left_border; i--){
                res.push_back(matrix[down_border][i]);
            }
            down_border--;
            if(up_border > down_border || left_border > right_border) break;
            for(int i = down_border; i >= up_border; i--){
                res.push_back(matrix[i][left_border]);
            }
            left_border++;
            if(up_border > down_border || left_border > right_border) break;
        }
        return res;
    }
};
```



#### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)



给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。

 

**示例:**

```
输入: [2,1,5,6,2,3]
输出: 10
```



```cpp
class Solution {
public:
    int largestRectangleArea(vector<int> arr){
        if(arr.size() == 0) return 0;
        if(arr.size() == 1) return arr[0];
        
        int res = 0;
        //单调栈
        stack<int> s;
        s.push(0);
        //前后哨兵
        arr.insert(arr.begin(), 1, 0);
        arr.push_back(0);
        int len = arr.size();
        for(int i = 1; i < len; i++){
            while(arr[i] < arr[s.top()]){
                //原栈顶元素出栈（它能勾勒出的最大矩形已经可以确定）
                int height = arr[s.top()];
                s.pop();
                //确定矩形宽度
                int width = i - s.top() - 1;
                res = (height * width) > res ? (height * width) : res;
            }
            //当前元素进栈
            s.push(i);
        }
        return res;
    }
};
```





#### [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)（单调栈+哨兵）

##### 心动网络&&Tap Tap，奇安信

给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

**示例:**

```
输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6
```

```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size() == 0) return 0;
        int res = 0;
        int n = matrix.size();
        int m = matrix[0].size();
        vector<int> heights(m, 0);
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(matrix[i][j] == '0'){
                    heights[j] = 0;
                }else{
                    heights[j] = heights[j] + 1;
                }
            }
            res = max(res, largestRectangleArea(heights));
        }
        return res;
    }

    int largestRectangleArea(vector<int> heights){
        if(heights.size() == 0) return 0;
        if(heights.size() == 1) return heights[0];
        
        int res = 0;
        //单调栈
        stack<int> s;
        s.push(0);
        //前后哨兵
        heights.insert(heights.begin(), 1, 0);
        heights.push_back(0);
        int len = heights.size();
        for(int i = 1; i < len; i++){
            while(heights[i] < heights[s.top()]){
                //原栈顶元素出栈（它能勾勒出的最大矩形已经可以确定）
                int height = heights[s.top()];
                s.pop();
                //确定矩形宽度
                int width = i - s.top() - 1;
                res = (height * width) > res ? (height * width) : res;
            }
            //当前元素进栈
            s.push(i);
        }
        return res;
    }
};
```





#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

##### 字节跳动

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```



```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    //先序数组确定root,中序数组确定左右子树
    vector<int> preArr;
    vector<int> inoArr;
    unordered_map<int, int> uo_map;        // 哈希表维护中序数组中元素和下标的关系
    TreeNode* build(int prev, int left, int right){
        if(left > right) return nullptr;
        TreeNode* root = new TreeNode(preArr[prev]);
        if(left == right){
            return root;
        }
        int index = uo_map[preArr[prev]];  // 找到当前root在中序数组中的位置
        root->left = build(prev + 1, left, index - 1);
        root->right = build(prev + (index - left) + 1, index + 1, right);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() == 0 || inorder.size() == 0) 
            return nullptr;
        preArr = preorder;
        inoArr = inorder;
        for(int i = 0; i < inoArr.size(); i++){
            uo_map[inorder[i]] = i;
        }
        return build(0, 0, preorder.size() - 1);
    }
};
```



#### [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)（双向链表+哈希）

##### 网易互娱

运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

**进阶:**

你是否可以在 **O(1)** 时间复杂度内完成这两种操作？

**示例:**

```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```



```cpp
struct LinkedListNode{
    int key;
    int value;
    LinkedListNode* prev;
    LinkedListNode* next;
    LinkedListNode() : key(0), value(0), prev(nullptr), next(nullptr){}
    LinkedListNode(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr){}
};

class LRUCache {
private:
    int size;               //当前容量
    int _capacity;          //最大容量
    LinkedListNode* head;   //伪头节点
    LinkedListNode* tail;   //伪尾节点
    unordered_map<int, LinkedListNode*> uo_map;
public:
    LRUCache(int capacity) : _capacity(capacity), size(0){
        head = new LinkedListNode();
        tail = new LinkedListNode();
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if(uo_map.find(key) != uo_map.end()){
            LinkedListNode* node = uo_map.at(key);
            removeNode(node);
            addToHead(node);
            return node->value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if(uo_map.count(key)){  //节点已经存在,更新value,移动到链表头部
            LinkedListNode* node = uo_map[key];
            removeNode(node);
            addToHead(node);
            node->value = value;
        }
        else{
            LinkedListNode* node = new LinkedListNode(key, value);
            uo_map.insert(pair<int, LinkedListNode*>(key, node));
            addToHead(node);
            size++;
            if(size > _capacity){              // 缓存已经满
                LinkedListNode* remove_node = removeTailNode();
                uo_map.erase(remove_node->key);
                delete remove_node;
                size--;
            }
        }
    }

    void removeNode(LinkedListNode* node){  // 双向链表删除节点
        node->prev->next = node->next;
        node->next->prev = node->prev;
        node->prev = nullptr;
        node->next = nullptr;
    }

    void addToHead(LinkedListNode* node){  // 头部插入节点
        node->next = head->next;
        head->next->prev = node;
        node->prev = head;
        head->next = node;
    }

    void addToTail(LinkedListNode* node){  //尾部插入节点
        tail->prev->next = node;
        node->prev = tail->prev;
        node->next = tail;
        tail->prev = node;
    }

    LinkedListNode* removeTailNode(){
        LinkedListNode* node = tail->prev;
        removeNode(node);
        return node;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```



#### [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

##### 百度地图

对链表进行插入排序。

![img](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

 

**插入排序算法：**

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

 

**示例 1：**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2：**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if(!head || !head->next){
            return head;
        }
        ListNode* guard = new ListNode(10086);//哨兵
        guard->next = head;
        ListNode* tail = head;
        ListNode* cur = head->next;
        while(cur){
            if(cur->val < tail->val){
                tail->next = cur->next;
                ListNode* p = guard;
                while(p->next->val < cur->val) p = p->next;
                cur->next = p->next;
                p->next = cur;
                cur = tail->next;
            }
            else{
                tail = cur;
                cur = cur->next;
            }
        }
        return guard->next;
    }
};
```







#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

##### 富途

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == NULL){
            return NULL;
        }
        ListNode* t = NULL;
        ListNode* p = head;
        ListNode* q = head->next;
        p->next = NULL;
        while(q != NULL){
            t = q->next;
            q->next = p;
            p = q;
            q = t;
        }
        //此时p指向翻转后的头节点
        return p;
    }
};
```





#### [946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)（模拟）

##### 字节跳动

给定 `pushed` 和 `popped` 两个序列，每个序列中的 **值都不重复**，只有当它们可能是在最初空栈上进行的推入 push 和弹出 pop 操作序列的结果时，返回 `true`；否则，返回 `false` 。

 

**示例 1：**

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**示例 2：**

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

 

**提示：**

1. `0 <= pushed.length == popped.length <= 1000`
2. `0 <= pushed[i], popped[i] < 1000`
3. `pushed` 是 `popped` 的排列。

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        int index = 0;
        stack<int> s;
        for(auto& num : pushed){
            s.push(num);
            while(!s.empty() && s.top() == popped[index]){
                s.pop();
                index++;
            }
        }
        return s.empty();
    }
};
```



#### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

##### 追一科技

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

 

**示例 1：**

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

**示例 2：**

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

**提示：**

- `1 <= values <= 10000`
- `最多会对 appendTail、deleteHead 进行 10000 次调用`

```cpp
class CQueue {
private:
    stack<int> s1;
    stack<int> s2;
public:
    CQueue() {

    }
    
    void appendTail(int value) {
        s1.push(value);
    }
    
    int deleteHead() {
        if(s2.empty()){
            if(s1.empty()){
                return -1;
            }
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        int res = s2.top();
        s2.pop();
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
```





#### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)（双指针）

##### 网易互娱，追一科技

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```



```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        int len = 0;
        ListNode* p = head;
        while(p){
            p = p->next;
            ++len;
        }
        int count = 0;
        ListNode* pp = head;
        while(pp){
            if(count++ == len - k){
                return pp; 
            }
            pp = pp->next;
        }
        return NULL;
    }
};
```



#### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)（队列）

##### 百度地图

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回：

```
[3,9,20,15,7]
```



```cpp
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        if(root == NULL) return res;
        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()){
            TreeNode* node = q.front();
            q.pop();
            res.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }

        return res;
    }
};
```





#### [面试题 17.14. 最小K个数](https://leetcode-cn.com/problems/smallest-k-lcci/)（快排变形）

##### 百度地图，富途

设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

**示例：**

```
输入： arr = [1,3,5,7,2,4,6,8], k = 4
输出： [1,2,3,4]
```

**提示：**

- `0 <= len(arr) <= 100000`
- `0 <= k <= min(100000, len(arr))`

```cpp
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        return vector<int>(arr.begin(), arr.begin() + k);
    }
};
```





#### [面试题 10.01. 合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)（双指针）

##### 追一科技

给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

初始化 A 和 B 的元素数量分别为 *m* 和 *n*。

**示例:**

```
输入:
A = [1,2,3,0,0,0], m = 3
B = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```

**说明:**

- `A.length == n + m`



```cpp
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        //赋值一份A数组的有效段
        vector<int> C;
        C.assign(A.begin(), A.begin() + m);
        //操作
        int a = 0, b = 0, c = 0;
        while(b < B.size() || c < C.size()){
            if(b == B.size()){
                A[a++] = C[c++];
                continue;
            }
            if(c == C.size()){
                A[a++] = B[b++];
                continue;
            }
            if(B[b] < C[c]){
                A[a] = B[b];
                b++;
            }
            else{
                A[a] = C[c];
                c++;
            }
            a++;
        }
    }
};
```



#### 其他

汉诺塔游戏（追一）

**一个数m，一个长度为n的数组，取其中任意k个元素（不能重复取）的和为sum，求sum % m的最大值？**（字节）

一个数num，可以拆成a,b,满足a+b=num,设val = s(a) + s(b)，其中s函数为求该数各个位上的和（如s(1234)= 1 + 2 + 3 + 4），求val的最大值（腾讯）

