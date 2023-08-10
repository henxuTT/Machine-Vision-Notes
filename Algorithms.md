# 零散知识点

## i++ 与++i

+ `i++`是后缀递增操作符，它会先使用变量的当前值，然后将变量递增1。也就是说，`i++`先返回`i`的值，然后再将`i`的值加1。
+ `++i`是前缀递增操作符，它会先将变量递增1，然后再使用递增后的值。也就是说，`++i`先将`i`的值加1，然后再返回递增后的值。
  + int i = 5;
  + int result = i++;      
  + **result = 5, i = 6**



---



# 排序

常用的排序算法包括以下几种：

1. 冒泡排序（Bubble Sort）：通过不断交换相邻的元素，将最大（或最小）的元素逐渐移动到数组的末尾。
2. 插入排序（Insertion Sort）：将未排序的元素逐个插入到已排序的子数组中，通过比较和移动元素，最终将整个数组排序。
3. 选择排序（Selection Sort）：每次从未排序的部分选择出最小（或最大）的元素，将其与未排序部分的第一个元素交换位置。
4. 快速排序（Quick Sort）：通过选择一个基准元素，将数组分割成两个子数组，其中一个子数组的元素都小于基准元素，另一个子数组的元素都大于基准元素，然后对两个子数组递归地进行快速排序。
5. 归并排序（Merge Sort）：将数组递归地分成两半，分别对两个子数组进行归并排序，然后将两个有序的子数组合并成一个有序的数组。
6. 堆排序（Heap Sort）：将数组构建成最大（或最小）堆，然后将堆顶元素与数组末尾的元素交换位置，并调整堆，重复该过程直到整个数组有序。
7. 希尔排序（Shell Sort）：是插入排序的一种改进，通过将数组分成多个子序列并对每个子序列进行插入排序，最终对整个数组进行插入排序。
8. 计数排序（Counting Sort）：通过统计数组中每个元素的出现次数，然后根据统计信息将元素放回数组的正确位置，实现排序。
9. 桶排序（Bucket Sort）：将元素根据值的范围分配到不同的桶中，对每个桶中的元素进行排序，然后按照顺序将每个桶中的元素依次取出，得到有序数组。
10. 基数排序（Radix Sort）：根据元素的位数将数组分成多个桶，按照位数的顺序依次对每个桶中的元素进行排序，最终得到有序数组。

---

## 冒泡排序（Bubble Sort）

相邻比较，交换位置，末尾最大/小

O(n^2)

```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 交换arr[j]和arr[j + 1]
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}
```
---

## 插入排序（Insertion Sort）

从左到右，逐渐有序

O(n^2)

```Java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }
}
```
---

## 归并排序（Merge Sort）

#### 递归

+ 悬而未决->进栈；类似多叉树； 栈空间：树高度

+ Master公式 Recursive Master Theorem

  T(n) = a * T(n/b) + O(N^d)

  其中，T(n) 是递归算法的时间复杂度，n 是问题的规模，a 是递归调用发生的次数(重复次数)，b 是问题规模缩小的比例，O(N^d)是递归以外的操作的时间复杂度。

  递归 Master 公式分为三种情况：

  1. 如果 d < log_b(a)，则时间复杂度为 O(n^log_b(a))。
  2. 如果 d = log_b(a)，其中 k ≥ 0，且满足正则条件，则时间复杂度为 O(n^d * log_n)。
  3. 如果 d > log_b(a)，则时间复杂度为 O(n^log_b^a)。

#### 排序

不停分为左右两侧，使两侧都有序，再merge两侧

O(n*log n)

```java
public class MergeSort {
    public static void mergeSort(int[] arr) {
        if (arr.length <= 1) {
            return;
        }
        int mid = arr.length / 2;
        int[] left = new int[mid];
        int[] right = new int[arr.length - mid];
        System.arraycopy(arr, 0, left, 0, mid);
        System.arraycopy(arr, mid, right, 0, arr.length - mid);
        mergeSort(left);
        mergeSort(right);
        merge(arr, left, right);
    }

    private static void merge(int[] arr, int[] left, int[] right) {
        int i = 0, j = 0, k = 0;
        while (i < left.length && j < right.length) {
            if (left[i] <= right[j]) {
                arr[k++] = left[i++];
            } else {
                arr[k++] = right[j++];
            }
        }
        while (i < left.length) {
            arr[k++] = left[i++];
        }
        while (j < right.length) {
            arr[k++] = right[j++];
        }
    }
}
```
#### 小和问题

