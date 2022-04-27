# Quick Sort

```java
private void quickSort(int[] arr) {
    pivot(arr, 0, arr.length-1);
}

private void pivot(int[] arr, int low, int high) {
    if(low >= high) return;

    int l = low;
    int r = high;
    int pivot = (l+r) / 2;
    int pivotVal = arr[pivot];

    while (l <= r) {
        while (arr[l] < pivotVal) l++;
        while (arr[r] > pivotVal) r--;

        if(l <= r) {
            int tmp = arr[l];
            arr[l] = arr[r];
            arr[r] = tmp;

            l++;
            r--;
        }
    }
    pivot(arr, low, r);
    pivot(arr, l, high);
}
```

# Merge Sort

```java
private void mergeSort(int[] arr) {
    divide(arr, 0, arr.length-1);
}

private void divide(int[] arr, int low, int high) {
    if(low >= high) return;

    int mid = (low + high) / 2;

    divide(arr, low, mid);
    divide(arr, mid+1, high);

    merge(arr, low, mid, high);
}

private void merge(int[] arr, int low, int mid, int high) {
    int[] temp = new int[high-low+1];
    int idx = 0;

    int left = low;
    int right = mid+1;

    while (left <= mid && right <= high) {
        if(arr[left] <= arr[right]) {
            temp[idx] = arr[left];
            left++;
        } else {
            temp[idx] = arr[right];
            right++;
        }
        idx++;
    }

    while (left <= mid) {
        temp[idx] = arr[left];
        idx++;
        left++;
    }

    while (right <= high) {
        temp[idx] = arr[right];
        idx++;
        right++;
    }

    for (int i = low; i <= high; i++) {
        arr[i] = temp[i-low];
    }
}
```