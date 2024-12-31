---
title: Java 实现各排序算法
date: 2024-09-28
tags: ['算法', '排序']
categories: ['后端开发', '算法']
description: 排序算法
---
# 冒泡排序

​        当有一次循环没有进行交换时，则已经有序，直接打破。

# 冒泡排序

```
public static void maopao(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            boolean flag = true;
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    flag = false;
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
            if (flag) {
                break;
            }
        }
    }
```

# 选择排序

​        每次找出最小值和最大值进行左右交换，当最小值和最大值索引相同时，应更新最大值索引（先交换最小值时）

# 选择排序

```
public static void xuanze(int[] arr) {
        for (int i = 0; i < arr.length / 2; i++) {
            int min = arr[i];
            int minIndex = i;
            int max = arr[i];
            int maxIndex = i;
            int len = arr.length - 1 - i;

            for (int j = i + 1; j <= len; j++) {
                if (min > arr[j]) {
                    min = arr[j];
                    minIndex = j;
                }
                if (max < arr[j]) {
                    max = arr[j];
                    maxIndex = j;
                }
            }
            if (minIndex != i) {
                arr[minIndex] = arr[i];
                arr[i] = min;
                if (maxIndex == i) {
                    maxIndex = minIndex; //如果最大值和最小值互换，则最大值下标也要互换
                }
            }
            if (maxIndex != len) {
                arr[maxIndex] = arr[len];
                arr[len] = max;
            }
        }
    }
```

# 插入排序

​        从1开始，到len - 1。

# 插入排序

```
public static void charu(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int insertIndex = i;
            int insertValue = arr[i];
            while (insertIndex > 0 && insertValue < arr[insertIndex - 1]) {
                arr[insertIndex] = arr[insertIndex - 1];
                insertIndex--;
            }
            arr[insertIndex] = insertValue;
        }
    }
```

# 希尔排序

​        插入式间隔为gap的插入式排序            gap一般为数组元素个数的二分之一

1. 创建一个gap循环体，每循环一次gap /= 2。
2. 在gap循环体内创建一个步长为gap的循环体，循环gap到arr.length的元素。
3. 与insertIndex - gap比较，选择是否插入。

# 希尔排序

```
public static void xier(int[] arr) {
        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            for (int i = gap; i < arr.length; i++) {
                int insertIndex = i;
                int insertValue = arr[i];
                while (insertIndex - gap >= 0 && insertValue < arr[insertIndex - gap]) {
                    arr[insertIndex] = arr[insertIndex - gap];
                    insertIndex -= gap;
                }
                arr[insertIndex] = insertValue;
            }
        }
    }
```

# 快速排序

​        定义一个基准值和左右指针，把小于基准值的放左边，大于基准值的放右边

# 快速排序

```
public static void quickSort(int[] arr, int left, int right) {
        int l = left, r = right;
        if (left >= right) {return;}
        while (l < r) {
            while (l < r && arr[r] >= arr[left]) r--;
            while (l < r && arr[l] <= arr[left]) l++;
            if (l < r) {
                swap(arr, l, r);
            }
        }swap(arr, left, l);
        quickSort(arr, left, l -1);
        quickSort(arr, r + 1, right);
    }
```

# 归并排序

# 归并排序

```
public static void mergeSort(int[] arr, int left, int right , int[] temp) {
        if (left < right) {
            int mid = ((left + right) / 2);
            mergeSort(arr, left, mid, temp);
            mergeSort(arr, mid + 1, right, temp);
            merge(arr, left, mid, right, temp);
        }
    }
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left, j = mid + 1, t = 0;
        while (i <= mid && j <= right) {
            if (arr[i] < arr[j]) {
                temp[t] = arr[i];
                t++; i++;
            }else {
                temp[t] = arr[j];
                t++; j++;
            }
        }
        while (i <= mid) {
            temp[t] = arr[i];
            t++; i++;
        }
        while (j <= right) {
            temp[t] = arr[j];
            t++; j++;
        }
        int leftIndex = left;
        t = 0;
        while (leftIndex <= right) {
            arr[leftIndex] = temp[t];
            leftIndex++; t++;
        }
    }
```

# 基数排序

1. 创建一个二维数组（桶），存储桶的个数和每个桶可存储的元素的个数。
2. 创建一个数组用来记录每个桶存储的元素的个数。
3. 遍历数组，找出最大的数，计算出它的位数有几位。
4. 写一个循环次数为‘位数’的循环体，然后遍历数组，把元素放进对应的桶里。
5. 把桶里的元素放回原数组，并清空该桶的元素。

# 基数排序

```
public static void radixSort(int[] arr) {
        int[][] bucket = new int[10][arr.length];
        int[] bucketElementcounts = new int[10];
        int max = arr[0];
        for (int i = 0; i < arr.length; i++) {
            if (max < arr[i]) {
                max = arr[i];
            }
        }
        int maxLength = (max + "").length();
        for (int i = 0; i < maxLength; i++) {
            for (int j = 0; j < arr.length; j++) {
                int value = arr[j] / (int) Math.pow(10, i) % 10;
                bucket[value][bucketElementcounts[value]] = arr[j];
                bucketElementcounts[value]++;
            }
            int index = 0;
            for (int k = 0; k < bucket.length; k++) {
                if (bucketElementcounts[k] != 0) {
                    for (int j = 0; j < bucketElementcounts[k]; j++) {
                        arr[index++] = bucket[k][j];
                    }
                }
                bucketElementcounts[k] = 0;
            }
        }
    }
```

# 堆排序

​        完全二叉树，最大堆

1. heapSort：从最后一个父节点开始，向根节点进行元素下沉(heapify)
   1. 从根目录开始循环，每次heapify都把第一个元素和最后一个元素交换，并且长度减一(每次隔离最大值元素)
2. heapify：判断是否存在两个子节点，若存在两个子节点则判断左子节点是否大于右子节点。
   1. 从传入的父节点开始，依次元素下沉，

# 堆排序

```
public static void heapSort(int[] arr, int len) {
        for (int i = (len - 2) / 2; i >= 0; i--) heapify(arr, i, len - 1);
        for (int i = len - 1; i > 0; i--) {
            swap(arr, 0, i);
            heapify(arr, 0, i - 1);
        }
    }
public static void heapify(int[] arr, int start, int end) {
    int dad = start, son = dad * 2 + 1;
    if (son > end) return;
    if (son + 1 <= end && arr[son] < arr[son + 1]) son++;
    if (arr[dad] > arr[son]) return;
    swap(arr, dad, son);
    heapify(arr, son, end);
}
```

# 计数排序

# 计数排序

```
public static void countSort(int[] arr) {
        int max = arr[0], min = arr[0];

        //找出最大值和最小值
        for (int i = 0; i< arr.length; i++) {
            if (max < arr[i]) max = arr[i];
            if (min > arr[i]) min = arr[i];
        }
        //初始化计数数组
        int[] temparr = new int[max - min + 1];
        for (int i = 0; i < temparr.length; i++) {
            temparr[i] = 0;
        }
        //开始计数
        for (int i = 0 ; i < arr.length; i++) {
            temparr[arr[i] - min]++;
        }
        int t = 0;
        for (int i = 0; i < temparr.length; i++) {
            for (int j = 0; j < temparr[i]; j++) {
                arr[t++] = i + min;
            }
        }count = 1;
    }
```