小和问题（Small Sum Problem）是一个经典的算法问题，其目标是计算一个数组中所有小于当前元素的元素之和。

等效于算右边有几个比它大。再乘以倍数

通过下标计算比左边大的数数目

```java
public class SmallSum {
    private static int mergeSort(int[] arr, int left, int right) {
        if (left == right) {
            return 0;
        }

    int mid = left + (right - left) / 2;
        int leftSum = mergeSort(arr, left, mid);
        int rightSum = mergeSort(arr, mid + 1, right);
        int mergeSum = merge(arr, left, mid, right);

        return leftSum + rightSum + mergeSum;
    }

    private static int merge(int[] arr, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left;
        int j = mid + 1;
        int k = 0;
        int sum = 0;

        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                sum += arr[i] * (right - j + 1);
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = arr[i++];
        }

        while (j <= right) {
            temp[k++] = arr[j++];
        }

        for (i = 0; i < temp.length; i++) {
            arr[left + i] = temp[i];
        }

        return sum;
    }

    public static int smallSum(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return 0;
        }

        return mergeSort(arr, 0, arr.length - 1);
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 4, 2, 5};
        int smallSum = smallSum(arr);
        System.out.println("小和为: " + smallSum);
    }
}
```
#### 逆序对问题

逆序对问题（Inverse Pairs Problem），逆序对是指数组中的两个元素，若前面的元素大于后面的元素，则称这两个元素构成一个逆序对。

merge时大到小排列，其余同上

---

## 快速排序（Selection Sort）

快速排序（Quicksort）是一种常用且高效的排序算法，它采用了分治的策略来对一个数组进行排序。该算法的基本思想是选择一个基准元素，将数组分为两个子数组，其中一个子数组的所有元素都小于基准元素，另一个子数组的所有元素都大于基准元素，然后对这两个子数组进行递归排序。

具体描述如下：

1. 选择基准元素：从数组中选择一个元素作为基准元素。通常情况下，可以选择数组的第一个元素、最后一个元素或者随机位置的元素作为基准元素。
2. 分区操作：将数组中的其他元素按照与基准元素的大小关系分为两部分。所有小于基准元素的元素放在基准元素的左边，所有大于基准元素的元素放在基准元素的右边。相同大小的元素可以放在任一边。
3. 递归排序：对基准元素左边的子数组和右边的子数组进行递归排序。重复上述步骤，直到子数组的长度为1或者为空。
4. 合并结果：将左边的子数组、基准元素和右边的子数组合并起来，形成有序的数组。

```java
public class QuickSort {
    public static void quickSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }
        
        quickSort(arr, 0, arr.length - 1);
    }

    private static void quickSort(int[] arr, int left, int right) {
        if (left >= right) {
            return;
        }
        
        int pivotIndex = partition(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }

    private static int partition(int[] arr, int left, int right) {
        int pivot = arr[right];
        int i = left - 1;

        for (int j = left; j < right; j++) {
            if (arr[j] <= pivot) {
                i++;
                swap(arr, i, j);
            }
        }

        swap(arr, i + 1, right);
        return i + 1;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] arr = {9, 5, 2, 7, 1, 8};
        System.out.println("Original array: " + Arrays.toString(arr));
        
        quickSort(arr);
        
       

```

若考虑相等情况，三个指针，首尾指向小于大于部分，一个指针遍历指导与大于指针相交（三路快速排序）

```java
 public class QuickSort {
    public void quickSort(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
        quickSortHelper(nums, 0, nums.length - 1);
    }
 private void quickSortHelper(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }
        int[] pivotIndices = partition(nums, left, right);
        quickSortHelper(nums, left, pivotIndices[0] - 1);
        quickSortHelper(nums, pivotIndices[1] + 1, right);
    }

    private int[] partition(int[] nums, int left, int right) {
        // 选择基准元素
        int pivot = nums[left];
        int lt = left;   // 小于基准元素的区域的右边界
        int gt = right;  // 大于基准元素的区域的左边界
        int i = left + 1;  // 当前遍历的元素

        while (i <= gt) {
            if (nums[i] < pivot) {
                // 将小于基准元素的元素放入小于区域
                swap(nums, i, lt);
                i++;
                lt++;
            } else if (nums[i] > pivot) {
                // 将大于基准元素的元素放入大于区域
                swap(nums, i, gt);
                gt--;
            } else {
                // 相等情况，直接跳过
                i++;
            }
        }

        return new int[]{lt, gt};
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
 }
```
#### 荷兰国旗问题

