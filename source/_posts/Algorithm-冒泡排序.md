---
title: Algorithm-冒泡排序
date: 2017-05-22 20:36:47
tags: [Java, Algorithm]
categories:
- Algorithm
- BubbleSort
---

# 冒泡排序
> 这里假定冒泡排序是整形数组，并且以升序排列

1. 冒牌排序需要我们遍历几次数组，每次遍历中，需要比较相邻的两个元素，如果按降序排列，则互换，否则保持不变！由于较小的值像“气泡”一样逐渐浮向顶部，而较大的值沉向底部，所以称为冒泡排序或者下沉排序。

2. 特征：假设一共有n个元素，第m次遍历之后，最后的m个元素顺序已经确定
3. 时间复杂度：最大为o(n*n)，最小为o(1)

----------

## 初始的冒泡排序：
```Java
/**
	 * 初级别的冒泡排序
	 * @param nums
	 * @return 排序后的数组
	 */
	public static int[] toBubbleSort0(int[] nums){
		int count = 0;
		int sum = 0;
		for (int i = 1; i < nums.length - 1; i++){
			for (int j = 0; j < nums.length - i; j++){
				sum++;
				if(nums[j] > nums[j+1]){
					count++;
					int temp = nums[j];
					nums[j] = nums[j+1];
					nums[j+1] = temp;
				}
			}
		}
		System.out.println(count);
		System.out.println(sum);
		return nums;
	}

```
<!-- more -->


----------

## 简单的冒泡排序：
```Java
/**
	 * 一个简单的冒泡排序
	 * @param nums
	 * @return 排序后的数组
	 */
public static int[] toBubbleSort(int[] nums){
		int count = 0;
		int sum = 0;
		for (int i = 0; i < nums.length - 1; i++){
			for (int j = 0; j < nums.length - 1 - i; j++){
				sum++;
				if(nums[j] > nums[j+1]){
					count++;
					int temp = nums[j];
					nums[j] = nums[j+1];
					nums[j+1] = temp;
				}
			}
		}
		System.out.println(count);
		System.out.println(sum);
		return nums;
	}

```
----------

## 复杂的冒泡排序
<font color="red">这里我们采用了一个标记符来决定是否继续下次排序，标记符的作用是如果上次遍历没有发生排序，说明我们排序已经确定，所以我们不再继续排序，因为每次排序的数字，都是上次排序的一个子集，如果我们上次没有改变，那么之后也不需要进行排序，从而减少我们耗费的时间</font>

```Java
/**
	 * 一个复杂的冒泡排序
	 * @param nums
	 * @return 排序后的数组
	 */
public static int[] toBubbleSort1(int[] nums){
		boolean next = true;
		int count = 0;
		int sum = 0;

		for (int i = 0; i < nums.length - 1 && next; i++){
			next = false;
			for (int j = 0; j < nums.length - 1 - i; j++){
				sum++;
				if(nums[j] > nums[j+1]){
					count++;
					int temp = nums[j];
					nums[j] = nums[j+1];
					nums[j+1] = temp;
					next = true;
				}
			}
		}
		System.out.println(count);
		System.out.println(sum);
		return nums;
	}
```

----------
## 测试数据集：
```Java
int[] b = {4,1,2,3};
```

结果记录：
-----
0
6
1 2 3 4
3
5
1 2 3 4
3
5
1 2 3 4
-----

> 通过结果发现，我们可以知道复杂的冒泡排序时间复杂度比简单的冒泡排序多加了个判断，极大程度的减少了我们排序的重复次数，对于算法的性能有很大的改进

在最佳的情况下，冒泡排序只需要一次就可以确定数组已排好序，不需要进行下一次便利，由于第一次遍历的次数为n-1，因此在最佳情况下，冒泡
排序的时间为O(n)
