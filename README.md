```cpp
void test(){
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(0);
  std::cout.tie(0);
}
```
## Bin Search
```cpp
int BinSearch(int* a, int l, int r, int x) {
  int middle = (l + r) / 2;
  if (x < a[middle]) {
    if (middle != r) {
      return BinSearch(a, l, middle, x);
    }
    return -1;
  } else if (a[middle] < x) {
    if (middle != l) {
      return BinSearch(a, middle, r, x);
    }
    return -1;
  } else if (x == a[middle]) {
    return middle + 1;
  } else {
    return -1;
  }
}
```

# Sorts

## N^2 

```cpp
void Swap(int& a, int& b) {
    int c = a;
    a = b;
    b = c;
  }
```

### 1.  Selection sort
```cpp
  void SelectionSort(int* array, int n) {
    for (int i = 0; i < n - 1; i++) {
      for (int j = i + 1; j < n; j++) {
        if (array[j] < array[i]) {
          Swap(array[j], array[i]);
        } } } }
```

### 2. Insection Sort
```cpp
  void InsectionSort(int* array, int n) {
    int j;
    for (int i = 0; i < n; i++) {
      j = i + 1;
      while (j > 0 && array[j - 1] > array[i]) {
        Swap(array[j - 1], array[i]);
        j--;
      }  } }
```

### 3. Bubble sort
```cpp
  void BubbleSort(int* array, int n) {
    for (int i = 0; i < n - 1; i++) {
      for (int j = 0; j < n - 1 - i; j++) {
        if (array[j + 1] < array[j]) {
          Swap(array[j + 1], array[j]);
        }  }  }  }
```

## N

```cpp
void Swap(int& a, int& b) {
    int c = a;
    a = b;
    b = c;
  } 
```

### 1. Merge Sort

```cpp
  void Merge(int* a, int n, int* b, int m, int* c) {
    if (c != nullptr) {
      int i = 0, j = 0;
      while (i < n && j < m) {
        if (a[i] > b[j]) {
          c[i + j] = a[i];
          i++;
        } else {
          c[i + j] = b[j];
          j++;
        } }
      while (i < n) {
        c[i + j] = a[i];
        i++;
      }
      while (j < m) {
        c[i + j] = b[j];
        i++;
      }  }   }
```

```cpp
  void MergeSort(int* a, int n, int* buff = nullptr) {
    if (n <= 1) { return;  }
    if (n == 2) {
      if (a[0] > a[1]) {
        Swap(a[0], a[1]);
      } 
	  return; }

    bool is_mem = false;
    if (buff = nullptr) {
      buff = new int[n];
      is_mem = true;
    }
    int m = n / 2;
    MergeSort(a, m, buff);
    MergeSort(a + m, n - m, buff);
    Merge(a, m, a + m, n - m, buff);

    if (buff != nullptr) {
      for (int i = 0; i < n; i++) {
        a[i] = buff[i];
      }
    }
    if (is_mem) {
      delete[] buff;
    }
  }
```

# NLog(N)

```cpp
void Swap(int& a, int& b) {
    int c = a;
    a = b;
    b = c;
  } 
```

### 1.1. Q Hoar

```cpp
  int Partition_Hoar(int* a, int n, int pivot) {
    int i = -1;
    int j = n;
    while (true) {
      do {
        i++;
      } while (a[i] < a[pivot]);
      do {
        j--;
      } while (a[j] > a[pivot]);
      if (i >= j) {  return j + 1; }
      Swap(a[i], a[j]);
    } } 
```

### 1.2. Q Lomuto
```cpp
  int Partition_Lomuto(int* a, int n) {
    int pivot = n;
    int i = -1;
    for (int j = 0; j < n; j++) {
      if (a[j] <= a[pivot]) {
        i++;
        if (i != j) {
          Swap(a[i], a[j]);
        } } }
    return i; } 
```

#### Quick
```cpp
  void QuickSort(int* a, int n) {
    if (n == 1) {  return; }
    int pivot = n / 2;
    int i = Partition_Hoar(a, n, pivot);
    QuickSort(a, i);
    QuickSort(a + i, n - i);
  }
```

