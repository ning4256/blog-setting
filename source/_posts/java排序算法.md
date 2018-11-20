---
title: java排序算法
tags: 这是标签
categories: 这是分类
archives: 这是归档
photos:
  - 'http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg'
date: 2018-11-13 20:24:22
---

1.  冒泡算法
```
import java.util.Arrays;

public class TestBubble2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] arr1 = {11, 221, 321, 21, 214, 21, 13};
		bubble(arr1);
		System.out.println(Arrays.toString(arr1));
	}
	public static void bubble(int[] arr) {
		if(arr == null || arr.length < 2) {
			return;
		}
		int temp;
		boolean flag = true;
		for(int i = 0; i < arr.length; i++) {
			for(int j = 0; j < arr.length - 1 - i; j++) {
				if(arr[j] > arr[j+1]) {
					temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
					flag = false;
				}
			}
			if(flag) {
				break;
			}
		}
	}
}

```

