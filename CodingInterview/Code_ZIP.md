# 자주 사용되는 코드 모음

## 목차
- [문자열](#문자열)
    - [문자열 뒤집기](#문자열-뒤집기)
    - [Palindromic 문자열 판별](#palindromic-문자열-판별)
- [자료구조 및 알고리즘](#자료구조-및-알고리즘)
    - [버블 정렬](#버블-정렬)
    - [선택 정렬](#선택-정렬)
    - [삽입 정렬](#삽입-정렬)
    - [합병 정렬](#합병-정렬)
    - [퀵 정렬](#퀵-정렬)
    - [이진 탐색](#이진-탐색)
    - [링크드 리스트](#링크드-리스트)
- [수학](#수학)
    - [소수 판별](#소수-판별)
    - [에라토스테네스의 체](#에라토스테네스의-체)
    - [소인수 분해](#소인수-분해)
    - [피보나치 수](#피보나치-수)
    - [10진수에서 2진수로](#10진수에서-2진수로)
    - [2진수에서 10진수로](#2진수에서-10진수로)
    - [2진수에서 8진수로](#2진수에서-8진수로)
    - [8진수에서 2진수로](#8진수에서-2진수로)


## 문자열
### 문자열 뒤집기

```cpp
void reverse(string& str) {
    int left = 0;
    int right = str.size() - 1;

    while (left < right) {
        char temp = str[left];
        str[left] = str[right];
        str[right] = temp;
    }
}
```

### Palindromic 문자열 판별

```cpp
bool isPalindromicString(string str) {
    int left = 0;
    int right = str.size() - 1;

    while (left < right) {
        if (str[left++] != str[right++]) {
            return false;
        }
    }
    return true;
}
```

## 자료구조 및 알고리즘
### 버블 정렬

```cpp
void bubbleSort(vector<int>& nums) {
    for (int i = 0; i < nums.size() - 1; ++i) {
        for (int j = 0; j < nums.size() - i - 1; ++j) {
            if (nums[j] > nums[j + 1]) {
                int temp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = temp;
            }
        }    
    }
}
```

### 선택 정렬

```cpp
void selectSort(vector<int>& nums) {
    for (int i = 0; i < nums.size() - 1; ++i ){
        int currentMinValuePosition = i;

        for (int j = i + 1; j < nums.size(); ++j) {
            if (nums[currentMinValuePosition] > nums[j]) {
                currentMinValuePosition = j;
            }
        }

        int temp = nums[currentMinValuePosition];
        nums[currentMinValuePosition] = nums[i];
        nums[i] = temp;
    }
}
```

### 삽입 정렬

```cpp
void insertionSort(vector<int>* nums) {
    for (int i = 1; i < nums.size(); ++i) {
        int currentValue = nums[i];

        int j;
        for (j = i - 1; j >= 0; --j) {
            if (currentValue < nums[j]) {
                nums[j + 1] = nums[j];
            }
            else {
                break;
            }
        }

        nums[j + 1] = currentValue;
    }
}
```

### 합병 정렬

```cpp
void mergeTwoArea(vector<int>& nums, int left, int mid, int right) {
    int leftPos = left;
    int rightPos = mid + 1;

    int sortPos = left;
    vector<int> sortArr;
    sortArr.reserve(right + 1);

    while (leftPos <= mid && rightPos <= right) {
        if (nums[leftPos] < nums[rightPos]) {
            sortArr[sortPos] = nums[leftPos++];
        }
        else {
            sortArr[sortPos] = nums[rightPos++];
        }
        sortPos++;
    }

    if (leftPos <= mid) {
        for (int i=leftPos; i<=mid; ++i) {
            sortArr[sortPos++] = nums[i];
        }
    }
    else {
        for (int i=rightPos; i<=right; ++i) {
            sortArr[sortPos++] = nums[i];
        }
    }

    for (int i=left; i<=right; ++i) {
        nums[i] = sortArr[i];
    }
}

void mergeSort(vector<int>& nums, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid + 1, right);
        mergeTwoArea(nums, left, mid, right);
    }
}
```

### 퀵 정렬

```cpp
void swap(vector<int>& nums, int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}

int partition(vector<int>& nums, int left, int right) {
    int pivot = nums[left];
    int low = left + 1;
    int high = right;

    while (low <= high) {
        while (low <= right && pivot >= nums[low]) {
            low++;
        }
        while (high > left && pivot <= nums[high]) {
            high--;
        }

        if (low <= high) {
            swap(nums, low, high);
        }
    }

    swap(nums, left, high);

    return high;
}

void quickSort(vector<int>& nums, int left, int right) {
    if (left <= right) {
        int pivot = partition(nums, left, right);
        quickSort(nums, left, pivot - 1);
        quickSort(nums, pivot + 1, right);
    }
}
```

### 이진 탐색

```cpp
int binarySearch(int[] a, int x) {
    int low = 0;
    int high = a.length - 1;
    int mid;

    while(low <= high) {
        mid = (low + high) / 2;
        if (a[mid] < x) {
            low = mid + 1;
        } else if (a[mid] > x) {
            high = mid - 1;
        } else {
            return mid;
        }
    }
    
    return -1;
}
```

```cpp
int binarySearchRecursive(int[] a, int x, int low, int high) {
    if (low > high) return -1;

    int mid = (low + high) / 2;
    if (a[mid] < x) {
        return binarySearchRecursive(a, x, mid + 1, high);
    } else if (a[mid] > x) {
        return binarySearchRecursive(a, x, low, mid - 1);
    } else {
        return mid;
    }
}
```

### 링크드 리스트

## 수학
### 소수 판별

```cpp
bool isPrime(int n) {
    for (int x = 2; x * x <= n; ++x) {
        if (n % x == 0) {
            return false;
        }
    }
    return true;
}
```

### 에라토스테네스의 체

```cpp
const int MAX_N = 1000001
bool sieve[MAX_N] = {false, };
vector<int> primes;

//0 ~ 1000000까지 소수 구하기
for (int i = 2; i < MAX_N; ++i) {
    if (!sieve[i]) {
        primes.push_back(i);

        // 현재 소수의 배수들은 모두 소수가 될 수 없으므로 지운다.
        for (int j = i + i; j < MAX_N; j += i) {
            sieve[j] = true;
        }
    }
}
```

### 소인수 분해

```cpp
int temp = n;
for (int i=2; i<=n; ++i) {
    while (temp % i == 0) {
        printf("%d\n", i);
        temp /= i;
    }
    
    if (temp == 1) break;
}
```

### 피보나치 수

### 10진수에서 2진수로

### 2진수에서 10진수로

### 2진수에서 8진수로

### 8진수에서 2진수로