
//Problem 1 — Median via Selection Sort Invariant

int find_median(vector<int>& arr) {
    int size = arr.size();
    int target_index = size / 2;
    for (int i = 0; i <= target_index; i++) {
        int min_idx = i;
        for (int j = i + 1; j < size; j++) {
            if (arr[j] < arr[min_idx])
                min_idx = j;
        }
        swap(arr[i], arr[min_idx]);
    }
    return arr[target_index];
}


//Problem 2 — K-th Largest via Bubble Sort Invariant


int find_kth_largest(vector<int>& arr, int k) {
    int size = arr.size();
    for (int i = 0; i < k; i++) {
        for (int j = 0; j < size - 1 - i; j++) {
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);
        }
    }
    return arr[size - k];
}


// Problem 3 — Merge Two Arrays via Insertion Sort Logic

vector<int> merge_sorted(vector<int>& a, vector<int>& b) {
    int n = a.size(), m = b.size();
    vector<int> out(n + m);
    int k = 0;
    auto insert_key = [&](int key) {
        int j = k - 1;
        while (j >= 0 && out[j] > key) {
            out[j + 1] = out[j];
            j--;
        }
        out[j + 1] = key;
        k++;
    };
    for (int x : a) insert_key(x);
    for (int x : b) insert_key(x);
    return out;
}

/ Problem 4.1 — Factorial

long long factorial(int n) {
    long long f = 1;
    for (int i = 1; i <= n; i++) f *= i;
    return f;
}
 

// Problem 4.2 — Fibonacci (recursive)

long long fibonacci(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
 

// Problem 4.3 — Count Digits

int count_digits(int n) {
    if (n < 10) return 1;
    return 1 + count_digits(n / 10);
}
 

// Problem 4.4 — Power Function

double power(double base, int exp) {
    if (exp == 0) return 1;
    return base * power(base, exp - 1);
}
 
// Problem 4.5 — Palindrome Check

bool is_palindrome(const string& str, int left, int right) {
    if (left >= right) return true;
    if (str[left] != str[right]) return false;
    return is_palindrome(str, left + 1, right - 1);
}
 

// Problem 4.6 — Sum of Array
int array_sum(vector<int>& arr, int size) {
    if (size == 1) return arr[0];
    return arr[size - 1] + array_sum(arr, size - 1);
}
 

// Problem 4.7 — Maximum of Array

int array_max(vector<int>& arr, int size) {
    if (size == 1) return arr[0];
    int mx = array_max(arr, size - 1);
    return (arr[size - 1] > mx) ? arr[size - 1] : mx;
}
 

// Problem 4.8 — Binary Search

int binary_search(vector<int>& arr, int left, int right, int target) {
    if (left > right) return -1;
    int mid = (left + right) / 2;
    if (arr[mid] == target) return mid;
    if (arr[mid] > target) return binary_search(arr, left, mid - 1, target);
    return binary_search(arr, mid + 1, right, target);
}
 

// Problem 5 — Merge Sort

void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> L(arr.begin() + left, arr.begin() + mid + 1);
    vector<int> R(arr.begin() + mid + 1, arr.begin() + right + 1);
    int i = 0, j = 0, k = left;
    while (i < (int)L.size() && j < (int)R.size()) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else               arr[k++] = R[j++];
    }
    while (i < (int)L.size()) arr[k++] = L[i++];
    while (j < (int)R.size()) arr[k++] = R[j++];
}
 
void merge_sort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        merge_sort(arr, left, mid);
        merge_sort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
 

// Problem 6 — Quick Sort (Lomuto)

int total_swaps = 0;
 
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            swap(arr[++i], arr[j]);
            total_swaps++;
        }
    }
    swap(arr[i + 1], arr[high]);
    total_swaps++;
    return i + 1;
}
 
void quick_sort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quick_sort(arr, low, pi - 1);
        quick_sort(arr, pi + 1, high);
    }
}
 

