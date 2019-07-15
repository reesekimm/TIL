# Sorting Algorithms

## 1. Bubble Sort
In a bubble sort, the elements of the list gradually **bubbles** to their proper location in the list.  
서로 이웃한 데이터끼리 대소비교를 하여 정렬조건에 맞춰 올바른 위치로 swap한다.

![bubble sort](https://user-images.githubusercontent.com/42695954/61192305-56312780-a6ee-11e9-809c-c05c1635667e.PNG)

#### How it works?
* Perform a sequence of passes through the list.
* On each pass: proceed from left to right, swapping adjacent elements if they are out of order.
* Larger elements bubbles up to the end of the list.
* At the end of the kth pass, the k rightmost elements are in their final positions. (in the worst case)

#### Big O
- **Worst Case: O(n2)**  
=> 1번 순회 할 때 마다 맨 오른쪽부터 요소가 알맞은 위치에 정렬됨. 1번 순회 시 O(n)이고, 최악의 경우 n번 순회해야 하기 때문에 O(n2)
- **Best Case: O(n)**  
=> 이미 정렬이 완료되어 있어서 한 번 순회하면 끝인 경우

#### Disadvantages
1. **Performance (especially with large dataset)**  
We study bubble sort for academic purpose, since it is the easiest to understand.

#### Advantages
1. **Memory**  
Bubble sort is "in place" algorithm  
요소의 위치를 swap하여 변경하기 때문에 추가적인 메모리가 필요하지 않다. 주어진 메모리 안에서 정렬 가능.
2. **Easy to write**  
Ideal for beginners
3. **Good for testing whether a list is sorted or not**  
Other sorting algorithms often cycle through their whole sorting sequence, but we can do O(n) with bubble sort in the best case scenario.  
→ 순회가 1회에서 끝나지 않으면 정렬이 되어있지 않음을 바로 알 수 있다.

&nbsp; 

## 2. Insertion Sort

Going from left to right, **inserts** each element into its proper location with respect to the elements to its left.  
배열 안의 '정렬된 부분' 안에 '정렬되지 않은 부분'의 데이터들을 정렬 조건에 맞추어 삽입해 나간다.

![insertion sort](https://user-images.githubusercontent.com/42695954/61192702-65fe3b00-a6f1-11e9-99a6-094542c4a727.PNG)

#### How it works?
* index 1부터 기준이 되는 값으로 설정하여 자신보다 앞의 원소와 대소비교 하여 자신의 위치를 찾아나간다.

![insertion sort how it works](https://user-images.githubusercontent.com/42695954/61203242-c78bcd00-a724-11e9-9cd2-a33b65a7ae9e.PNG)

=> 삽입을 하면 데이터가 하나씩 밀려야 하기 때문에 배열의 길이가 길어질수록 효율이 떨어진다.

#### Big O
- **Worst Case: O(n2)**  
=> 데이터가 정렬조건과 정 반대로 뒤집혀져 있을 경우 O(n)을 n번 반복해야 한다.
- **Best Case: O(n)**  
=> 정렬조건에 따라 이미 정렬이 완료되어 있을 경우

#### Disadvantages / Advantages
Bubble sort와 동일

&nbsp; 

> **What does "stable" mean?**  
> 원본에서 같은 값을 가진 원소들의 상대적인 위치가 정렬 이후에도 유지되면 `stable`, 정렬 이후에 바뀌면 `unstable`
> ![stable](https://user-images.githubusercontent.com/42695954/61200397-31ec3f80-a71c-11e9-96cd-9819db5986de.jpg)

&nbsp; 

> **What does "in place" mean?**  
> 입력리스트 내부에서 정렬이 이뤄지는 경우를 가리키며, 반대는 정렬 도중에 별도 저장공간을 필요로 하는 경우이다.  
> Merge Sort의 경우 입력리스트를 분할해 이를 정렬하고 다시 합치는 과정에서 분할된 리스트를 별도로 저장해 두어야 하기 때문에 in-place가 아니다.


ref)
![average case](https://user-images.githubusercontent.com/42695954/61206208-fad25a00-a72c-11e9-8c61-b482c707e9cb.PNG)

&nbsp; 

## 3. Selection Sort
'정렬되지 않은 부분'안의 최소값 (또는 최대값)을 선택해서 '정렬된 부분의 끝으로 swap한다.

![selection](https://user-images.githubusercontent.com/42695954/61193382-0ce4d600-a6f6-11e9-8c25-f42723275b6f.PNG)


#### How it works?
* 배열의 index 0 부터 데이터 하나를 선택한다.
* 선택한 데이터 뒤에 있는 원소들 중 가장 작은 값을 찾는다.
* 내가 선택한 값과 바꾼다. (swapping)

#### Big O
- **Worst Case: O(n2)**  
=> 데이터의 개수가 n개라고 했을 때,  
첫 회전에서의 비교횟수 : 1 ~ n-1 => n-1  
두번째 회전에서의 비교횟수 : 2 ~ n-1 => n-2  
.  
.  
.  
(n-1) + (n-2) + .... + 2 + 1 => n(n-1) / 2  
즉, O(n2) 

- **Best Case: O(n2)**  

#### Disadvantages
1. **Performance (especially with large dataset)**  
We study bubble sort for academic purpose, since it is the easiest to understand.

#### Advantage
1. **Memory**  
Bubble sort is "in place" algorithm  
요소의 위치를 swap하여 변경하기 때문에 추가적인 메모리가 필요하지 않다. 주어진 메모리 안에서 정렬 가능.
2. **Easy to write**  
Ideal for beginners

~~~
[ (5), [5], 1 ]  ---> [ 1, [5], (5) ]
            (5)와 1을 swap
~~~

=> Selection Sort는 Unstable함!


&nbsp; 

## 4. Merge Sort
리스트를 잘게 쪼갠 뒤 둘씩 크기를 비교해 정렬하고 분리된 리스트를 재귀적으로 **합쳐서** 정렬을 완성한다. 분할된 리스트를 저장해둘 공간이 필요해 메모리 소모량이 큰 편

![merge sort](https://user-images.githubusercontent.com/42695954/61193627-9cd74f80-a6f7-11e9-9b5a-1c22f547cfc7.gif)

#### Big O
- **Worst Case: O(n log n)**
- **Best Case: O(n log n)**  

요소 하나만 봤을 때 O(log n)인데, 전체적으로 n개의 요소가 있기 때문에 O(n log n).  

#### Disadvantages
1. **Performance:**
Requires additional O(n) space.  
요소별로 잘라서 일일히 배열을 만들어 줘야 하기 때문에 memory 효율성이 떨어진다.

#### Advantage
1. **Best case, Worst case 모두 O(n log n)**  
기복 없이 언제나 일정한 시간복잡도를 보장한다.  

> **< Divide and Conquer >**  
> **1. Divide** : Split the array in half, forming two subarrays.  
> **2. Conquer** : Apply merge sort recursively to the subarrays, stopping when a subarray has a single element.  
> **3. Combine** : Merge the sorted subarrays.
>
> e.g. Binary Search Tree, Quick Sort, Merge Sort, ...  
>
> 큰 문제를 작게 쪼개서 재귀를 통해 반복 수행하고 취합

&nbsp; 

**Which of the following are stable?**
> 
> * **Insertion Sort**
> * **Merge Sort**
> * **Bubble Sort**
> * ~~Selection Sort~~  
> 
> 정답 : Selection Sort 제외하고 전부  
> 
> ref) Merge Sort 빼고 전부 "in place"

&nbsp; 

**< 추가로 공부할 것 >**
> - ★Quick Sort★
> - Divide and Conquer
> - Dynamic Programming

&nbsp; 

---

**Reference**  

- [https://zeddios.tistory.com/20](https://zeddios.tistory.com/20)
- [https://ratsgo.github.io/data%20structure&algorithm/2017/10/19/sort/](https://ratsgo.github.io/data%20structure&algorithm/2017/10/19/sort/)
