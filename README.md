# Dynamic Programming


>## 1. 0/1 Knapsack Recursive :
```c++
#include <bits/stdc++.h>
using namespace std;

int knapSack(int wt[], int val[], int w, int n) {
    if (w == 0 || n == 0)
        return 0;

    if (wt[n - 1] > w)
        return knapSack(wt, val, w, n - 1);
    else
        return max(val[n - 1] + knapSack(wt, val, w - wt[n - 1], n - 1), knapSack(wt, val, w, n - 1));
}
int main(){   

    int val[] = { 60, 100, 120 };
    int wt[] = { 10, 20, 30 };
    int w = 50;
    int n = sizeof(val) / sizeof(val[0]);

    cout << knapSack(wt, val, w, n);
    return 0;
}
```

>## 2. 0/1 Knapsack Memoization :
``` c++
#include <bits/stdc++.h>
using namespace std;


int dp[1000][1000];

int knapSack(int wt[], int val[], int w,  int n) {
    if (w == 0 || n == 0)
        return 0;

    if (dp[n][w] != -1)
        return dp[n][w];

    if (wt[n - 1] > w)
        return dp[n][w] = knapSack(wt, val, w, n - 1);
    else
        return dp[n][w] = max(val[n - 1] + knapSack( wt, val, w - wt[n - 1], n - 1), knapSack( wt, val, w, n - 1));
}

int main()
{

    int val[] = { 60, 100, 120 };
    int wt[] = { 10, 20, 30 };
    int w = 50;
    int n = sizeof(val) / sizeof(val[0]);

    for (int i = 0; i <= n; i++)
        for (int j = 0; j <= w; j++)
            dp[i][j] = -1;

    cout << knapSack(wt, val, w, n);
    return 0;
}

```
>## 3. 01 Knapsack Top Down DP :

```c++
#include <bits/stdc++.h>
using namespace std;


int main(){ 

    int val[] = { 60, 100, 120 };
    int wt[] = { 10, 20, 30 };
    int w = 50;
    int n = sizeof(val) / sizeof(val[0]);

    int dp[n + 1][w + 1];

    for (int i = 0; i <= n; i++)
        for (int j = 0; j <= w; j++)
            if (i == 0 or j == 0)
                dp[i][j] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= w; j++) {
            if (wt[i - 1] <= j)
                dp[i][j] = max(val[i - 1] + dp[i - 1][j - wt[i - 1]], dp[i - 1][j]);
            else
                dp[i][j] = dp[i - 1][j];
        }
    }

    cout << dp[n][w] << endl;
    return 0;
}
```
>#  4. SubsetSum :

```c++
#include <bits/stdc++.h>
using namespace std;


bool isSubsetSum(vector<int>& arr, int n, int sum) {
        bool dp[n + 1][sum + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= sum; j++) {
                if (i == 0)
                    dp[i][j] = false;
                if (j == 0)
                    dp[i][j] = true;
            }
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                if (arr[i-1] <= j)
                    dp[i][j] = dp[i-1][j - arr[i - 1]] || dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][sum];
    }

int main()
{   freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);

    int set[] = { 3, 34, 4, 12, 5, 2 };
    int sum = 9;
    int n = sizeof(set) / sizeof(set[0]);
    
    if (isSubsetSum(set, n, sum) == true)
        printf("Found a subset with given sum");
    else
        printf("No subset with given sum");
    return 0;
}
```

## 5. Equal Sum Partition:

```c++
#include <bits/stdc++.h>
using namespace std;


bool isSubsetSum(vector<int>& arr, int n, int sum) {
        bool dp[n + 1][sum + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= sum; j++) {
                if (i == 0)
                    dp[i][j] = false;
                if (j == 0)
                    dp[i][j] = true;
            }
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                if (arr[i-1] <= j)
                    dp[i][j] = dp[i-1][j - arr[i - 1]] || dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][sum];
    }


bool findPartiion(int arr[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];

    if (sum % 2 != 0)
        return false;

    if (isSubsetSum(arr, n, sum / 2))
        return true;
    return false;
}


int main()
{   
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);

    int arr[] = { 3, 1, 5, 9, 12 };
    int n = sizeof(arr) / sizeof(arr[0]);

    if (findPartiion(arr, n) == true)
        cout << "YES";
    else
        cout << "NO";
    return 0;
}

```

## 6. Count of Subsets Sum with a Given Sum :

```c++
#include <bits/stdc++.h>
using namespace std;


int findCnt(int arr[], int n, int sum) {
    int dp[n + 1][sum + 1];

    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= sum; j++) {
            if (i == 0)
                dp[i][j] = 0;
            if (j == 0)
                dp[i][j] = 1;
        }
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= sum; j++) {
            if (arr[i - 1] <= j)
                dp[i][j] = dp[i - 1][j - arr[i - 1]] + dp[i - 1][j];
            else
                dp[i][j] = dp[i - 1][j];
        }
    }
    return dp[n][sum];
}


int main()
{
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);

    int arr[] = { 3, 3, 3, 3 };
    int n = 4;
    int sum = 6;

    cout << findCnt(arr, n, sum);
    return 0;
}

```