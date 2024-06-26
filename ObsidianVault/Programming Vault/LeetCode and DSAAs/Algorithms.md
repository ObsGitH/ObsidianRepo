# Common "Edge" Cases

- What if the data structure is empty
- What if there is only one element in the data structure
- What if the operation has to be done to the element in the beginning of the data structure
- What if the operation has to be done to the element  in the end of the data structure
- What about the elements in the middle of the data structure.
# Searching Algorithms

- Linear Search
	- iterate through the data structure one element at a time
	- O(n)
	- Disadvantages
		- Slow for large data sets
	- Advantages
	- decent/fast for medium/small data sets respectively
	- Does not need to be sorted
	- Useful dor Data Structures that are incapable of random access (Linked Lists)
- Binary Search
	- used to find the position of a element in a sorted array
	- sorted array sort their elements following a certain criteria
		- Generally, sorted arrays sort the elements in ascending order
	- It is called binary search because we search based on a certain proposition being true or false.
		- Take for example the stereotypical array (elements sorted in ascending order), to do binary search of an element on it we would go to the index in the middle of the array and see if the value that we want is bigger, equal or lesser than the value in the middle. That would eliminate half of the array. We would continue this process until we find the element
	- Binary search is log n complexity. Because it takes all the elements  of a Data Structure that would need to be checked and divide that number by 2 at each iteration. To be more specific, it is O(log₂(n))
	- Linear search is more efficient for small data sets (O(n)) but binary search is way better than linear search for large data sets (O(log n))
- Interpolation Search
	- Improvement of binary search for "uniformly" distributed data. The word uniformly here means that the data and it's index are following a linear pattern. 
	- You guess where the value is based on a probe. Use the result of the probe to narrow the area. Rinse and repeat until the probe gives you the result you are looking for.
		- The probe is: pos=low+(array[high]−array[low])(x−array[low])​×(high−low)
	- On average it is O(log(log (n))). 
	- Worst case cenario is O(n), this happens when the data increases exponentially. Interpolation search expects a linear pattern between the data values  and it's index, if the data follows an exponential pattern than the values held by the data have high levels of discrepancy between each other and the probe becomes ineffective.
- Bubble Sort
	- O(n^2) time complexity.  If the data set is expected to grow overtime, then bubble sort is a terrible sort algorithm. If the data set is a relatively small and static, then bubble sort is a valid sorting algorithm.
	- Elements are sorted to ascending order
	- I created a bubble sort that is only O(n^2) in the worst case cenario
- Selection Sort
	- This is an optmization of Bubble Sort, but it is still O(n^2). Meaning that every time you can use a bubble sort you should use a selection sort instead.
	- Keep track of the min value in each iteration of the inner loop and chance the value of the current iteration of the outer loop with the iteration of the inner loop
- Insertion Sort
	- 

# Sorting Algorithms