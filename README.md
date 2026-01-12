							Shalala Sort (not in-place, stable algorithm)
				This sorting algorithm is dedicated to my dear mother, Shalala Hasanzadeh Neman, a mathematician and programmer who passed away untimely.
				Shalala Sort is a slow yet memory-efficient and very simple variant of the Counting Sort concept.
				It also reflects the "select elements and append to the sorted array" principle, similar to Selection Sort.
				In essence, it acts as a hybrid of Counting Sort and Insertion Sort.
				Shalala Sort is a stable sorting algorithm; it is not in-place.
				In this respect, it is comparable to Counting Sort.
				However, compared to Counting Sort, it uses less memory (more in-place than Counting Sort).
				In terms of auxiliary space, it is nearly one of the most optimal sorting algorithms.
				This makes it potentially useful in memory-constrained systems and environments where code simplicity is prioritized
				(particularly embedded systems, low-level software, microcontrollers, etc.).

				Worst Case:
			        Time Complexity – O(K * N) → O(K) * (O(N) + O(1)) = O(K * N)
					          O(K) – external loop
					          O(N) – internal loop
						  O(1) – if statement
			        Space Complexity– O(N)
						  O(1) - Auxiliary Space


Using POD to describe the building and working principle of the sorting algorithm:

1.1) Working with `"zero, positive and negative"` numbers version

```c
#include <stdio.h>
#include <stdlib.h>
#define arraySize 12
#define arrayRange 8
#define arrayNegVal -7

void shalalaSort(int* arr, int* arrCountSort)
{
    int counter = 0;
    
    for(int key = arrayNegVal; key < arrayRange; ++key)  // O(K)
    {
        for(int i = 0; i < arraySize; ++i)               // O(N)
            if (key == arr[i])                           // O(1), but inside the algorithm it works O(K * N) times all together
            {
                arrCountSort[counter] = key;
                counter++;
            }
    }
}

void printArray(int* arr)
{
    for(int i = 0; i < arraySize; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(int argc, char* argv[])
{
    int arrayMain[arraySize] = {4, 3, 1, -5, 0, 1, 6, -7, 5, -1, 5, 3};
    printArray(arrayMain);
    
    int arraySorted[arraySize] = {0};  

    shalalaSort(arrayMain, arraySorted);
    printArray(arraySorted);

    return EXIT_SUCCESS;
}


Output:
-------
4 3 1 -5 0 1 6 -7 5 -1 5 3 
-7 -5 -1 0 1 1 3 3 4 5 5 6
```


1.2) Working with `"zero, positive, negative"` numbers without macros declaration

```c
#include <stdio.h>
#include <stdlib.h>

void shalalaSort(int* arr, int* arrCountSort, int arraySize)
{
    int counter = 0;
    int arrMinVal = arr[0], arrMaxVal = arr[0];
    
    for (int i = 1; i < arraySize; ++i)
        if (arr[i] < arrMinVal)
            arrMinVal = arr[i];
        else if (arr[i] > arrMaxVal)
            arrMaxVal = arr[i];
    
    for(int key = arrMinVal; key <= arrMaxVal; ++key)  // O(K)
    {
        for(int i = 0; i < arraySize; ++i)               // O(N)
            if (key == arr[i])                           // O(1), but inside the algorithm it works O(K * N) times all together
            {
                arrCountSort[counter] = key;
                counter++;
            }
    }
}

void printArray(int* arr, int arraySize)
{
    for(int i = 0; i < arraySize; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(int argc, char* argv[])
{
    int arrayMain[] = {4, 3, 1, -5, 0, 1, 6, -7, 5, -1, 5, 3};
    int arraySize = sizeof(arrayMain) / sizeof(arrayMain[0]);
    printArray(arrayMain, arraySize);
    
    int arraySorted[arraySize] = {0};  

    shalalaSort(arrayMain, arraySorted, arraySize);
    printArray(arraySorted, arraySize);

    return EXIT_SUCCESS;
}

Output:
-----------
4 3 1 -5 0 1 6 -7 5 -1 5 3
-7 -5 -1 0 1 1 3 3 4 5 5 6
```


| Feature                | Evaluation                                                     |
| ---------------------- | -------------------------------------------------------------- |
| **Time Complexity**    |  O(n × k)  – `n` = number of elements, `k = max - min + 1`     |
| **Space Complexity**   |  O(n)  – Only output array (`arrCountSort[]`)                  |
| **Auxiliary Space**    |  O(1)  – No counting arrays or helper structures               |
| **Stable?**            | ✅ Yes – Equal values retain their original order in the output|
| **In-place?**          | ❌ No – A separate output array is used                        |
| **Supports negative?** | ✅ Yes – Min value is determined (`arrMinVal`), no offset used |

`Stability` is thanks to `arr[i] == key` comparisons proceeding in increasing `i`, preserving original order.
`Not in-place`, though more `space-efficient` than `Counting Sort`. `Auxiliary space` is minimal.
`Time complexity` depends on the value range `→ O(n × k)`.


2.1) Working with `"strings"` version

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define arraySize 11
#define arrayRange 255
#define arrayNegVal 0

