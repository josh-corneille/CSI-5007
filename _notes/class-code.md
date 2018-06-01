---
layout: post
mathjax: true
---

All python code that appears in the slides.

- [Bubble Sort](#bubble-sort)
- [Insertion Sort](#insertion-sort)
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
    - [Iterative](#iterative)
        - [Explanation](#explanation)
            - [Loop Invariant](#loop-invariant)
    - [Recursive](#recursive)
- [Find in Log M](#find-in-log-m)
    - [Python Interpretation](#python-interpretation)
- [Contiguous Subarray Sum](#contiguous-subarray-sum)
    - [Brute Force](#brute-force)
    - [Pre-Computing](#pre-computing)
    - [Recursive](#recursive)
    - [Scanning](#scanning)
        - [Return Indices](#return-indices)
- [Matrix Multiplication](#matrix-multiplication)
    - [Strassen's Algorithm](#strassens-algorithm)
- [Merge Sort](#merge-sort)
- [Shell Sort](#shell-sort)
- [Abstract (Selection) Sort](#abstract-selection-sort)
- [Min-Heap Heapsort](#min-heap-heapsort)
- [Quicksort](#quicksort)
        - [Runtime](#runtime)
        - [Faster Quicksort](#faster-quicksort)
- [Bin Sort](#bin-sort)
- [Radix Sort](#radix-sort)
- [Min and Max](#min-and-max)
- [Dutch Flag](#dutch-flag)
- [Squaring without Multiplication](#squaring-without-multiplication)
        - [Iterative](#iterative)
        - [Recursive](#recursive)
    - [Doubling Formula](#doubling-formula)
        - [Iterative](#iterative)
        - [Iterative](#iterative)

# Bubble Sort
* 1 - Introduction

```py
def bs(a):
    n = len(a)
    for i in range(n-1):
        for j in range(i+1, n):
            if a[i] > a[j]:
                a[i], a[j] = a[j], a[i]
    return a
```

* 2 - Informal Correctness
```py
def bs0(a):
    s, n = True, len(a)
    while s:
        s = False
        for i in range(n-1, 0, -1):
            if a[i] < a[i -1]:
                a[i], a[i-1] = a[i-1], a[i]
                s = True
    return a
```

# Insertion Sort
* 3 - Runtime Computations
```py
def isort(a):
    for i in range(1, len(a)):
        currentvalue = a[i]
        position = i

        while position > 0 and a[position - 1] > currentvalue:
            a[position] = a[position - 1]
            position = position - 1

        a[position] = currentvalue
    return a
```

```py
def insertion_sort(array):
    for slot in range(1, len(array)): 
        value = array[slot]
        test_slot = slot - 1
        while test_slot > -1 and array[test_slot] > value:
            array[test_slot + 1] = array[test_slot]
            test_slot = test_slot - 1
        array[test_slot + 1] = value
    return array
```

# Linear Search

* 5 - Divide and Conquer
```py
def s(a, e):
    for i in range(len(a)):
        if a[i] == e:
            return i
    return -1
```

# Binary Search

* 5 - Divide and Conquer

## Iterative
```py
def bsearch(a, e):
    l, h = 0, len(a) - 1
    while l <= h:
        m = (l+h)//2
        if a[m] == e:
            return m
        if a[m] < e:
            l = m+1
        else:
            h = m-1
    return -1
```

### Explanation
At the start the interval `[l,h]` encompasses the whole array and it is
reduced at every step. Therefore, the algorithm terminates.
#### Loop Invariant
If element `e` is in the array, it is in `a[l,..,h]`. The algorithm considers
the mid-point and compares its element with the target `e`. If the element is
too large, the target must be in the lower half and reduces `h`. If the
element is too small, the element must be in the upper half and increases
`l`. It stops when it finds `e` or when the array is exhausted.

* 5 - Divide and Conquer

## Recursive
```py
def rbsearch(a, e, l, h):
    if l > h:
        return -1
    else:
        m = (l+h)//2
        if a[m] == e:
            return m
        if a[m] < e:
            return rbsearch(a, e, m+1, h)
        else:
            return rbsearch(a, e, l, m-1)
```

# Find in Log M
```lisp
( defun find−in−log−m ( v )
    ( let ∗ ( (max (1− ( length v ) ) )
        ( symbol−m ( a r ef v max) ) )
    ( labels ( ( bound−in−log−m ( i )
        ( if ( eql symbol−m ( aref v (− max i ) ) )
            ( bound−in−log−m (∗ 2 i ) )
            i ) ) )
( let ( ( min ( bound−in−log−m 1 ) ) )
    (1+ ( nth−value 1 ( com.informatimago.common−lisp
( lambda (index)
( cond
    ( ( eql symbol−m ( aref v index
    ( ( eql symbol−m ( aref v (1+
    ( t +1)))
    min max ) ) ) ) ) ) )

```
## Python Interpretation
```py
def findInLogM(v):
        max = len(v) - 1
        symbol-m = v[max]      
        def boundInLogM(i):  
                if (symbol-m == v[max-i]:
                        bound-in-log-m(i*2)
                else:
                        return i
```

# Contiguous Subarray Sum
* 6 - Recap
## Brute Force
```py
def A0(a):
    n, largest = len(a), 0
    for i in range(n):
        for j in range(i, n):
            s = sum(a[i:j+1])
            largest = max(largest, s)
    return largest
```
Runtime: $\Theta(n^3)$

```py
def A1(a):
    n, largest = len(a), 0
    for i in range(n):
        s = 0
        for j in range(i, n):
            s += a[j]
            largest = max(largest, s)
    return largest
```
Runtime: $\Theta(n^2)$

## Pre-Computing
```py
def A2(a):
    c = [0] * (len(a) + 1)
    for i in range(len(a)):
        c[i+1] = c[i]+a[i]
    largest = 0
    for i in range(len(a)):
        for j in range(i, len(a)):
            s = c[j+1]-c[i]
            largest = max(s, largest)
    return largest
```
Runtime: $\Theta(n^2)$

## Recursive
```py
def A3(a):
    n = len(a)
    if n == 0:
        return 0
    if n == 1:
        return max(0, a[0])
    m = n//2
    MA, MB, s, MCl = A3(A[0:m]), A3(a[m:n]), 0, 0
    for i in range(m, 0, -1):
        s = s + a[i]
        MCl = max(s, MCl)
    s, MCr = 0, 0
    for i in range(m+1, n):
        s = s + a[i]
        MCr = max(MCr, s)
    return max(MA, MB MCl + MCr)
```

Runtime: $\Theta(nlog_2(n))$

## Scanning
```py
def A4(a):
    mf = 0
    mh = 0
    n = len(a)
    for i in range(n):
        mh = max(mh+a[i], 0)
        mf = max(mf, mh)
    return mf
```
Runtime: $\Theta(n)$

### Return Indices
```py
def A4ix(a):
    mf = 0
    mh = 0
    n = len(a)
    s = 0
    e = -1
    sh = 0
    eh = -1
    for i in range(n):
        if mh+a[i] > 0:
            eh = i
            mh += a[i]
        else:
            mh = 0
            sh = i + 1
        if mh > mf:
            mf = mh
            s = sh
            e = eh
    return mf, s, e
```


# Matrix Multiplication
* 8 - Matrix Multiplication
```py
def size(A):
    return len(A), len(A[0])

def mm(A, B):
    (p, q) = size(A)
    (q, r) = size(B)
    C = [[0 for _ in range(r)] for _ in range(p)]
    for i in range(p):
        for j in range(q):
            for k in range(r):
                C[i][k] += A[i][j] * B[j][k]
    return C
```
Runtime: 
$\Theta(pqr)$

$\Theta(n^3)$ if all matrices are size $n$

## Strassen's Algorithm
```py
def strassen(A, B, n, n0):
    if n <= n0:
        return mm(A,B)
    else:
        m = n // 2
        u = range(m)
        v = range(m+1, n)
        P1 = strassen(A(u, u) + A(u, v), B(u, u) + B(u, v), m, n0)
        P2 = strassen(A(v, u) + A(v, v), B(u, u), m, n0)
        P3 = strassen()
        #Missing P4, P5, P6, P7
        #P4 = A22(B21 - B11)
        #P5 = B22(A11 + A12)
        #P6 = (A21 - A11)(B11 + B12)
        #P7 = (A12 - A22)(B21 + B22)
        C(u, u) = P1 + P4 - P5 + P7
        C(u, v) = P3 + P5
        C(v, u) = P2 + P4
        C(v, v) = P1 + P3 - P2 + P6
    return C
```
Runtime: $\Theta(n^{2.8})$

# Merge Sort
* 9 - The Heap and the Quick

```py
def merge_sort(array):
    if len(array) <= 1:
        return array
    else:
        m = floor(len(array) / 2)
   return merge(array[0:m], merge_sort(array[m:]))

def merge(a, b):
    ia = 0
    ib = 0
    ic = 0
    na = len(a)
    nb = len(b)
    nc = na + nb
    c = [0] * nc
    while(ic < nc):
        if(ia < na):
            if(ib < nb):
                if(a[ia] < b[ib]):
                    c[ic] = a[ia]
                    ic += 1
                    ia += 1
                else:
                    c[ic] = b[ib]
                    ic += 1
                    ib += 1
            else:
                c[ic] = a[ia]
                ic += 1
                ia += 1
        else:
            c[ic] = b[ib]
            ic += 1
            ib += 1
    return c
```

Runtime: $\Theta(nlog(n))$

# Shell Sort
* 9 - The Heap and the Quick
```py
def shellSort(gaps):
    for gap in gaps:
        for i in range(gap):
            gaplSort(lst, i, gap)

def gaplSort(lst, start, gap):
    for i in range(start + gap, len(lst), gap):
        v = lst[i]
        p = i
        while p >= gap & lst[p-gap] > v:
            lst[p] = lst[p - gap]
            p = p - gap
        lst[p] = v
```
Runtime: $O(n^{3/2})$ to $O(nlog^2(n))$

# Abstract (Selection) Sort
* 9 - The Heap and the Quick
```py
def abstractSort(a):
    n = len(a)
    na = []
    for i in range(n):
        smallest = extractSmallestAndDelete(a)
        na.append(smallest)
    return na
```

# Min-Heap Heapsort
* 9 - The Heap and the Quick
```py
def newheap(n):
    return [0]*(n+1)

def insert(a, e):
    a[0] = a[0] + 1
    a[a[0]] = e
    heapfixup(a,a[0])

def extractsmallest(a):
    e = a[1]
    a[1] = a[a[0]]
    a[0] = a[0] - 1
    heapfixdown(a, 1)
    return e
```

```py
def heapfixup(a,i):
    while i > 1:
        parent = floor(i/2)
        if a[p] > a[i]:
            a[p], a[i] = a[i], a[p]
            i = p
        else:
            return
```
Runtime: $\Theta(log(n))$

```py
def heapfixdown(a, i):
    while (2 * i) <= a[0]:
        c = 2 * i
        if (c + 1) <= a[0]:
            if a[c + 1] < a[c]:
                c += 1
            if a[i] > a[c]:
                a[i], a[c] = a[c], a[i]
                i = c
            else:
                return
```
Runtime: $\Theta(log(n))$

```py
def heapsort(x):
    n = len(x)
    a = newheap(n)
    for i in range(n):
        insert(a, x[i])
    for i in range(n):
        x[i] = extractsmallest(a)
    return x
```
Runtime: $\Theta(nlog(n))$

# Quicksort
* 9 - The Heap and the Quick
```py
def partition(a, l, u):
    t = a[l]
    m = l
    for i in range(l+1, u+1):
        if a[i] < t:
            m = m + 1
            a[i] = a[m]
            a[m] = a[i]
        a[m] = a[l]
        a[l] = a[m]
    return m

def qsort0(a, l=0, u=None):
    if u is None:
        u = len(a) - 1
    if l < u:
        m = partition(a, l, u)
        qsort0(a, l, m-1)
        qsort0(a, m+1, u)
    return a
```
### Runtime

Best Case: $O(nlogn)$

Worst Case: $O(n^2)$

Average Case: $O(nlogn)$

### Faster Quicksort

```py
def qsort1(a, l=0, u=None):
    if u is None:
        u = len(a) - 1
    if l < u:
        m = partition(a, l, u)
        qsort0(a, l, m-1)
        qsort0(a, m+1, u)
    elif l < u:
        insertion_sort(a, l, u)
    return a
```

# Bin Sort
* 10 - Limits of Sorting
```py
def binsort(a):
    bins = [a]
    for l in range(len(a[0]) - 1, -1, -1):
        binsTwo = [[] for _ in range(10)]
        for bin in bins:
            for e in bin:
                binsTwo[e[l]].append(e)
        bins = binTwo
    return [e for bin in bins for e in bin]
```
Running Time:
$\Theta(n)$

# Radix Sort
* 10 - Limits of Sorting
```py
def radixSort(a, d):
    for i in range(d):
        sortStably(a, lambda a0, a1 : a0[i] < a1[i])
```

# Min and Max
* 10 - Limits of Sorting
```py
def minandmax(a):
    min = max = a [0]
    for i in range(1, len(a)):
        if a[i] < min:
            min = a[i]
        elif a[i] > max:
            max = a[i]
    return min, max
```
Number of comparisons is between $n$ and $2n$

```py
def minandmax2(a):
    min, max = a[0], a[1] if a[0] < a[1] else a[1], a[0]
    for i in range(2, len(a), 2):
        j = i + 1
        if a[i] < a[j]:
            if a[i] < min:
                min = a[i]
            if a[j] > max:
                max = a[j]
        else:
            if a[j] < min:
                min = a[j]
            if a[i] > max:
                max = a[i]
    return min, max
```
Number of comparisons is $3n/2$

# Dutch Flag
```py
def dutch_flag(a, red = 0, white = 1, blue = 2):
    r = 0
    w = 0
    b = len(a) - 1
    n = len(a) - 1
    while w <= b:
        if a[w] == red:
            a[r], a[w] = a[w], a[r]
            r += 1
            w += 1
        elif a[w] == blue:
            a[b], a[w] = a[w], a[b]
            b -= 1
        elif a[w] == white:
            w += 1
    return a
```

# Squaring without Multiplication
### Iterative
```py
def sq_no_mult_0(n):
    n2 = 0
    for i in range(n):
        n2 += n
    return n2
```

### Recursive
```py
def sq_no_mult_r(n, count = -1, n2 = 0):
    if count == 0:
        return n2
    else:
        if count == -1:
            count = n
        return sq_no_mult_r(n, count-1, n2 + n)
```

## Doubling Formula
### Iterative
```py

```

### Iterative
```py
def sq_no_mult_1(n):
    #Assumes n = 2^k
    n2 = n
    n1 = 1
    while n1 < n:
        n1 += n1
        n2 += n2
    return n2
```

```py
def sq_no_mult_2(n):
    n2 = n
    n1 = 1
    while n1 + n1 < n:
        n1 += n1
        n2 += n2
    m = n - n1
    n3 = n1 + m
    n4 = 0
    for i in range(m):
        n4 += n3
    return n2 + n4
```