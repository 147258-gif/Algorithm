[TOC]



## 前缀和

### **[303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)**

给定一个整数数组  `nums`，处理以下类型的多个查询:

1. 计算索引 `left` 和 `right` （包含 `left` 和 `right`）之间的 `nums` 元素的 **和** ，其中 `left <= right`

实现 `NumArray` 类：

- `NumArray(int[] nums)` 使用数组 `nums` 初始化对象
- `int sumRange(int i, int j)` 返回数组 `nums` 中索引 `left` 和 `right` 之间的元素的 **总和** ，包含 `left` 和 `right` 两点（也就是 `nums[left] + nums[left + 1] + ... + nums[right]` )

**示例 1：**

```
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1))
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

**提示：**

- `1 <= nums.length <= 104`
- `105 <= nums[i] <= 105`
- `0 <= i <= j < nums.length`
- 最多调用 `104` 次 `sumRange` ****方法

```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.preSum = [0] * (len(nums) + 1)
        for i in range(1, len(self.preSum)):
            self.preSum[i] = self.preSum[i - 1] + nums[i - 1]

    def sumRange(self, left: int, right: int) -> int:
        return self.preSum[right + 1] - self.preSum[left]

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(left,right)
class NumArray:

    def __init__(self, nums: List[int]):
        # 初始化一个前缀和数组，开头加上一个额外的0
        # 这样后面计算区间和会更容易
        self.preSum = [0] * (len(nums) + 1)
        for i in range(1, len(self.preSum)):
            # 通过将当前元素加上前一个元素的前缀和来计算当前元素的前缀和
            self.preSum[i] = self.preSum[i - 1] + nums[i - 1]

    def sumRange(self, left: int, right: int) -> int:
        # 计算区间和时，我们将右端点下标+1的前缀和减去左端点下标的前缀和
        # 这是因为前缀和数组开头加了一个额外的0
        return self.preSum[right + 1] - self.preSum[left]
```

时间复杂度: O(n)，其中n是输入数组的长度。

空间复杂度: O(n)，需要一个前缀和数组来存储前缀和。

这是一个用于计算输入数组中一定范围内元素和的 Python 实现 `NumArray` 类。

`__init__` 方法使用输入数组 `nums` 初始化一个前缀和数组。前缀和数组是一个数组，其中第i个元素是原始数组前i个元素的总和。前缀和数组开头加了一个额外的0，这使得后面计算区间和更容易。这个方法的时间复杂度是O(n)，其中n是输入数组的长度。

`sumRange` 方法接受两个下标 `left` 和 `right`，并返回这两个下标之间（包括这两个下标）的元素的和。为了计算这个和，我们将右端点下标加1的前缀和减去左端点下标的前缀和。这个方法的时间复杂度是O(1)。

`NumArray` 对象使用输入数组 `nums` 实例化。对 `NumArray` 对象调用 `sumRange` 方法并传入两个下标 `left` 和 `right`，以返回在输入数组中这两个下标之间（包括这两个下标）的元素的和。







### **[304. 二维区域和检索 - 矩阵不可变](https://leetcode.cn/problems/range-sum-query-2d-immutable/)**

给定一个二维矩阵 `matrix`，以下类型的多个请求：

- 计算其子矩形范围内元素的总和，该子矩阵的 **左上角** 为 `(row1, col1)` ，**右下角** 为 `(row2, col2)` 。

实现 `NumMatrix` 类：

- `NumMatrix(int[][] matrix)` 给定整数矩阵 `matrix` 进行初始化

- `int sumRegion(int row1, int col1, int row2, int col2)` 返回 所描述的子矩阵的元素 **总和** 。

  **左上角**

  `(row1, col1)` 、**右下角** `(row2, col2)`

  https://pic.leetcode-cn.com/1626332422-wUpUHT-image.png

  ![https://pic.leetcode-cn.com/1626332422-wUpUHT-image.png](https://pic.leetcode-cn.com/1626332422-wUpUHT-image.png)

**示例 1：**

```
输入:
["NumMatrix","sumRegion","sumRegion","sumRegion"]
[[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
输出:
[null, 8, 11, 12]

解释:
NumMatrix numMatrix = new NumMatrix([[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (红色矩形框的元素总和)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (绿色矩形框的元素总和)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (蓝色矩形框的元素总和)
```

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `105 <= matrix[i][j] <= 105`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- 最多调用 `104` 次 `sumRegion` 方法

```python
class NumMatrix:
    def __init__(self, matrix: List[List[int]]):
        m, n = len(matrix), len(matrix[0])
        # 计算前缀和
        if m == 0 or n == 0:
            return
        # 初始化前缀和数组
        self.preSum = [[0] * (n + 1) for _ in range(m + 1)]
        # 初始化前缀和数组
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                self.preSum[i][j] = self.preSum[i - 1][j] + self.preSum[i][j - 1] + matrix[i - 1][j - 1] - self.preSum[i - 1][j - 1]

    def sumRegion(self, x1: int, y1: int, x2: int, y2: int) -> int:
        # 计算子矩阵和
        return self.preSum[x2 + 1][y2 + 1] - self.preSum[x1][y2 + 1] - self.preSum[x2 + 1][y1] + self.preSum[x1][y1]
        # 根据公式计算子矩阵和

# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
[0, 0 , 0 , 0 , 0 , 0 ]
[0, 3 , 3 , 4 , 8 , 10]
[0, 8 , 14, 18, 24, 27]
[0, 9 , 17, 21, 28, 36]
[0, 13, 22, 26, 34, 49]
[0, 14, 23, 30, 38, 58]
```

- 第 11 行：返回一个表达式，该表达式计算了目标矩阵四个相邻矩阵的和，然后减去两个交叉矩阵的和。具体来说，`self.preSum[x2 + 1][y2 + 1]` 表示右下角矩阵的元素和，`self.preSum[x1][y2 + 1]` 表示上面的矩阵元素和，`self.preSum[x2 + 1][y1]` 表示左边的矩阵元素和，`self.preSum[x1][y1]` 表示左上角矩阵的元素和。因为 `preSum` 矩阵中的索引从 `1` 开始，所以需要添加 `1`。

计算时间复杂度：

- `__init__()` 方法中的两个嵌套循环，时间复杂度为 O(mn)。
- `sumRegion()` 方法中只有常数时间的计算，时间复杂度为 O(1)。

因此，总的时间复杂度为 O(mn)。

计算空间复杂度：

- 初始化前缀和数组时，使用了额外的 O(mn) 的空间。
- `sumRegion()` 方法中只有常数个变量，空间复杂度为 O(1)。

因此，总的空间复杂度为 O(mn)。