void shalalaSort(char* arr, char* arrCountSort)
{
    int counter = 0;
    
    for(int key = arrayNegVal; key < arrayRange; ++key)  // O(K)
    {
        for(int i = 0; i < arraySize; ++i)               // O(N)
            if (key == arr[i])                           // O(1), but works O(K * N) times total
            {
                arrCountSort[counter] = key;
                counter++;
            }
    }

    arrCountSort[arraySize] = '\0';
}

void printArray(char* arr)
{
    for(int i = 0; i < strlen(arr); ++i)
        printf("%c ", arr[i]);
    printf("\n");
}

int main(int argc, char* argv[])
{
    char arrayMain[] = "shalala1966";
    printArray(arrayMain);
    
    char arraySorted[strlen(arrayMain)];
    
    shalalaSort(arrayMain, arraySorted);
    printArray(arraySorted);

    return EXIT_SUCCESS;
}

Output:
-------
s h a l a l a 1 9 6 6
1 6 6 9 a a a h l l s
```

2.2) Working with `"strings"` without macros declaration

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void shalalaSort(const char* arr, char* arrCountSort)
{
    int counter = 0;
    int arraySize = strlen(arr);
    char arrMinChar = arr[0];
    char arrMaxChar = arr[0];

    for (int i = 1; arr[i] != '\0'; ++i) {
        if (arr[i] < arrMinChar)
            arrMinChar = arr[i];
        if (arr[i] > arrMaxChar)
            arrMaxChar = arr[i];
    }
    
    for(int key = arrMinChar; key < (arrMaxChar + 1); ++key)  // O(K)
    {
        for(int i = 0; i < arraySize; ++i)                   // O(N)
            if (key == arr[i])                               // O(1), total O(K * N)
            {
                arrCountSort[counter] = key;
                counter++;
            }
    }

    arrCountSort[arraySize] = '\0';
}

void printArray(char* arr)
{
    for(int i = 0; i < strlen(arr); ++i)
        printf("%c ", arr[i]);
    printf("\n");
}

int main(int argc, char* argv[])
{
    char arrayMain[] = "shalala19-1";
    printArray(arrayMain);
    
    char arraySorted[strlen(arrayMain)];
    
    shalalaSort(arrayMain, arraySorted);
    printArray(arraySorted);

    return EXIT_SUCCESS;
}

Output:
-------
s h a l a l a 1 9 - 1
- 1 1 9 a a a h l l s
```

3) `Stability` test using a `struct array`

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int id;
    int val;     // Value to be sorted
} Item;

void shalalaSort(Item* input, Item* output, int size) {
    int min = input[0].val, max = input[0].val;
    int counter = 0;

    for (int i = 1; i < size; ++i) {
        if (input[i].val < min) min = input[i].val;
        if (input[i].val > max) max = input[i].val;
    }

    for (int key = min; key <= max; ++key) {
        for (int i = 0; i < size; ++i) {
            if (input[i].val == key) {
                output[counter++] = input[i];  // Copied in stable order
            }
        }
    }
}

void printItems(Item* arr, int size, const char* label) {
    printf("%s:\n", label);
    for (int i = 0; i < size; ++i) {
        printf("ID: %d, Val: %d\n", arr[i].id, arr[i].val);
    }
    printf("\n");
}

int main(void) {
    Item input[] = {
        {1, 4}, {2, 2}, {3, 4}, {4, 3}, {5, 2}, {6, 4}
    };

    int size = sizeof(input) / sizeof(input[0]);
    Item output[size];

    printItems(input, size, "Original");
    shalalaSort(input, output, size);
    printItems(output, size, "Sorted");

    return EXIT_SUCCESS;
}

Output:
-------
Original:
ID: 1, Val: 4
ID: 2, Val: 2
ID: 3, Val: 4
ID: 4, Val: 3
ID: 5, Val: 2
ID: 6, Val: 4