### 2. Counting Sort
```cpp
  void CountingSort(int* arr, int n, int min, int max) {
    int k = max - min + 1;
    int* a = new int[k] {};
    int* b = new int[k] {};
    for (int i = 0; i < n; i++) {
      a[arr[i] - min]++;
    }
    for (int i = 1; i < k; i++) {
      a[i] = a[i] + a[i - 1];
    }
    for (int j = n - 1; j >= 0; j--) {
      b[a[arr[j] - min] - 1] = arr[j];
      a[arr[j] - min]--;
    }
    delete[] a;
    for (int i = 0; i < n; i++) {  // в исходный массив, чтоб без return
      arr[i] = b[i];
    }
    delete[] b;  } 
```

# BackPack
### Кража 
```cpp
  void Craja() {
    int max_weight;
    int n;
    std::cin >> max_weight >> n;

    auto* weights = new int[n];
    auto* costs = new int[n];

    auto* max_costs = new int[max_weight + 1] {0};
    auto* max_weights = new int[max_weight + 1] {0};

    for (int i = 0; i < n; i++) {
      std::cin >> costs[i] >> weights[i];
    }
    for (int i = 0; i < n; i++) {
      for (int w = max_weight; w >= weights[i]; w--) {
        int a = max_costs[w];
        int b = max_costs[w - weights[i]] + costs[i];
        if (a >= b) {
          max_costs[w] = a;
        } else {
          max_costs[w] = b;
          max_weights[w] = max_weights[w - weights[i]] + weights[i]; } } }

    int final_cost = 0, final_weight = 0;
    for (int w = max_weight; w >= 0; w--) {
      if (final_cost < max_costs[w]) {
        final_cost = max_costs[w];
        final_weight = max_weights[w];
      }
    }
    std::cout << final_cost << " " << final_weight;

    delete[] weights;
    delete[] costs;
    delete[] max_costs;
    delete[] max_weights;
  } 
```

# ATD
1. list
### 2. Heap
 ```cpp
 void Swap(int& a, int& b) {
    int c = a;
    a = b;
    b = c;
  } 
```
```cpp
  void SiftUp(int* a, int i, int& index_added) {
    while (i > 0) {
      int p_i = (i - 1) / 2;
      if (a[p_i] >= a[i]) {
        break;
      }
      Swap(a[p_i], a[i]);
      i = p_i;
      index_added = i + 1;
    }  }

  void SiftDown(int* a, int i, int size) {
    while (i < size) {
      int left_i = i * 2 + 1;
      int right_i = i * 2 + 2;
      int max = i;
      if (left_i < size && a[left_i] > a[max]) {
        max = left_i;
      }
      if (right_i < size && a[right_i] > a[max]) {
        max = right_i;
      }
      if (i != max) {
        Swap(a[i], a[max]);
        i = max;
      } else {
        break;
      }  } }

  void ExtractMax(int* a, int& size) {
    if (size == 1) {
      std::cout << "0 " << a[0] << "\n";
      size--;
    } else {
      Swap(a[0], a[size - 1]);
      int index_ex_last = 0;
      size--;
      if (size > 0) {
        SiftDown(a, 0, size);  // ", index_ex_last"
      }
      std::cout << (index_ex_last + 1) << " " << a[size] << "\n";
    } }

  void Resize(int*& a, int& capacity) {
    auto* b = new int[2 * capacity + 1];
    for (int i = 0; i < capacity; i++) {
      b[i] = a[i];
    }
    delete[] a;
    a = b;
    capacity = 2 * capacity + 1;
  }

  void Push(int*& a, int& size, int& capacity, int x) {
    if (size == capacity) {
      Resize(a, capacity);
    }
    a[size] = x;
    size++;
    int index_added = size;
    SiftUp(a, size - 1, index_added);
    std::cout << (index_added) << "\n";
  }

  void BuildHeap(int* a, int n) {
    for (int i = n / 2; i >= 0; i--) {
      SiftDown(a, i, n);
    } }

  void HeapSort(int* a, int n) {
    int size = n;
    for (int i = 0; i < n - 1; i++) {  // кроме последнего
      Swap(a[0], a[n - 1 - i]);        // первый = max => свап с последним
      size--;
      SiftDown(a, 0, size);
    } } 
```

# Tree
### 1. BinarySearchTree