荷兰国旗问题（Dutch National Flag Problem）是一个经典的算法问题，它提出了一种对包含多个不同元素的数组进行原地排序的方法。

具体描述如下：给定一个包含 n 个元素的数组，数组中的元素只有三种可能的取值：0、1、2。问题的目标是将数组按照这三个值进行排序，使得所有的0都排在1的前面，而所有的1都排在2的前面。

---

## 堆排序（Heap Sort）

#### 二叉树

二叉树是一种有序树结构，它由一组节点组成，这些节点通过指向其他节点的链接连接在一起。每个节点最多有两个子节点，分别称为左子节点和右子节点。二叉树的一个重要特点是，左子节点小于或等于父节点，右子节点大于或等于父节点。这种特性使得在二叉搜索树中可以高效地进行查找、插入和删除操作。二叉树可以是空树（没有节点）或者由根节点和若干子树组成。

#### 堆

堆是一种特殊的二叉树，它满足堆属性（Heap Property）。堆树通常被用来实现优先队列等应用，其中最常见的是二叉堆。二叉堆是一种完全二叉树，它具有以下两个特性：

- 最大堆（Max Heap）：对于堆中的每个节点，节点的值大于或等于其子节点的值。
- 最小堆（Min Heap）：对于堆中的每个节点，节点的值小于或等于其子节点的值。

堆的主要操作是插入和删除操作，以及获取堆中的最大或最小值。在最大堆中，最大值总是位于堆的根节点（顶部），而在最小堆中，最小值位于根节点。这使得堆可以快速地找到最大或最小值，并在插入和删除操作后保持堆属性。

+ 角标计算（数组实现）： i位置 ->    
  + 左孩：2\*i+1   
  + 右孩：2\*i+2  
  + 父：(i-1)/2

##### 堆插入（Heap Insert）

 堆插入操作是向堆中添加一个新元素的过程。通常情况下，插入操作会在堆的最后一个位置添加新元素，并确保插入后仍满足堆的性质。具体的插入步骤如下： a. 将新元素添加到堆的最后一个位置（完全二叉树中的最后一个空缺位置）。 b. 将新元素与其父节点进行比较，如果新元素大于（或小于，根据堆的类型）父节点的值，则交换它们的位置。 c. 重复步骤b，直到新元素不再大于（或小于）父节点，或者新元素达到了堆的根节点。

堆插入操作的时间复杂度为O(log n)，其中n是堆中元素的数量。

##### 堆化（Heapify）

 堆化操作是将一个无序的数组或部分有序的数组转化为堆的过程。通常情况下，堆化操作用于构建一个初始堆，或者在堆的根节点发生改变后，恢复堆的性质。具体的堆化步骤如下： a. 从数组的倒数第二层开始，从右到左依次将每个节点作为根节点进行堆调整。 b. 在堆调整的过程中，将当前节点与其子节点进行比较，并根据堆的类型（最大堆或最小堆）进行交换，以确保当前节点满足堆的性质。 c. 重复步骤b，直到所有节点都满足堆的性质。

堆化操作的时间复杂度为O(n)，其中n是数组的长度。

