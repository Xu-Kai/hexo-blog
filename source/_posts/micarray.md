title: MIC中部分数组的分配以及数组之间的传输
date: 2016-07-06 13:03:21
tags: Intel Xeon Phi/MIC
categories: MIC/GPU
---
编写MIC程序时使用offload模式时，经常需要在CPU端和MIC端传输数据，但是有时并不需要将CPU端的整个数组上传到MIC，只需要上传一部分，并且上传的起始地址并不是数组的起始地址，而有时需要将CPU端的a数组上传到b数组，其实Intel提供了这两种上传方式的方法。
#### **MIC中部分数组的分配**
在MIC端只分配数组的一部分，可以看下面的示例。
```
#include <stdio.h>
#include <stdlib.h>
int main(){
	int *a = (int*)malloc(200 * sizeof(int));
	a[10] = 10;
	printf("CPU: a[10] = %d\n", a[10]);
	#pragma offload targe(mic:0)\
		in(a[10:100] : alloc(a[5:150]))
		{
			printf("MIC: a[10] = %d\n", a[10]);
		}
	return 0;
}
```
a[10:100] 是指CPU端的数组自下标10起的100个元素，即10~109上传到MIC。
alloc(a[5:150])是指在MIC端分配150个元素，但是起始下标是5，即150个元素为5~154。
需要注意的一点是从CPU端上传到MIC端的元素范围必须在MIC端分配的范围之内即5~104之间，若上传a[0:100]或者是a[5:180]均会出现越界的情况。
#### **MIC数组之间的传输**
MIC之间数组之间的传输，可以看下面的示例。
```
#include <stdio.h>
#include <stdlib.h>
int main(){
	int *a = (int*)malloc(1000 * sizeof(int));
	int *b;
	a[100] = 100;
	printf("CPU: a[100] = %d\n", a[100]);
	#pragma offload target(mic:0))\
		in(a[0:500]:into(b[400:500])
		{
			printf("MIC: b[500] = %d\n", b[500]);
		}
	return 0;
}
```
上面的示例程序是指将a数组中下标为0~499的500个元素上传到MIC端b数组中，元素在数组b中的下标范围为400~899。

 上面的操作可以减少MIC端数组分配，减少内存使用，再结合alloc\_if 和 free\_if 就可以完成提高数据传输的效率了。
