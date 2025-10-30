

```c#

namespace AlgorithmPlayground;

// 处理 不相交集合(DisjointSet) 的数据结构
// 查找（Find）：确定元素属于哪个集合
// 合并（Union）：将两个集合合并为一个集合
public class UnionFind {
  // 存储每个元素的父节点
  private readonly int[] parent;

  // 每个集合的秩（树的高度）
  private readonly int[] height;

  // 每个集合的数量
  private readonly int[] count;

  public UnionFind(int size) {
    parent = new int[size];
    height = new int[size];
    count = new int[size];

    // 初始化：每个元素都是自己的父节点
    for (int i = 0; i < size; i++) {
      parent[i] = i;
      height[i] = 0;
      count[i] = 1;
    }
  }

  // 查找操作 返回根元素
  public int Find(int x) {
    while (x != parent[x]) {
      // parent[x] = parent[parent[x]];
      x = parent[x];
    }

    return x;
  }

  // 合并操作（按秩合并）
  public void Union(int x, int y) {
    int rootX = Find(x), rootY = Find(y);
    // same set
    if (rootX == rootY) return;

    // 秩优化后 可以通过大小 判断谁该合并给谁
    if (height[rootX] < height[rootY]) {
      parent[rootX] = rootY;
      count[rootY] += count[rootX];
      // height use rootY
    }
    else if (height[rootX] > height[rootY]) {
      parent[rootY] = rootX;
      count[rootX] += count[rootY];
      // height use rootX
    }
    else {
      parent[rootY] = rootX;
      count[rootX] += count[rootY];
      height[rootX]++;
    }
  }

  // 检查两个元素是否连通
  public bool IsConnected(int x, int y) {
    return Find(x) == Find(y);
  }

  // 获取x所在集合的大小
  public int GetCount(int x) {
    return count[Find(x)];
  }

  // 获取连通分量数量
  public int GetDisjointSetCount() {
    int cnt = 0;
    for (int i = 0; i < parent.Length; i++) {
      if (i == Find(i)) {
        cnt++;
      }
    }

    return cnt;
  }
}

```