```cpp
struct Node {
    int value;
    Node* left = nullptr;
    Node* right = nullptr;
    Node* parent = nullptr;
  }; 
```
private:
```cpp
  Node* root_ = nullptr;

  void Clear(Node* node = nullptr) {
    if (node == nullptr) {
      return Clear(root_);
    }
    Node* left = node->left;
    Node* right = node->right;
    delete node;
    if (left) {
      Clear(left);
    }
    if (right) {
      Clear(right);
    } } 
```
public:
```cpp
  bool Push(int x, Node* node = nullptr) {
    if (root_ == nullptr) {
      auto* new_node = new Node;
      new_node->value = x;
      root_ = new_node;
      return true;
    }
    if (node == nullptr) {
      return Push(x, root_);
    }
    if (x == node->value) {
      return false;
    }
    if (x > node->value) {
      if (node->right == nullptr) {
        auto* new_node = new Node;
        new_node->value = x;
        new_node->parent = node;
        node->right = new_node;
      }
      return Push(x, node->right);
    }
    if (x < node->value) {
      if (node->left == nullptr) {
        auto* new_node = new Node;
        new_node->value = x;
        new_node->parent = node;
        node->left = new_node;
      }
      return Push(x, node->left);
    }
    return true; }

  void Print(Node* node = nullptr) {
    if (node == nullptr) { return Print(root_); }
    if (node->left) { Print(node->left); }
    std::cout << node->value << "\n";
    if (node->right) {
      Print(node->right);
    } }

  ~BinarySearchTree() { Clear(root_); }
```

### 2. Decartovoe
```cpp
    Node() {
      y = rand();
    }
    explicit Node(int x) {
      y = rand();
      key = x;
    } 
```

private:
```cpp
  Node* root_ = nullptr;

  void Clear(Node* node = nullptr) {
    if (node == nullptr) {
      return Clear(root_);
    }
    Node* left = node->left;
    Node* right = node->right;
    delete node;
    if (left) {
      Clear(left);
    }
    if (right) {
      Clear(right);
    }
  }

  pair<Node*, Node*> Split(Node* t, int key) {
    if (t == nullptr) {
      return {nullptr, nullptr};
    }
    if (t->key < key) {
      auto t_splitted = Split(t->right, key);
      t->right = t_splitted.first;
      return {t, t_splitted.second};
    }
    auto t_splitted = Split(t->left, key);
    t->left = t_splitted.second;
    return {t_splitted.first, t};
  }

  Node* Merge(Node* t1, Node* t2) {
    if (t1 == nullptr) {
      return t2;
    }
    if (t2 == nullptr) {
      return t1;
    }
    if (t1->y >= t2->y) {
      t1->right = Merge(t1->right, t2);
      return t1;
    }
    t2->left = Merge(t1, t2->left);
    return t2;
  }

  Node* Insert(Node* t, int key) {
    auto t_splitted = Split(t, key);
    auto new_node = new Node(key);
    return Merge(Merge(t_splitted.first, new_node), t_splitted.second);
  }

  void Remove(Node* t, int key, Node* parent = nullptr) {
    if (t == nullptr) {
      return;
    }
    if (parent == nullptr) {
      parent = t;
    }
    if (key < t->key) {
      return Remove(t->left, key, t);
    }
    if (key > t->key) {
      return Remove(t->right, key, t);
    }
    if (parent->right && parent->right->key == key) {
      parent->right = Merge(t->left, t->right);
      delete t;
    } else if (parent->left && parent->left->key == key) {
      parent->left = Merge(t->left, t->right);
      delete t;
    }
  }

  bool Find(Node* t, int key) {
    if (t == nullptr) {
      return false;
    }
    if (key == t->key) {
      return true;
    }
    if (key < t->key) {
      if (t->left == nullptr) {
        return false;
      }
      return Find(t->left, key);
    }
    if (key > t->key) {
      if (t->right == nullptr) {
        return false;
      }
      return Find(t->right, key);
    }
    return false;  // for CE
  }
```
public:
```cpp
  int GetMin() {
    if (root_) {
      Node* runner = root_;
      while (runner->left) {
        runner = runner->left;
      }
      return runner->key;
    }
    return 0; }

  int GetMax() {
    if (root_) {
      Node* runner = root_;
      while (runner->right) {
        runner = runner->right;
      }
      return runner->key;
    }
    return 0; }

  bool Find(int key) {  return Find(root_, key); }

  void Push(int key) {  root_ = Insert(root_, key); }

  void Pop(int key) {  Remove(root_, key); }

  ~Decartovoe() {  Clear(root_);  } 
```