```java
import java.util.Arrays;

public class Heap {
    private int[] heapArray;
    private int heapSize;
    
    public Heap(int capacity) {
        heapArray = new int[capacity];
        heapSize = 0;
    }

    // 堆插入操作
    public void insert(int value) {
        if (heapSize == heapArray.length) {
            // 堆已满
            throw new IllegalStateException("Heap is full.");
        }

        // 将新元素添加到堆的最后一个位置
        int currentIndex = heapSize;
        heapArray[currentIndex] = value;
        heapSize++;

        // 调整堆，保持堆的性质
        while (currentIndex > 0) {
            int parentIndex = (currentIndex - 1) / 2;
            if (heapArray[currentIndex] > heapArray[parentIndex]) {
                swap(currentIndex, parentIndex);
                currentIndex = parentIndex;
            } else {
                break;
            }
        }
    }

    // 堆化操作
    public void heapify() {
        int lastNonLeafNodeIndex = (heapSize / 2) - 1;

        // 从最后一个非叶子节点开始，向上进行堆调整
        for (int i = lastNonLeafNodeIndex; i >= 0; i--) {
            heapifyDown(i);
        }
    }

    // 向下堆化（从给定索引开始）
    private void heapifyDown(int index) {
        int largestIndex = index;
        int leftChildIndex = (2 * index) + 1;
        int rightChildIndex = (2 * index) + 2;

        // 找到当前节点、左子节点和右子节点中的最大值
        if (leftChildIndex < heapSize && heapArray[leftChildIndex] > heapArray[largestIndex]) {
            largestIndex = leftChildIndex;
        }
        if (rightChildIndex < heapSize && heapArray[rightChildIndex] > heapArray[largestIndex]) {
            largestIndex = rightChildIndex;
        }

        if (largestIndex != index) {
            swap(index, largestIndex);
            heapifyDown(largestIndex);
        }
    }

    // 交换数组中两个元素的位置
    private void swap(int i, int j) {
        int temp = heapArray[i];
        heapArray[i] = heapArray[j];
        heapArray[j] = temp;
    }

    // 打印堆中的元素
    public void printHeap() {
        System.out.println(Arrays.toString(heapArray));
    }

    public static void main(String[] args) {
        Heap heap = new Heap(10);
        heap.insert(4);
        heap.insert(8);
        heap.insert(2);
        heap.insert(10);
        heap.insert(6);

        System.out.println("Heap after insertions:");
        heap.printHeap();

        heap.heapify();

        System.out.println("Heap after heapify:");
        heap.printHeap();
    }
}
```
##### 完全二叉树（Complete Binary Tree）

除了最后一层可能不满外，其它层都是满的。最后一层从左到右可能缺少一些节点，但是节点的排列仍然保持从左到右的顺序。

          1
        /   \
       2     3
      / \   /
     4   5 6

高度：[log_2 N]+1

从上往下：O（N*log N）

从下往上：O（N）

#### 前缀树

前缀树（Prefix Tree），也称为字典树（Trie），是一种用于高效存储和查找字符串的树形数据结构。它的主要特点是将字符串按字符逐个存储在树中，并利用共享前缀的性质来节省空间并提高查找效率。

在前缀树中，每个节点代表一个字符，从根节点到叶子节点的路径表示一个完整的字符串。根节点不包含任何字符，每个非根节点都包含一个字符，并且每个节点可能有多个子节点，每个子节点代表一个可能的字符。通过节点之间的连接，可以构建出一棵表示字符串集合的树。

以下是前缀树的一些特点：

1. 共享前缀：前缀树中的节点可以共享相同的前缀，这使得前缀树能够高效地存储大量的字符串，并减少存储空间的需求。
2. 唯一路径：从根节点到任意叶子节点的路径唯一对应一个字符串。这意味着在前缀树中查找字符串时，可以沿着路径一步步匹配字符，快速找到目标字符串。
3. 查找效率：在前缀树中查找字符串的平均时间复杂度是O(k)，其中k是目标字符串的长度。由于每次查找只需比较一个字符，所以查找的效率非常高。

前缀树在许多字符串处理的应用中非常有用，例如单词搜索、字符串匹配、自动补全和拼写检查等。它还可以用于统计和分析文本数据，如计算单词频率或查找出现次数最多的单词等。

---

**不基于比较的排序：桶排序、基数排序、计数排序**

+ 时间复杂度O（N），额外空间复杂度O（M）
+ 应用范围有限，需要根据样本数据状况满足桶的划分

## 桶排序（Bucket Sort）

桶排序将待排序的元素分到不同的桶（区间段）中，每个桶中的元素再分别进行排序，最后将各个桶中的元素按顺序合并起来。

桶排序的步骤：

- 创建固定数量的桶。
- 将元素按照一定的映射规则分配到不同的桶中。
- 对每个桶中的元素进行排序（可以使用其他排序算法或递归调用桶排序）。
- 合并各个桶中的元素，得到最终排序结果。