// Problem 7 — Fractional Knapsack

struct Item { int weight, value; };
 
double fractional_knapsack(vector<Item>& items, int capacity) {
    sort(items.begin(), items.end(), [](const Item& a, const Item& b) {
        return (double)a.value / a.weight > (double)b.value / b.weight;
    });
    double total = 0.0;
    int cur_w = 0;
    for (auto& item : items) {
        if (cur_w + item.weight <= capacity) {
            cur_w += item.weight;
            total += item.value;
        } else {
            int rem = capacity - cur_w;
            total += item.value * ((double)rem / item.weight);
            break;
        }
    }
    return total;
}
 

// Problem 9a — Dijkstra's Algorithm

void dijkstra(vector<vector<int>>& graph, int src) {
    int V = graph.size();
    vector<int> dist(V, INT_MAX);
    vector<bool> visited(V, false);
    dist[src] = 0;
    for (int count = 0; count < V - 1; count++) {
        int u = -1;
        for (int v = 0; v < V; v++)
            if (!visited[v] && (u == -1 || dist[v] < dist[u])) u = v;
        visited[u] = true;
        for (int v = 0; v < V; v++) {
            if (!visited[v] && graph[u][v] && dist[u] != INT_MAX
                && dist[u] + graph[u][v] < dist[v])
                dist[v] = dist[u] + graph[u][v];
        }
    }
    cout << "Vertex\tDistance from Source\n";
    for (int i = 0; i < V; i++)
        cout << i << "\t" << dist[i] << "\n";
}
 

// Problem 9b — Bellman-Ford Algorithm

struct Edge { int src, dest, weight; };
 
void bellman_ford(int V, vector<Edge>& edges, int src) {
    vector<int> dist(V, INT_MAX);
    dist[src] = 0;
    for (int i = 1; i <= V - 1; i++) {
        for (auto& e : edges) {
            if (dist[e.src] != INT_MAX && dist[e.src] + e.weight < dist[e.dest])
                dist[e.dest] = dist[e.src] + e.weight;
        }
    }
    for (auto& e : edges) {
        if (dist[e.src] != INT_MAX && dist[e.src] + e.weight < dist[e.dest]) {
            cout << "Graph contains a negative-weight cycle\n";
            return;
        }
    }
    cout << "Vertex\tDistance from Source\n";
    for (int i = 0; i < V; i++)
        cout << i << "\t" << dist[i] << "\n";
}
 
// Problem 10a — 0/1 Knapsack

int knapsack_01(vector<int>& weights, vector<int>& values, int W) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (weights[i-1] <= w)
                dp[i][w] = max(values[i-1] + dp[i-1][w - weights[i-1]], dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];
        }
    }
    return dp[n][W];
}
 

// Problem 10b — Minimum Number of Coins

int min_coins(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, INT_MAX);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int c : coins) {
            if (c <= i && dp[i - c] != INT_MAX)
                dp[i] = min(dp[i], dp[i - c] + 1);
        }
    }
    return (dp[amount] == INT_MAX) ? -1 : dp[amount];
}
 
/ Problem 10c — Count Ways (Coin Combinations)

int count_ways(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, 0);
    dp[0] = 1;
    for (int c : coins)
        for (int j = c; j <= amount; j++)
            dp[j] += dp[j - c];
    return dp[amount];
}
 

// Problem 10d — Longest Common Subsequence

int lcs(const string& s1, const string& s2) {
    int m = s1.size(), n = s2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else                     dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    return dp[m][n];
}
 

// Problem 10e — Longest Increasing Subsequence

int lis(vector<int>& arr) {
    int n = arr.size();
    if (n == 0) return 0;
    vector<int> dp(n, 1);
    int result = 1;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++)
            if (arr[j] < arr[i]) dp[i] = max(dp[i], dp[j] + 1);
        result = max(result, dp[i]);
    }
    return result;
}
 