### AVL
```cpp
    explicit Node(int x) {
      key = x;
      height = 1;
    } 
```
private:
```cpp
  Node* root_ = nullptr;

  void Clear(Node* node) {
    if (node == nullptr) {
      return;
    }
    Node* left = node->left;
    Node* right = node->right;
    delete node;
    if (left) {
      Clear(left);
    }
    if (right) {
      Clear(right);
    } }

  int Height(Node* node) {
    if (node == nullptr) { return 0; }
    return node->height;
  }

  void FixHeight(Node* node) {
    int height_l = Height(node->left);
    int height_r = Height(node->right);
    node->height = 1 + (height_l > height_r ? height_l : height_r);
  }

  int BFactor(Node* node) {
    return Height(node->left) - Height(node->right);
  }

  Node* Balance(Node* node) {
    FixHeight(node);
    int b_factor = BFactor(node);
    if (b_factor == -2) {  // left
      if (BFactor(node->right) <= 0) {
        return LeftRotate(node);
      }
      return BigLeftRotate(node);
    }
    if (b_factor == 2) {  // right
      if (BFactor(node->left) >= 0) {
        return RightRotate(node);
      }
      return BigRightRotate(node);
    }
    return node; } 
```
Rotate
```cpp
  Node* LeftRotate(Node* a) {
    Node* c = a->right->left;
    Node* b = a->right;

    b->left = a;
    a->right = c;

    FixHeight(b->left);
    FixHeight(b);
    return b; }

  Node* RightRotate(Node* a) {
    Node* c = a->left->right;
    Node* b = a->left;

    b->right = a;
    a->left = c;

    FixHeight(b->right);
    FixHeight(b);
    return b; } 
```
Big Rotate
```cpp
  Node* BigLeftRotate(Node* a) {
    Node* b = a->right;
    Node* c = b->left;
    Node* m = c->left;
    Node* n = c->right;

    a->right = m;
    b->left = n;
    c->left = a;
    c->right = b;

    FixHeight(a);
    FixHeight(b);
    FixHeight(c);
    return c; }

  Node* BigRightRotate(Node* a) {
    Node* b = a->left;
    Node* c = b->right;
    Node* m = c->left;
    Node* n = c->right;

    a->left = n;
    b->right = m;
    c->right = a;
    c->left = b;

    FixHeight(a);
    FixHeight(b);
    FixHeight(c);
    return c;  } 
```
```cpp
  Node* Insert(Node* node, int key) {
    if (node == nullptr) {
      auto node = new Node(key);
      return node;
    }
    if (key < node->key) {
      node->left = Insert(node->left, key);
    }
    if (key > node->key) {
      node->right = Insert(node->right, key);
    }
    if (key == node->key) { /* nothing */ }
    return Balance(node);
  }

  pair<int, Node*> RemoveMin(Node* node) {
    if (node->left == nullptr) {
      Node* t = node->right;
      int m = node->key;
      delete node;
      return {m, t};
    }
    auto p = RemoveMin(node->left);
    node->left = p.second;
    return {p.first, Balance(node)};
  }

  Node* Remove(Node* node, int x) {
    if (node == nullptr) {
      return nullptr;
    }
    if (node->key == x) {
      if (node->right == nullptr) {
        Node* t = node->left;
        delete node;
        return t;
      }
      auto p = RemoveMin(node->right);
      node->right = p.second;
      node->key = p.first;
      return Balance(node);
    }
    if (x < node->key) {
      node->left = Remove(node->left, x);
    } else {
      node->right = Remove(node->right, x);
    }
    return Balance(node);
  }

  void Print(Node* node) {
    if (node == nullptr) {
      return;
    }
    if (node->left) {
      Print(node->left);
    }
    std::cout << node->key << "\n";
    if (node->right) {
      Print(node->right);
    }  } 
```
public:
```cpp
  void Insert(int key) {  root_ = Insert(root_, key); }

  void Remove(int key) { root_ = Remove(root_, key);  }

  void Print() { Print(root_);  }

  void Clear() { Clear(root_);   }

  ~AVLTree() { Clear(); } 
```