桶排序适用于元素分布较为均匀的情况，例如在小范围整数排序或浮点数排序时。

---

## 计数排序（Counting Sort）

计数排序是一种用于排序非负整数的线性时间复杂度算法。

计数排序的步骤：

- 找出待排序数组中的最大值，确定计数数组的大小。
- 遍历待排序数组，统计每个元素出现的次数，并在计数数组中记录。
- 对计数数组进行顺序累加，以确定每个元素在排序结果中的位置。
- 遍历待排序数组，根据计数数组中的累加值，将元素放置到正确的位置上。

计数排序适用于元素范围较小、整数较多且重复较多的排序场景。

---

## 基数排序（Radix Sort）

基数排序是根据元素的位数来排序的，它将待排序的元素按照个位、十位、百位等位数进行排序。

基数排序的步骤：

- 从最低位开始，按照当前位数将元素分配到不同的桶中。
- 对每个位数上的桶中的元素进行排序（可以使用其他排序算法或递归调用基数排序）。
- 按照排序的位数顺序依次合并各个桶中的元素，得到最终排序结果。

基数排序适用于元素位数相同、位数较小的排序场景，例如整数或字符串的排序。

---

## 总结

下面是一个总结表格，展示了几种不同排序算法的时间复杂度、额外空间复杂度和稳定性。

| 排序算法 | 时间复杂度（平均情况） | 时间复杂度（最坏情况） | 额外空间复杂度 | 稳定性 |
| -------- | ---------------------- | ---------------------- | -------------- | ------ |
| 冒泡排序 | O(n^2)                 | O(n^2)                 | O(1)           | 稳定   |
| 选择排序 | O(n^2)                 | O(n^2)                 | O(1)           | 不稳定 |
| 插入排序 | O(n^2)                 | O(n^2)                 | O(1)           | 稳定   |
| 希尔排序 | O(n log n) ~ O(n^2)    | O(n^2)                 | O(1)           | 不稳定 |
| 归并排序 | O(n log n)             | O(n log n)             | O(n)           | 稳定   |
| 快速排序 | O(n log n)             | O(n^2)                 | O(log n)       | 不稳定 |
| 堆排序   | O(n log n)             | O(n log n)             | O(1)           | 不稳定 |
| 桶排序   | O(n + k)               | O(n^2)                 | O(n + k)       | 稳定   |
| 计数排序 | O(n + k)               | O(n + k)               | O(k)           | 稳定   |
| 基数排序 | O(k * n)               | O(k * n)               | O(n + k)       | 稳定   |

说明：

- 时间复杂度中，n 表示元素的数量，k 表示元素的取值范围。
- 额外空间复杂度指的是除了存储输入数据之外，算法运行过程中所需要的额外空间。
- 稳定性表示排序算法在相等元素的情况下，能否保持它们的相对顺序不变。

需要注意的是，表格中的时间复杂度和空间复杂度是一般情况下的估算值，实际的复杂度可能因具体实现方式、数据分布等因素而有所不同。此外，排序算法的选择应根据具体的排序需求和输入数据的特征来确定。

# 数据结构

单向链表、双向链表、栈、队列、数组、哈希表

### 哈希表

可分为（HashMap、HashSet），实际结构一回事

集合结构

增删改查：put, remove, put, get

放入东西，基础类型内部按值传递，内存占用为值大小；非基础内型按引用传递，内存占用8字节、

### 链表问题

#### 快慢指针

+ 快指针走两步，慢指针走一步

#### 回文结构(Palindrome)

指一个链表从前往后和从后往前读取值都是相同的结构。换句话说，如果将链表的值以正序和逆序分别读取出来，得到的序列是相同的。

给定单链表头节点，判断是否为回文结构：三个指针，反转后半部分链表

+ 快慢指针找到中点
+ 三个指针反转后半链表，前半段末尾指向null
+ 左右同时判断是否相等（结束标志： n1.next == null || n2.next == null ）

```java
class ListNode {
    int val;
    ListNode next;
	ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```


