---
title: "PHP源码-排序算法"
date: 2021-10-09T16:16:24+08:00
Description: ""
Tags: [PHP, 排序]
Categories: [PHP]
DisableComments: false
---

* 文件： ``Zend/zend_sort.c``:``ZEND_API void zend_sort(void *base, size_t nmemb, size_t siz, compare_func_t cmp, swap_func_t swp``

PHP中的默认排序算法主要为快速排序，同时根据待排序元素数量不同使用不同的算法，具体如下：  


* 当待排序元素数量小于等于16是，使用插入排序  
```code
if (nmemb <= 16) {
	zend_insert_sort(base, nmemb, siz, cmp, swp);
	return;
}
```

* 获取中间位置  

```code
size_t offset = (nmemb >> Z_L(1));
char *pivot = start + (offset * siz);
```

* 根据不同数量使用不同排序算法 数量大于等于1024是使用``zend_sort_5``，否则使用``zend_sort_3``，``zend_sort_n``都是简单粗暴的排序方式，此处主要是为后续快速排序选择基准值  

```code
if ((nmemb >> Z_L(10))) { // 数量大于等于1024
	size_t delta = (offset >> Z_L(1)) * siz; // 1/4 位置
	zend_sort_5(start, start + delta, pivot, pivot + delta, end - siz, cmp, swp); // 对0, nmemb/4, nmemb/2, (3 * nmemb)/4, nmemb 位置进行排序
} else {
	zend_sort_3(start, pivot, end - siz, cmp, swp); // 对 0, nmemb/2, nmemb 位置进行排序
}
```

* 选择快速排序基准值  

```
swp(start + siz, pivot); // 把上一步几个值的中间值作为基准，用于后面的快速排序
pivot = start + siz; // base index 0
i = pivot + siz; // 从左向右找比基准大的值
j = end - siz; // 从右向左找比基准小的值
```

* 寻找大于等于基准的值  

```code
while (cmp(pivot, i) > 0) {
	i += siz; // 从左向右
	if (UNEXPECTED(i == j)) {
		goto done;
	}
}
```

* 寻找小于等于基准的值  

```code
j -= siz; // 这里索引减一是因为经过前面的排序最后一位一定是大于基准的
if (UNEXPECTED(j == i)) {
	goto done;
}
while (cmp(j, pivot) > 0) {
	j -= siz; // 从右向左
	if (UNEXPECTED(j == i)) {
		goto done;
	}
}
swp(i, j);
```

## 源码

```code
ZEND_API void zend_sort(void *base, size_t nmemb, size_t siz, compare_func_t cmp, swap_func_t swp)
{
	while (1) {
		if (nmemb <= 16) {
			zend_insert_sort(base, nmemb, siz, cmp, swp);
			return;
		} else {
			char *i, *j;
			char *start = (char *)base;
			char *end = start + (nmemb * siz);
			size_t offset = (nmemb >> Z_L(1));
			char *pivot = start + (offset * siz);

			if ((nmemb >> Z_L(10))) {
				size_t delta = (offset >> Z_L(1)) * siz;
				zend_sort_5(start, start + delta, pivot, pivot + delta, end - siz, cmp, swp);
			} else {
				zend_sort_3(start, pivot, end - siz, cmp, swp);
			}
			swp(start + siz, pivot);
			pivot = start + siz;
			i = pivot + siz;
			j = end - siz;
			while (1) {
				while (cmp(pivot, i) > 0) {
					i += siz;
					if (UNEXPECTED(i == j)) {
						goto done;
					}
				}
				j -= siz;
				if (UNEXPECTED(j == i)) {
					goto done;
				}
				while (cmp(j, pivot) > 0) {
					j -= siz;
					if (UNEXPECTED(j == i)) {
						goto done;
					}
				}
				swp(i, j);
				i += siz;
				if (UNEXPECTED(i == j)) {
					goto done;
				}
			}
done:
			swp(pivot, i - siz);
			if ((i - siz) - start < end - i) {
				zend_sort(start, (i - start)/siz - 1, siz, cmp, swp);
				base = i;
				nmemb = (end - i)/siz;
			} else {
				zend_sort(i, (end - i)/siz, siz, cmp, swp);
				nmemb = (i - start)/siz - 1;
			}
		}
	}
}
```