Sorted:
ID: 2, Val: 2
ID: 5, Val: 2
ID: 4, Val: 3
ID: 1, Val: 4
ID: 3, Val: 4
ID: 6, Val: 4
```


Note: `Shalala Sort` copies struct elements with equal keys into the output in their original input order, satisfying stability.

| Observation                                     | Result |
| ----------------------------------------------- | ------ |
| Do equal-key elements retain initial order?     | ✅ Yes |
| Is search done left to right?                   | ✅ Yes |
| Are objects copied to output in original order? | ✅ Yes |
| Is Shalala Sort stable?                         | ✅ Yes |


# 1. Shalala Sort:

## Working principle:
Avoids counting arrays. Iterates through possible values and scans the input array for matches, appending each to the output.

## Time complexity:
Outer loop: `O(K)`
Inner loop: `O(N)`
Total: `O(N × K)`

## Space complexity:
Output only: `O(N)`
Auxiliary: `O(1)`

| Feature                   | Evaluation                 |
| ------------------------- | -------------------------- |
| **Time Complexity**       |  O(n × k)                  |
| **Space Complexity**      |  O(n)                      |
| **Auxiliary Space**       |  O(1)                      |
| **Stable?**               | ✅ Yes                     |
| **In-place?**             | ❌ No                      |
| **Works with negatives?** | ✅ Yes (no offset required)|



# 2. Counting Sort:

## Working principle:
Counts occurrences of values `(including negatives)` and builds the sorted output using a count array, with offset via `arr[i] - arrayNegVal`.

## Time complexity:
Counting: `O(N)`
Output construction: `O(K)`
Total: `O(N + K)`

## Space complexity:
Count array: `K`
Output array: `N`

| Feature                   | Evaluation                                        |
| ------------------------- | ------------------------------------------------- |
| **Time Complexity**       |  O(n + k)                                         |
| **Space Complexity**      |  O(n + k)                                         |
| **Auxiliary Space**       |  O(n + k)  – includes count[] and output[]        |
| **Stable?**               | ✅ Yes                                            |
| **In-place?**             | ❌ No                                             |
| **Works with negatives?** | ❌ No (requires offset, prone to crash without it)|


`Counting Sort uses more memory, particularly for large ranges. Shalala Sort uses less extra memory, only an output array`.

| Parameter         | Counting Sort      | Shalala Sort       |
| ----------------- | ------------------ | ------------------ |
| Space Complexity  | O(N + K)           | O(N)               |
| Auxiliary Space   | O(K) (count array) | O(1)               |
| Memory Use Result | More memory needed | Less memory needed |



| Category             | Commentary                             |
| -------------------- | -------------------------------------- |
| **Auxiliary Space**  | Very minimal (`O(1)`) – no count array |
| **Simplicity**       | Extremely simple – single nested loops |
| **Stability**        | ✅ Stable                              |
| **Negative support** | ✅ Built-in, no offset required        |



| Feature          | Counting Sort                  | Shalala Sort                            |
| ---------------- | ------------------------------ | --------------------------------------  |
| Negative Support | ✅ With offset                 | ✅ No offset needed                     |
| String Support   | ✅ Good for single-char strings| ✅ Simple for single/multi-char strings |
| Time Complexity  | O(N + K)                       | O(N × K)                                |
| Space Complexity | O(N + K)                       | O(N)                                    |
| Auxiliary Space  | O(K)                           | O(1)                                    |



## Final Comparison:

| Feature          | **Shalala Sort**  | **Counting Sort**         |
| ---------------- | ----------------  | ------------------------  |
| Time Complexity  | ❌  O(N × K)      | ✅  O(N + K)             |
| Space Complexity | ✅  O(N)          | ❌  O(N + K)             |
| Auxiliary Space  | ✅  O(1)          | ❌  O(N + K)             |
| Stability        | ✅ Yes            | ✅ Yes                   |
| In-place         | ❌ No             | ❌ No                    |
| Simplicity       | ✅ Very simple    | ❌ Some complexity       |
| Negative Support | ✅ Built-in       | ❌ Requires offset       |
| String Support   | ✅ Good           | ✅ Good for char strings |



Auxiliary and Total Extra Memory Comparison:

| Meaning                         | Shalala Sort | Counting Sort |
| ------------------------------- | ------------ | ------------- |
| **Auxiliary Space (internal)**  | ✅  O(1)     | ❌  O(N + K)  |
| **Total Extra Memory (output)** | ❌  O(N)     | ❌  O(N + K)  |



Let's present `Shalala Sort` in a comparison table with the most popular sorting algorithms:

| Sorting Algorithm  | Time Complexity (Best) | Time Complexity (Avg) | Time Complexity (Worst) | Space Complexity | Auxiliary Space | In-place  | Stable  |
| ------------------ | ---------------------- | --------------------- | ----------------------- | ---------------- | --------------- | --------  | ------  |
| **Bubble Sort**    | O(N)                   | O(N²)                 | O(N²)                   | O(1)             | O(1)            | ✅        | ✅      |
| **Insertion Sort** | O(N)                   | O(N²)                 | O(N²)                   | O(1)             | O(1)            | ✅        | ✅      |
| **Quick Sort**     | O(N log N)             | O(N log N)            | O(N²)                   | O(log N)         | O(log N)        | ✅        | ❌      |
| **Heap Sort**      | O(N log N)             | O(N log N)            | O(N log N)              | O(1)             | O(1)            | ✅        | ❌      |
| **Selection Sort** | O(N²)                  | O(N²)                 | O(N²)                   | O(1)             | O(1)            | ✅        | ❌      |
| **Shalala Sort**   | O(K × N)               | O(K × N)              | O(K × N)                | O(N)             | O(1)            | ❌        | ✅      |
| **Merge Sort**     | O(N log N)             | O(N log N)            | O(N log N)              | O(N)             | O(N)            | ❌        | ✅      |
| **Counting Sort**  | O(N + K)               | O(N + K)              | O(N + K)                | O(N + K)         | O(N + K)        | ❌        | ✅      |
| **Radix Sort**     | O(N × d)               | O(N × d)              | O(N × d)                | O(N + K)         | O(N + K)        | ❌        | ✅      |
| **Bucket Sort**    | O(N + K)               | O(N + K)              | O(N²)                   | O(N + K)         | O(N + K)        | ❌        | ✅      |



