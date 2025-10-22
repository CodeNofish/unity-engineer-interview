给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。


```C#
public class Solution {
    private Random _random = new Random();

    public int FindKthLargest(int[] nums, int k) {
        return QuickSelect(nums, 0, nums.Length - 1, nums.Length - k);
    }

    public int QuickSelect(int[] array, int left, int right, int targetIndex) {
        int povitIndex = RandomPartition(array, left, right);
        // 锚点是确定好的解，每次快排后锚点都不会动。
        if (povitIndex == targetIndex) {
            return array[povitIndex];
        } else {
            if (povitIndex < targetIndex) {
                return QuickSelect(array, povitIndex + 1, right, targetIndex);
            } else {
                return QuickSelect(array, left, povitIndex - 1, targetIndex);
            }
        }
    }

    public int RandomPartition(int[] array, int left, int right) {
        // 随机选择锚点
        int randomIndex = _random.Next(left, right + 1);
        Swap(array, randomIndex, right);
        return Partition(array, left, right);
    }

    public int Partition(int[] array, int left, int right) {
        int povit = array[right];
        int border = left - 1; // 指向最后一个小于基点的元素
        // 扩展边界，border不可能超过遍历的索引i
        for (int i = left; i < right; i++) {
            if (array[i] <= povit) {
                border++;
                Swap(array, border, i);
            }
        }
        border++;
        Swap(array, border, right);
        return border;
    }

    public void Swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```