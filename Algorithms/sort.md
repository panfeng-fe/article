#### 1、冒泡排

相邻的两个数不断比较，选出极值，放在最后

```
js版本
function Bubble(arr){
    let len = arr.length;
    for(let i = 0; i < len -1; i++){
        for(let j = i+1; j < len; j++){
            if(arr[i] > arr[j]){
                let temp = arr[j]
                arr[j] = arr[i]
                arr[i] = temp
            }
        }     
    }
    return arr
}

Golang版本
func Bubble(arr []int) []int {
	lens := len(arr)
	for i := 0; i < lens - 1; i++ {
		for j := i + 1; j < lens; j++ {
			if arr[i] > arr[j] {
				arr[i] ,arr[j] = arr[j] ,arr[i]
			}
		}
	}
	return arr
}
```



#### 2、选择排

遍历出的一个数依次和后面所有数比较，选出极值

```
js版本
function Selection(arr){
    let len = arr.length
    let minIndex, temp
    for(let i = 0; i < len - 1; i++) {
        minIndex = i
        for(let j = i + 1; j < len; j++) {
            if(arr[j] < arr[minIndex]) {
                minIndex = j
            }
        }
        temp = arr[i]
        arr[i] = arr[minIndex]
        arr[minIndex] = temp
    }
    return arr;
}

Golang版本
func Selection(arr []int) []int {
	lens := len(arr)
	var minIndex int
	for i := 0; i < lens - 1; i++ {
		minIndex = i
		for j := i + 1; j < lens; j++ {
			if arr[j] < arr[minIndex] {
				minIndex = j
			}
		}
		arr[i],arr[minIndex] = arr[minIndex],arr[i]
	}
	return arr
}
```



#### 3、插入排序

默认第一个数为极值，从第二个数开始遍历，将遍历出来的数放入零食变量，不断将前面的数与自己比较，按照最大或最小排序不同，选择是否将前面的数向后移动，最终达到排序的目的

```
js版本
function Insertion(arr){
    let len = arr.length
    let current,preIndex
    for (let i = 1; i < len; i++){
        current = arr[i]
        for (preIndex = i-1; preIndex >= 0 && arr[preIndex] > current; preIndex-- ){
            arr[preIndex +1] = arr[preIndex]
        }
        arr[preIndex+1] = current
    }
    return arr
}

Golang版本
func Insertion(arr []int) []int {
	lens := len(arr)
	for i := 1; i < lens; i++{
		// 需要插入的数tmp
        tmp := arr[i]
        for j:=i-1;j>=0;j--{
            if tmp < arr[j] {
                arr[j+1] = arr[j]
                arr[j] = tmp
            } else {
                break
            }
        }
	}
	return arr
}
```



#### 4、希尔排序

没整明白

```
js版本
function Shell(arr){
    let len = arr.length
    for(let gap = Math.floor(len/2); gap>0; gap = Math.floor(gap/2)){
        for(let i = gap; i < len; i++){
            let j = i
            let current = arr[i]
            while(j - gap >= 0 && current < arr[j-gap]){
                arr[j] = arr[j-gap]
                j = j - gap
            }
            arr[j] = current
        }
    }
    return arr
}
```



#### 5、归并排序

归并排序的思想是分治

```
// A 数组 p 起始下表 q 中间下表 r 结尾下标
function merge(A, p, q, r) {
    const A1 = A.slice(p, q);
    const A2 = A.slice(q, r);
    // 将数组根据下表分为2个数组
    // 往A1和A2里push一个最大值，比如防止A1里的数都比较小，导致第三次遍历某个数组里没有值
    A1.push(Number.MAX_SAFE_INTEGER);
    A2.push(Number.MAX_SAFE_INTEGER);
  	
  	// 将新剥离出来的数组比较，把较大的数挡在原数组的位置上
    for (let i = p, j = 0, k = 0; i < r; i++) {
      if (A1[j] < A2[k]) {
        A[i] = A1[j++];
      } else {
        A[i] = A2[k++];
      }
    }
}

//排序：A 数组 p 第一次默认0 r 数组右下标
function merge_sort(A, p = 0, r) {
	// 判断是否存在r，不存在就是数组长度，第一次调用时会用到
    r = r || A.length;
    // 判断左右下标是否相邻，相邻则代表数组只有一个数
    if (r - p === 1) {
      return;
    }
    // 分
    const q = Math.floor((p + r) / 2);
    // 将 数组左下标到p 再次归并
    merge_sort(A, p, q);
    // 将 数组右下标到r 再次归并
    merge_sort(A, q, r);
    // 合并
    merge(A, p, q, r);
    return A;
}
```



#### 6、快速排序

随意从长度不为一的数组取一个值做为中间值，

```
js版本
function Quick(arr){
    if (arr.length <= 1 ){
        return arr
    }
    let middleIndex = Math.floor(arr.length/2),
    middleValue = arr.splice(middleIndex,1)[0];

    let leftArr = [],
    rightArr = [];

    for(let i = 0;i<arr.length;i++){
        arr[i]<middleValue?leftArr.push(arr[i]):rightArr.push(arr[i]);
    }
    return Quick(leftArr).concat(middleValue,Quick(rightArr))
}

```



#### 7、堆排序

pass

#### 8、计数排序



#### 9、桶排序



#### 10、基数排序