```java
public class PalindromeLinkedList {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
// 找到链表的中点
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 反转链表的后半部分
        ListNode prev = null;
        ListNode curr = slow.next;
        while (curr != null) {
            ListNode nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }
        slow.next = prev;

        // 比较前半部分和反转后的后半部分
        ListNode p1 = head;
        ListNode p2 = slow.next;
        while (p2 != null) {
            if (p1.val != p2.val) {
                return false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }

        return true;
    }
}
```
#### 按值划分

将链表按某值划分为左边小，中间相等，右边大形状

+ 笔试： 链表放数组，再做partition
+ 面试： 分成小中大三部分再串起来

#### 特殊赋值链表

node中增加属性node randn，可能为任意节点也可能为null

+ 笔试： 通过HashMap逐个复制，key为node，value为新node

  map.get(cur).next = map.get(cur.next);

  map.get(cur).rand = map.get(cur.randn);
  
+ 面试： 

  在每个节点后面插入空白新node，再通过next赋值

  node1.rand -> node3

  node1_.rand = node1.rand.next   (即node3\_)

  适用于节点属性和链表其他节点有相关性的题目

#### 链表有环

通过快慢指针，从头结点出发，在`fast == null || fast.next == null` 前相等则有环

若相遇（有环），将快指针重新放回起始节点，步长改为1，再次相遇为环头

#### 链表相交

两个可能有环的单链表，判断是否相交，若相交返回第一个相交节点，若不相交返回null

+ 判断是否有环

+ 链表1，2皆无环

  走到底，判断末尾是否相等，相等则相交
  
  找交点：用n = len1 - len2找到长度差，让长链表先走n步，相等时为交点

+ 一个有环，一个无环

  不可能相交

+ 两个都有环： 无交点 / 环前相交 / 环内相交
  + 判断环头，若相等，则环前相交，用无环法找交点
  + 若环头不相等，沿其中一个环遍历一圈，若没有相遇，则不相交；若相遇，则任意返回一个环头



---





# 二叉树

## 先序、中序、后序遍历

取决于递归过程中打印位置（   先序 || f(left)  || 中序  ||  f(right)  ||  后序）

`if(head == null)  return;`

---

```markdown
    1
   / \
  2   3
 / \
4   5
```

这棵二叉树的先序遍历结果为：1 2 4 5 3 

这棵二叉树的中序遍历结果为：4 2 5 1 3 

这棵二叉树的后序遍历结果为：4 5 2 3 1

---

+ 先序遍历（Pre-order Traversal）
+ 中序遍历（In-order Traversal）
+ 后序遍历（Post-order Traversal）

非递归实现（栈）： 弹打EP，有右压右，有左压左

---

## 按层遍历（宽度优先遍历）

BFS，Breadth-First Search

从根节点开始，逐层访问二叉树的节点，先访问根节点，然后按照从左到右的顺序访问同一层的节点，直到遍历完所有节点。

### 队列实现

+ 加入头，开始重复弹出头，加入左右子节点

+ 区分层，并返回最宽层节点数

  + 头节点校验： if(head == null)  return 0; 

    将头节点加入队列 `queue.add(head)`

  + 设置当前层最右 `curEnd=head`，下一层最右`nextEnd=null`，当前层节点数 `curLevelNodes=0`，最宽层节点数

  + 队列非空的话，弹出首个节点`curNode`。

    + 判断左节点是否存在，存在加入队列，`nextEnd`更新为左节点
    + 判断右节点是否存在，存在加入队列，`nextEnd`更新为右节点
    + `curLevelNodes++`
    + 判断当前节点是否等于`curEnd`，若相等，此层结束，max更新为`max（max，curLevelNodes）`，`curEnd`更新为`nextEnd`， `curLevelNodes`重置为0
  
+ 或者用hash表判断层数

  + `HashMap <Node, Integer>  levelMap = new HashMap<>();`
  + 放入左右节点时
    + `levelMap.put(cur.left, curLevel + 1)`
    +  `levelMap.put(cur.right, curLevel + 1)`
  + 判断末尾
    + 比较`curNodeLevel`和`curLevel`，相等`curLevelNodes`++，否则新层，`max`更新，`curLevel++`，`curLeelNoes=1`

## 序列化和反序列化

首先按先序遍历、中序遍历、后序遍历、按层遍历，实现二叉树序列化

对应方法反序列化，用null填补空节点