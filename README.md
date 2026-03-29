# Data Structures & Algorithms Cheatsheet — Java (End-to-End)

> A single-file, in-depth reference covering every major DSA topic you need for coding interviews and competitive programming — **theory + code**. Every snippet is **Java 17+** compatible.

---

## Table of Contents

1. [Big-O Complexity](#1-big-o-complexity)
2. [Arrays](#2-arrays)
3. [Strings](#3-strings)
4. [Linked Lists](#4-linked-lists)
5. [Stacks](#5-stacks)
6. [Queues](#6-queues)
7. [Hashing](#7-hashing)
8. [Trees (Binary Tree & BST)](#8-trees-binary-tree--bst)
9. [Heaps / Priority Queues](#9-heaps--priority-queues)
10. [Graphs](#10-graphs)
11. [Tries](#11-tries)
12. [Sorting Algorithms](#12-sorting-algorithms)
13. [Searching Algorithms](#13-searching-algorithms)
14. [Two Pointers](#14-two-pointers)
15. [Sliding Window](#15-sliding-window)
16. [Recursion & Backtracking](#16-recursion--backtracking)
17. [Dynamic Programming](#17-dynamic-programming)
18. [Greedy Algorithms](#18-greedy-algorithms)
19. [Bit Manipulation](#19-bit-manipulation)
20. [Math & Number Theory](#20-math--number-theory)
21. [Union-Find (Disjoint Set)](#21-union-find-disjoint-set)
22. [Segment Trees & BITs](#22-segment-trees--bits)
23. [Intervals](#23-intervals)
24. [Monotonic Stack / Queue](#24-monotonic-stack--queue)
25. [Topological Sort](#25-topological-sort)
26. [Common Patterns Quick Reference](#26-common-patterns-quick-reference)
27. [Java Tips & Pitfalls for DSA](#27-java-tips--pitfalls-for-dsa)
28. [Common Mistakes & Edge Cases](#28-common-mistakes--edge-cases)
29. [Must-Do 50 Problems — Revision Checklist](#29-must-do-50-problems--revision-checklist)

---

## 1. Big-O Complexity

### Theory

**Big-O notation** describes the upper bound of an algorithm's growth rate as input size approaches infinity. It answers: *"How does runtime/memory scale when input grows?"*

- **Big-O (O)** — upper bound (worst case). "At most this fast."
- **Big-Omega (Ω)** — lower bound (best case). "At least this fast."
- **Big-Theta (Θ)** — tight bound (average). "Exactly this fast."

In interviews, we almost always talk about **Big-O** (worst-case).

**Rules for calculating Big-O:**
1. **Drop constants** — O(2n) → O(n). Constants don't matter at scale.
2. **Drop lower-order terms** — O(n² + n) → O(n²). The dominant term wins.
3. **Different inputs → different variables** — if two arrays of size `a` and `b`, it's O(a + b) not O(n).
4. **Sequential steps add** — O(a) + O(b) = O(a + b).
5. **Nested steps multiply** — O(a) inside O(b) = O(a · b).
6. **Recursive calls** — use the recurrence relation. e.g. T(n) = 2T(n/2) + O(n) → O(n log n) via Master Theorem.

**Master Theorem** (for recurrences T(n) = aT(n/b) + O(nᵈ)):
- If d < log_b(a): T(n) = O(n^log_b(a))
- If d = log_b(a): T(n) = O(nᵈ · log n)
- If d > log_b(a): T(n) = O(nᵈ)

### Growth Rates (fastest → slowest)

| Notation | Name | Example | n=1000 ops |
|---|---|---|---|
| O(1) | Constant | Hash lookup, array index | 1 |
| O(log n) | Logarithmic | Binary search | ~10 |
| O(n) | Linear | Single loop | 1,000 |
| O(n log n) | Linearithmic | Merge sort, heap sort | ~10,000 |
| O(n²) | Quadratic | Nested loops | 1,000,000 |
| O(2ⁿ) | Exponential | Subsets / power set | Astronomical |
| O(n!) | Factorial | Permutations | Astronomical |

**Rough guide for competitive programming:**
- n ≤ 10 → O(n!) or O(2ⁿ) is fine
- n ≤ 20 → O(2ⁿ) is fine
- n ≤ 500 → O(n³) is fine
- n ≤ 5,000 → O(n²) is fine
- n ≤ 10⁶ → O(n log n) is needed
- n ≤ 10⁸ → O(n) is needed
- n > 10⁸ → O(log n) or O(1) is needed

### How to Analyze

```java
// O(1)
int x = arr[0];

// O(n)
for (int i = 0; i < n; i++) { }

// O(n²)
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++) { }

// O(log n) — halving the search space each time
int lo = 0, hi = n - 1;
while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    // ...
}

// O(n log n) — sort then iterate
Arrays.sort(arr);       // O(n log n)
for (int x : arr) { }  // O(n)
```

### Space Complexity

Space complexity measures extra memory used by an algorithm (excluding input).

| Structure | Space |
|---|---|
| Primitive variable | O(1) |
| 1-D array of size n | O(n) |
| 2-D array n×m | O(n·m) |
| HashMap of n entries | O(n) |
| Recursion depth d | O(d) stack frames |

**In-place algorithms** use O(1) extra space (e.g., insertion sort, quicksort's partitioning).

### Amortized Analysis

Amortized analysis considers the average cost per operation over a sequence of operations, even if some individual operations are expensive.

- **ArrayList.add()** — O(1) amortized (doubles capacity when full; occasional O(n) copy).
- **HashMap.put()** — O(1) amortized (rehash at load factor; occasional O(n)).

The **Aggregate Method**: Total cost of n operations / n = amortized cost per operation.

---

## 2. Arrays

### Theory

An **array** is a contiguous block of memory storing elements of the same type. Each element is accessed by its index in O(1) via pointer arithmetic: `address = base + index × element_size`.

**Properties:**
- **Fixed size** (in Java, primitive arrays). Once allocated, cannot resize.
- **Random access** — O(1) read/write by index. This is the core advantage.
- **Cache-friendly** — elements are stored contiguously in memory, leading to excellent CPU cache performance (spatial locality).
- **Insertion/deletion** — O(n) because elements must be shifted.

**Static Array vs Dynamic Array (ArrayList):**

| Operation | Static Array | ArrayList |
|---|---|---|
| Access by index | O(1) | O(1) |
| Set by index | O(1) | O(1) |
| Append | N/A (fixed) | O(1) amortized |
| Insert at index | O(n) | O(n) |
| Delete at index | O(n) | O(n) |
| Search (unsorted) | O(n) | O(n) |
| Search (sorted) | O(log n) | O(log n) |

**ArrayList internals:** Starts with a default capacity (10 in Java). When full, it allocates a new array of 1.5× the old capacity, copies all elements, and discards the old array. This gives O(1) amortized append.

**Key Techniques on Arrays:**
- **Prefix Sum** — precompute cumulative sums to answer range-sum queries in O(1).
- **Two Pointers** — process from both ends or with fast/slow pointers.
- **Sliding Window** — maintain a window over a subarray for stream-like problems.
- **Kadane's Algorithm** — maximum subarray sum in O(n).
- **Dutch National Flag** — 3-way partition in a single pass.
- **Sorting** — unlocks binary search, two-pointer, and greedy approaches.

### Java Essentials

```java
int[] arr = new int[10];                   // fixed size, default 0
int[] arr2 = {1, 2, 3, 4, 5};
int[][] grid = new int[3][4];             // 2-D array
int len = arr.length;                      // field, not method

// Copy
int[] copy = Arrays.copyOf(arr, arr.length);
int[] rangeCopy = Arrays.copyOfRange(arr, 1, 4); // [1, 4)

// Fill
Arrays.fill(arr, -1);

// Sort
Arrays.sort(arr);                          // O(n log n) dual-pivot quicksort
Arrays.sort(arr, 2, 6);                   // sort subrange [2, 6)

// Sort 2D by first element
int[][] intervals = {{1,3},{2,6},{8,10}};
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

// Binary search (array MUST be sorted)
int idx = Arrays.binarySearch(arr2, 3);   // returns index or -(insertion)-1

// ArrayList (dynamic array)
List<Integer> list = new ArrayList<>();
list.add(10);           // O(1) amortized
list.get(0);            // O(1)
list.set(0, 20);        // O(1)
list.remove(0);         // O(n) — shifts elements
list.size();
Collections.sort(list);
```

### Prefix Sum

**Idea:** Build a cumulative sum array so that any range sum `[l, r]` can be answered in O(1) using `prefix[r+1] - prefix[l]`. Preprocessing: O(n). Each query: O(1).

```java
int[] prefix = new int[n + 1];
for (int i = 0; i < n; i++)
    prefix[i + 1] = prefix[i] + arr[i];

// Range sum query [l, r] inclusive in O(1)
int rangeSum = prefix[r + 1] - prefix[l];
```

### 2-D Prefix Sum

**Idea:** Extend prefix sums to a matrix. Use inclusion-exclusion to compute the sum of any sub-rectangle in O(1) after O(m·n) preprocessing.

```java
int[][] pre = new int[m + 1][n + 1];
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        pre[i][j] = grid[i-1][j-1] + pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1];

// Sum of submatrix (r1,c1) to (r2,c2) — 0-indexed original
int sum = pre[r2+1][c2+1] - pre[r1][c2+1] - pre[r2+1][c1] + pre[r1][c1];
```

### Kadane's Algorithm (Maximum Subarray Sum)

**Idea:** At each position, decide: "Is it better to extend the previous subarray or start a new one here?" Keep a running current sum and a global max. O(n) time, O(1) space.

**Why it works:** If the current sum becomes negative, it can only drag down future sums — so we reset.

```java
int maxSubarraySum(int[] nums) {
    int maxSum = nums[0], curSum = nums[0];
    for (int i = 1; i < nums.length; i++) {
        curSum = Math.max(nums[i], curSum + nums[i]);
        maxSum = Math.max(maxSum, curSum);
    }
    return maxSum;
}
```

### Dutch National Flag (3-Way Partition)

**Idea:** Partition an array into three regions using three pointers (lo, mid, hi). Elements before `lo` are 0s, between `lo` and `mid` are 1s, after `hi` are 2s. Single pass — O(n) time, O(1) space.

```java
void sortColors(int[] nums) {
    int lo = 0, mid = 0, hi = nums.length - 1;
    while (mid <= hi) {
        if (nums[mid] == 0) swap(nums, lo++, mid++);
        else if (nums[mid] == 1) mid++;
        else swap(nums, mid, hi--);
    }
}
```

### Rotate Array by k Positions

**Idea:** Right rotation by k can be done in O(n) time, O(1) space using three reverses:
1. Reverse the entire array.
2. Reverse the first k elements.
3. Reverse the remaining n-k elements.

```java
void rotate(int[] nums, int k) {
    int n = nums.length;
    k %= n;
    reverse(nums, 0, n - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, n - 1);
}

void reverse(int[] nums, int l, int r) {
    while (l < r) { int t = nums[l]; nums[l++] = nums[r]; nums[r--] = t; }
}
```

### Cyclic Sort

**Theory:** When elements are in the range `[0, n]` or `[1, n]`, each element has a "correct" index it should sit at. Cyclic Sort places each number at its correct index in O(n) time, O(1) space by repeatedly swapping the current element to its correct position.

**When to use:** Find missing number, find duplicates, find first missing positive — any problem where values map to indices.

```java
// Place each number at index == its value (for range [0, n-1])
void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correct = nums[i];
        if (correct >= 0 && correct < nums.length && nums[i] != nums[correct])
            swap(nums, i, correct);
        else
            i++;
    }
}
```

### Find Missing Number (range [0, n])

```java
int missingNumber(int[] nums) {
    int i = 0, n = nums.length;
    while (i < n) {
        if (nums[i] < n && nums[i] != i) swap(nums, i, nums[i]);
        else i++;
    }
    for (i = 0; i < n; i++)
        if (nums[i] != i) return i;
    return n;
}
```

### Find All Duplicates (range [1, n])

```java
List<Integer> findDuplicates(int[] nums) {
    List<Integer> result = new ArrayList<>();
    for (int num : nums) {
        int idx = Math.abs(num) - 1;
        if (nums[idx] < 0) result.add(idx + 1);
        else nums[idx] = -nums[idx];
    }
    return result;
}
```

### First Missing Positive

**Idea:** Ignore negatives and numbers > n. Place each valid number at `nums[i] - 1`. The first index where `nums[i] != i + 1` is the answer. O(n) time, O(1) space.

```java
int firstMissingPositive(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++)
        while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i])
            swap(nums, i, nums[i] - 1);
    for (int i = 0; i < n; i++)
        if (nums[i] != i + 1) return i + 1;
    return n + 1;
}
```

---

## 3. Strings

### Theory

A **string** is a sequence of characters. In Java, `String` is **immutable** — every modification creates a new object. This has critical implications:

**Immutability:**
- `String` objects cannot be changed after creation.
- String concatenation in a loop is O(n²) because each `+` creates a new string.
- Use `StringBuilder` for mutable string building — O(1) amortized append.

**String Pool:** Java maintains a string constant pool. Literals like `"hello"` are interned and reused. This is why you must use `.equals()` (not `==`) for comparison — `==` compares references, not content.

**Encoding:** Java strings use UTF-16 internally. Each `char` is 2 bytes. For ASCII problems, this doesn't matter; for Unicode, be aware of surrogate pairs.

**Key Techniques:**
- **Frequency Array** — use `int[26]` for lowercase letters, `int[128]` for ASCII. O(1) lookups.
- **Two Pointers** — palindrome check, reverse.
- **Sliding Window** — longest substring without repeating chars, minimum window substring.
- **Hashing** — Rabin-Karp rolling hash for pattern matching.
- **KMP Algorithm** — O(n + m) pattern matching using a failure function (LPS array).

**String Comparison Complexity:**
- `s.equals(t)` — O(min(|s|, |t|))
- `s.compareTo(t)` — O(min(|s|, |t|))
- `s.hashCode()` — O(|s|) first time, cached after

### Java Essentials

```java
String s = "hello";
char c = s.charAt(0);            // 'h'
int len = s.length();            // method (not field)
String sub = s.substring(1, 3);  // "el" — [1, 3)
String lower = s.toLowerCase();
String upper = s.toUpperCase();
boolean eq = s.equals("hello");  // NEVER use ==
int cmp = s.compareTo("world"); // lexicographic
String[] parts = s.split(",");
String joined = String.join("-", parts);
char[] chars = s.toCharArray();
String fromChars = new String(chars);
boolean starts = s.startsWith("he");
boolean ends = s.endsWith("lo");
int idx = s.indexOf("ll");      // 2, or -1
s.contains("ell");              // true
s.trim();                       // remove leading/trailing whitespace

// StringBuilder — mutable, O(1) amortized append
StringBuilder sb = new StringBuilder();
sb.append("hello");
sb.append(' ');
sb.append("world");
sb.reverse();
sb.deleteCharAt(0);
sb.insert(0, 'H');
String result = sb.toString();
```

### Character Operations

```java
Character.isLetter('a');      // true
Character.isDigit('3');       // true
Character.isLetterOrDigit('a');
Character.isLowerCase('a');
Character.isUpperCase('A');
Character.toLowerCase('A');   // 'a'
char c = 'a';
int idx = c - 'a';           // 0 (useful for frequency arrays)
```

### Frequency Array for Lowercase Letters

```java
int[] freq = new int[26];
for (char c : s.toCharArray())
    freq[c - 'a']++;
```

### Check Anagram

**Idea:** Two strings are anagrams if they have the same character frequencies. Use a single frequency array — increment for string `s`, decrement for string `t`. If all zero, they're anagrams. O(n) time, O(1) space (26 letters).

```java
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] count = new int[26];
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
        count[t.charAt(i) - 'a']--;
    }
    for (int c : count) if (c != 0) return false;
    return true;
}
```

### Palindrome Check

**Idea:** A palindrome reads the same forwards and backwards. Use two pointers from both ends moving inward. O(n) time, O(1) space.

```java
boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s.charAt(l++) != s.charAt(r--)) return false;
    }
    return true;
}
```

### KMP Pattern Matching — O(n + m)

**Theory:** The **Knuth-Morris-Pratt** algorithm avoids re-scanning matched characters by precomputing a **Longest Proper Prefix which is also Suffix (LPS)** array. When a mismatch occurs at position `j` in the pattern, instead of restarting, we jump to `lps[j-1]` — the longest prefix of the pattern that is also a suffix of the matched portion.

**Why it's O(n + m):** The text pointer never goes backward. Each character is compared at most twice.

**LPS Array Example:** Pattern "ABCABD" → LPS = [0, 0, 0, 1, 2, 0]
- At "ABCAB", the prefix "AB" is also a suffix, so LPS[4] = 2.

```java
int[] buildLPS(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    int len = 0, i = 1;
    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            lps[i++] = ++len;
        } else if (len > 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}

int kmpSearch(String text, String pattern) {
    int[] lps = buildLPS(pattern);
    int i = 0, j = 0;
    while (i < text.length()) {
        if (text.charAt(i) == pattern.charAt(j)) {
            i++; j++;
            if (j == pattern.length()) return i - j; // found
        } else if (j > 0) {
            j = lps[j - 1];
        } else {
            i++;
        }
    }
    return -1; // not found
}
```

### Rabin-Karp Hashing

**Theory:** **Rabin-Karp** uses a **rolling hash** to compare the pattern hash against substring hashes of the text. If hashes match, verify with an actual comparison (to handle collisions).

**Rolling hash:** When the window slides by one character, the new hash is computed in O(1):
`newHash = (oldHash - text[i] × baseᵐ⁻¹) × base + text[i + m]`

**Time:** O(n + m) average, O(nm) worst case (many collisions). In practice, collisions are rare with a good hash.

```java
int rabinKarp(String text, String pattern) {
    int n = text.length(), m = pattern.length();
    long MOD = 1_000_000_007, BASE = 31;
    long patHash = 0, txtHash = 0, power = 1;

    for (int i = 0; i < m; i++) {
        patHash = (patHash * BASE + pattern.charAt(i)) % MOD;
        txtHash = (txtHash * BASE + text.charAt(i)) % MOD;
        if (i > 0) power = power * BASE % MOD;
    }

    for (int i = 0; i <= n - m; i++) {
        if (patHash == txtHash && text.substring(i, i + m).equals(pattern))
            return i;
        if (i + m < n) {
            txtHash = (txtHash - text.charAt(i) * power % MOD + MOD) % MOD;
            txtHash = (txtHash * BASE + text.charAt(i + m)) % MOD;
        }
    }
    return -1;
}
```

---

## 4. Linked Lists

### Theory

A **linked list** is a linear data structure where each element (node) contains data and a pointer (reference) to the next node. Unlike arrays, elements are **not stored contiguously** in memory.

**Types:**
- **Singly Linked List** — each node points to the next. Traversal is one-way.
- **Doubly Linked List** — each node points to both next and previous. Traversal is two-way.
- **Circular Linked List** — the last node points back to the first.

**Array vs Linked List:**

| Operation | Array | Linked List |
|---|---|---|
| Access by index | O(1) | O(n) |
| Insert at head | O(n) | O(1) |
| Insert at tail | O(1)* | O(1)** |
| Insert at middle | O(n) | O(1)*** |
| Delete | O(n) | O(1)*** |
| Memory | Contiguous, cache-friendly | Scattered, extra pointer overhead |

\* amortized for dynamic array | \** if tail pointer maintained | \*** if you already have a reference to the node

**When to use Linked Lists:**
- Frequent insertions/deletions at arbitrary positions (and you have a reference to the node).
- Implementing stacks, queues, LRU cache.
- When you don't know the size in advance and don't need random access.

**Key Techniques:**
- **Dummy/Sentinel Node** — simplifies edge cases (empty list, head deletion). Create `new ListNode(0)` as a fake head.
- **Fast & Slow Pointers (Floyd's)** — find the middle, detect cycles, find cycle start.
- **Reversal** — iterative (three pointers) or recursive.
- **Merge** — merge two sorted lists using a dummy node.

### Node Definition

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```

### Reverse a Linked List — Iterative

**Idea:** Use three pointers: `prev`, `curr`, `next`. At each step, reverse `curr.next` to point to `prev`, then advance all pointers. O(n) time, O(1) space.

```java
ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### Reverse — Recursive

**Idea:** Recursively reverse the rest of the list, then fix the pointers. The base case is when we reach the last node (which becomes the new head). O(n) time, O(n) space (call stack).

```java
ListNode reverseRecursive(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode newHead = reverseRecursive(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

### Detect Cycle (Floyd's Tortoise & Hare)

**Theory:** Use two pointers — slow moves 1 step, fast moves 2 steps. If there's a cycle, they **must** meet. If fast reaches null, there's no cycle.

**Why they meet:** In a cycle of length `C`, the fast pointer gains 1 step per iteration on the slow pointer. So after at most `C` iterations inside the cycle, they collide.

```java
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

### Find Cycle Start

**Theory:** After slow and fast meet, reset slow to head. Now move both one step at a time — they'll meet at the cycle's start.

**Mathematical proof:** Let the distance from head to cycle start be `a`, and from cycle start to meeting point be `b`, and the remaining cycle length be `c`. At meeting: slow traveled `a + b`, fast traveled `a + b + c + b`. Since fast moves 2× speed: `2(a + b) = a + 2b + c` → `a = c`. So moving from head and from meeting point by 1 step converges at the cycle start.

```java
ListNode detectCycleStart(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }
    return null;
}
```

### Find Middle

**Idea:** Slow pointer moves 1 step, fast moves 2 steps. When fast reaches the end, slow is at the middle.

```java
ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow; // for even length, returns second middle
}
```

### Merge Two Sorted Lists

**Idea:** Use a dummy node. Compare heads of both lists, attach the smaller one, advance that pointer. O(n + m) time.

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), curr = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
        else { curr.next = l2; l2 = l2.next; }
        curr = curr.next;
    }
    curr.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

### Merge Sort a Linked List — O(n log n)

**Idea:** Recursively split the list at the middle (using fast/slow), sort each half, then merge. This is the preferred O(n log n) sort for linked lists because it doesn't need random access (unlike quicksort/heapsort on arrays).

```java
ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode mid = findMiddle(head);
    ListNode right = mid.next;
    mid.next = null; // cut
    return mergeTwoLists(sortList(head), sortList(right));
}
```

### Remove N-th Node From End

**Idea:** Use two pointers `n+1` apart. When the fast pointer reaches the end, slow is just before the target node. Dummy node handles the edge case of removing the head.

```java
ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    ListNode fast = dummy, slow = dummy;
    for (int i = 0; i <= n; i++) fast = fast.next;
    while (fast != null) { fast = fast.next; slow = slow.next; }
    slow.next = slow.next.next;
    return dummy.next;
}
```

### Doubly Linked List Node

```java
class DLLNode {
    int key, val;
    DLLNode prev, next;
    DLLNode(int key, int val) { this.key = key; this.val = val; }
}
```

---

## 5. Stacks

### Theory

A **stack** is a **Last-In, First-Out (LIFO)** data structure. Think of a stack of plates — you add and remove from the top only.

**Operations (all O(1)):**
- **push(x)** — add element to the top.
- **pop()** — remove and return the top element.
- **peek()/top()** — view the top element without removing.
- **isEmpty()** — check if empty.

**Implementation Options:**
- **Array-based** (`ArrayDeque` in Java) — better cache performance, preferred.
- **Linked-list-based** — no capacity limits, more memory overhead.

**In Java:** Use `Deque<T> stack = new ArrayDeque<>()` — NOT the legacy `Stack` class (which is synchronized and extends `Vector`).

**When Stacks Are Used:**
- **Function call stack** — the JVM uses a stack to manage method calls and local variables.
- **Expression evaluation** — infix to postfix, evaluate postfix.
- **Bracket matching** — validate parentheses.
- **Undo/Redo** — most text editors use stacks.
- **DFS (iterative)** — explicit stack replaces recursion.
- **Monotonic Stack** — next greater/smaller element problems.
- **Backtracking** — implicit stack via recursion.

**Monotonic Stack:** A stack where elements are always in increasing (or decreasing) order. When a new element breaks the order, pop elements and process them. This pattern solves "next greater element" problems in O(n).

### Java Essentials

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);         // push to top
stack.pop();            // remove & return top — throws if empty
stack.peek();           // view top — throws if empty
stack.isEmpty();
stack.size();
```

### Valid Parentheses

```java
boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(') stack.push(')');
        else if (c == '[') stack.push(']');
        else if (c == '{') stack.push('}');
        else if (stack.isEmpty() || stack.pop() != c) return false;
    }
    return stack.isEmpty();
}
```

### Min Stack — O(1) getMin

**Idea:** Store pairs of (value, current minimum) on the stack. Each entry remembers what the minimum was at that point in time. When we pop, the minimum automatically reverts.

```java
class MinStack {
    Deque<int[]> stack = new ArrayDeque<>(); // [val, currentMin]

    void push(int val) {
        int min = stack.isEmpty() ? val : Math.min(val, stack.peek()[1]);
        stack.push(new int[]{val, min});
    }

    void pop() { stack.pop(); }
    int top() { return stack.peek()[0]; }
    int getMin() { return stack.peek()[1]; }
}
```

### Evaluate Reverse Polish Notation

**Theory:** In **Reverse Polish Notation (postfix)**, operators come after operands: `3 4 + 5 *` = `(3 + 4) * 5 = 35`. No parentheses needed. Process left to right: push numbers, when you see an operator, pop two operands, compute, push the result.

```java
int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String t : tokens) {
        switch (t) {
            case "+" -> stack.push(stack.pop() + stack.pop());
            case "-" -> { int b = stack.pop(), a = stack.pop(); stack.push(a - b); }
            case "*" -> stack.push(stack.pop() * stack.pop());
            case "/" -> { int b = stack.pop(), a = stack.pop(); stack.push(a / b); }
            default  -> stack.push(Integer.parseInt(t));
        }
    }
    return stack.pop();
}
```

### Next Greater Element (Monotonic Stack)

**Idea:** Traverse from left to right. Maintain a stack of indices whose next greater element hasn't been found yet. When the current element is larger than `stack.peek()`, it's the answer for that index. The stack stays in decreasing order. O(n) time — each element is pushed and popped at most once.

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>(); // stores indices
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && nums[stack.peek()] < nums[i])
            result[stack.pop()] = nums[i];
        stack.push(i);
    }
    return result;
}
```

### Largest Rectangle in Histogram

**Theory:** For each bar, we need to find how far left and right it can extend without encountering a shorter bar. A monotonic stack (increasing) efficiently tracks this. When a shorter bar is encountered, pop taller bars and calculate their areas using the current index as the right boundary and the new stack top as the left boundary. O(n) time.

```java
int largestRectangleArea(int[] heights) {
    int n = heights.length, maxArea = 0;
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i <= n; i++) {
        int h = (i == n) ? 0 : heights[i];
        while (!stack.isEmpty() && h < heights[stack.peek()]) {
            int height = heights[stack.pop()];
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    return maxArea;
}
```

---

## 6. Queues

### Theory

A **queue** is a **First-In, First-Out (FIFO)** data structure. Think of a line at a ticket counter — first person in line is served first.

**Operations (all O(1)):**
- **offer(x) / enqueue** — add to the back.
- **poll() / dequeue** — remove from the front.
- **peek()** — view the front element.
- **isEmpty()** — check if empty.

**Types of Queues:**
- **Simple Queue (FIFO)** — standard queue.
- **Deque (Double-Ended Queue)** — insert/remove from both ends. Can function as both stack and queue.
- **Priority Queue** — elements are dequeued by priority (see Heaps section).
- **Circular Queue** — the last position wraps around to the first. Efficient use of array space.

**Implementation Options:**
- `Queue<T> queue = new LinkedList<>()` — linked-list based.
- `Deque<T> deque = new ArrayDeque<>()` — circular array, preferred for performance.

**When Queues Are Used:**
- **BFS (Breadth-First Search)** — the fundamental use case. Explore nodes level by level.
- **Task scheduling** — FIFO order processing.
- **Sliding window maximum** — monotonic deque.
- **Stream processing** — buffer incoming data.

**BFS with Queue:**
BFS explores all neighbors at the current depth before moving to the next depth. It finds the **shortest path in unweighted graphs**. The queue ensures level-by-level exploration.

### Java Essentials

```java
// Standard Queue (FIFO)
Queue<Integer> queue = new LinkedList<>();
queue.offer(10);        // enqueue
queue.poll();           // dequeue (null if empty)
queue.peek();           // front element (null if empty)
queue.isEmpty();
queue.size();

// Deque (Double-ended queue)
Deque<Integer> deque = new ArrayDeque<>();
deque.offerFirst(1);    deque.offerLast(2);
deque.pollFirst();      deque.pollLast();
deque.peekFirst();      deque.peekLast();
```

### BFS with Queue

```java
void bfs(int[][] grid, int startRow, int startCol) {
    int m = grid.length, n = grid[0].length;
    boolean[][] visited = new boolean[m][n];
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{startRow, startCol});
    visited[startRow][startCol] = true;
    int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};

    while (!queue.isEmpty()) {
        int[] cell = queue.poll();
        for (int[] d : dirs) {
            int nr = cell[0] + d[0], nc = cell[1] + d[1];
            if (nr >= 0 && nr < m && nc >= 0 && nc < n
                    && !visited[nr][nc] && grid[nr][nc] == 1) {
                visited[nr][nc] = true;
                queue.offer(new int[]{nr, nc});
            }
        }
    }
}
```

### Sliding Window Maximum (Monotonic Deque)

**Theory:** Maintain a deque of indices in **decreasing order** of their values. The front of the deque is always the maximum in the current window. When the window slides:
1. Remove indices that have fallen out of the window from the front.
2. Remove indices from the back whose values are ≤ the new element (they can never be the max).
3. Add the new index to the back.

This ensures each element is added and removed at most once → O(n) total.

```java
int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < n; i++) {
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1)
            deque.pollFirst();
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i])
            deque.pollLast();
        deque.offerLast(i);
        if (i >= k - 1)
            result[i - k + 1] = nums[deque.peekFirst()];
    }
    return result;
}
```

### Implement Queue Using Stacks

**Theory:** Use two stacks — `in` for push, `out` for pop/peek. When `out` is empty, pour all elements from `in` into `out` (reversing the order). This gives **O(1) amortized** per operation because each element is moved at most twice.

```java
class MyQueue {
    Deque<Integer> in = new ArrayDeque<>(), out = new ArrayDeque<>();

    void push(int x) { in.push(x); }

    int pop() { shift(); return out.pop(); }

    int peek() { shift(); return out.peek(); }

    boolean empty() { return in.isEmpty() && out.isEmpty(); }

    private void shift() {
        if (out.isEmpty())
            while (!in.isEmpty()) out.push(in.pop());
    }
}
```

### Implement Stack Using Queue

**Theory:** Use a single queue. After each push, rotate the queue so the newly added element is at the front. Push: O(n). Pop/Peek: O(1).

```java
class MyStack {
    Queue<Integer> queue = new LinkedList<>();

    void push(int x) {
        queue.offer(x);
        for (int i = 0; i < queue.size() - 1; i++)
            queue.offer(queue.poll());
    }

    int pop() { return queue.poll(); }
    int top() { return queue.peek(); }
    boolean empty() { return queue.isEmpty(); }
}
```

---

## 7. Hashing

### Theory

**Hashing** maps keys to indices in a fixed-size table using a **hash function**, enabling O(1) average-case lookups, insertions, and deletions.

**Hash Function:** Takes a key and returns an integer (hash code). A good hash function:
- Is **deterministic** — same key always produces the same hash.
- Distributes keys **uniformly** across the table.
- Is **fast** to compute.

In Java, every object has `.hashCode()`. HashMap uses it to find the bucket, then `.equals()` to resolve the exact match.

**Collision Handling:**
- **Separate Chaining** — each bucket holds a linked list (or tree for Java 8+). Multiple keys can live in the same bucket.
- **Open Addressing** — on collision, probe for the next empty slot (linear probing, quadratic probing, double hashing).

**Java HashMap Internals (Java 8+):**
- Array of buckets (default initial capacity: 16).
- **Load factor** = number of entries / number of buckets (default 0.75).
- When load factor is exceeded, the table **rehashes** (doubles in size and re-distributes all entries).
- Buckets with > 8 entries convert from linked list to **red-black tree** (O(log n) worst case instead of O(n)).

**Time Complexity:**

| Operation | Average | Worst (all collisions) |
|---|---|---|
| put() | O(1) | O(n) |
| get() | O(1) | O(n) |
| remove() | O(1) | O(n) |
| containsKey() | O(1) | O(n) |

**Common Hash-Based Patterns:**
- **Frequency counting** — count occurrences of elements.
- **Two Sum** — complement lookup.
- **Prefix Sum + HashMap** — count subarrays with a given sum.
- **Group by key** — group anagrams, group by property.
- **Caching / Memoization** — store computed results.

**HashMap vs TreeMap vs LinkedHashMap:**

| Feature | HashMap | TreeMap | LinkedHashMap |
|---|---|---|---|
| Order | None | Sorted by key | Insertion order |
| Get/Put | O(1) avg | O(log n) | O(1) avg |
| Implementation | Hash table | Red-Black tree | Hash table + doubly linked list |
| Null keys | 1 allowed | Not allowed | 1 allowed |

### Java Collections

```java
// HashMap — O(1) avg get/put
Map<String, Integer> map = new HashMap<>();
map.put("key", 1);
map.get("key");                    // 1
map.getOrDefault("missing", 0);   // 0
map.containsKey("key");           // true
map.containsValue(1);             // O(n)
map.remove("key");
map.size();
map.keySet();                     // Set<String>
map.values();                     // Collection<Integer>
map.entrySet();                   // Set<Map.Entry<String, Integer>>

// Iterate
for (Map.Entry<String, Integer> e : map.entrySet())
    System.out.println(e.getKey() + " = " + e.getValue());

map.forEach((k, v) -> System.out.println(k + " = " + v));

// Merge / compute
map.merge("key", 1, Integer::sum);          // increment
map.computeIfAbsent("key", k -> new ArrayList<>()).add(10);

// LinkedHashMap — insertion order preserved
Map<String, Integer> lhm = new LinkedHashMap<>();

// TreeMap — sorted by keys, O(log n) operations
TreeMap<Integer, String> tm = new TreeMap<>();
tm.firstKey();   tm.lastKey();
tm.floorKey(5);  tm.ceilingKey(5);
tm.lowerKey(5);  tm.higherKey(5);

// HashSet — O(1) avg add/contains/remove
Set<Integer> set = new HashSet<>();
set.add(1);
set.contains(1);
set.remove(1);

// TreeSet — sorted, O(log n)
TreeSet<Integer> ts = new TreeSet<>();
ts.floor(5);  ts.ceiling(5);
ts.lower(5);  ts.higher(5);
ts.first();   ts.last();
```

### Frequency Map Pattern

```java
Map<Integer, Integer> freq = new HashMap<>();
for (int num : nums)
    freq.merge(num, 1, Integer::sum);
```

### Group Anagrams

**Idea:** Anagrams have the same sorted character sequence. Sort each string to create a canonical key, then group by that key using a HashMap.

```java
List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    for (String s : strs) {
        char[] ca = s.toCharArray();
        Arrays.sort(ca);
        String key = new String(ca);
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }
    return new ArrayList<>(map.values());
}
```

### Two Sum

**Idea:** For each element, check if `target - element` already exists in the map. If yes, we found the pair. If no, store the current element. One-pass, O(n).

```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement))
            return new int[]{map.get(complement), i};
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

### Subarray Sum Equals K (Prefix Sum + HashMap)

**Theory:** If `prefixSum[j] - prefixSum[i] = k`, then the subarray `[i+1, j]` sums to `k`. We store the frequency of each prefix sum in a HashMap. For each new prefix sum, we check how many previous prefix sums equal `currentSum - k`. O(n) time, O(n) space.

```java
int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0, 1);
    int sum = 0, count = 0;
    for (int num : nums) {
        sum += num;
        count += prefixCount.getOrDefault(sum - k, 0);
        prefixCount.merge(sum, 1, Integer::sum);
    }
    return count;
}
```

### LRU Cache

**Theory:** A **Least Recently Used** cache evicts the least recently accessed item when full. It requires O(1) get and put. Implementation: **HashMap** (for O(1) lookup) + **Doubly Linked List** (for O(1) removal and insertion at ends). Java's `LinkedHashMap` with `accessOrder=true` does this automatically.

```java
class LRUCache extends LinkedHashMap<Integer, Integer> {
    private final int capacity;

    LRUCache(int capacity) {
        super(capacity, 0.75f, true); // accessOrder = true
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```

### Design HashMap from Scratch

**Theory:** Use an array of buckets with **separate chaining** (linked lists). Hash function maps keys to bucket indices. On collision, traverse the chain. Resize when load factor exceeds threshold.

```java
class MyHashMap {
    private static final int SIZE = 1024;
    private ListNode[] buckets = new ListNode[SIZE];

    private int hash(int key) { return key % SIZE; }

    public void put(int key, int value) {
        int idx = hash(key);
        if (buckets[idx] == null) { buckets[idx] = new ListNode(-1, -1); }
        ListNode prev = find(buckets[idx], key);
        if (prev.next == null) prev.next = new ListNode(key, value);
        else prev.next.val = value;
    }

    public int get(int key) {
        int idx = hash(key);
        if (buckets[idx] == null) return -1;
        ListNode prev = find(buckets[idx], key);
        return prev.next == null ? -1 : prev.next.val;
    }

    public void remove(int key) {
        int idx = hash(key);
        if (buckets[idx] == null) return;
        ListNode prev = find(buckets[idx], key);
        if (prev.next != null) prev.next = prev.next.next;
    }

    private ListNode find(ListNode head, int key) {
        ListNode curr = head;
        while (curr.next != null && curr.next.key != key) curr = curr.next;
        return curr;
    }

    private static class ListNode {
        int key, val;
        ListNode next;
        ListNode(int key, int val) { this.key = key; this.val = val; }
    }
}
```

---

## 8. Trees (Binary Tree & BST)

### Theory

A **tree** is a hierarchical data structure consisting of nodes connected by edges. A **binary tree** is a tree where each node has at most 2 children (left and right).

**Terminology:**
- **Root** — the topmost node (no parent).
- **Leaf** — a node with no children.
- **Height** — longest path from root to a leaf (root has height 0 or 1 depending on convention; we use edges here).
- **Depth** — distance from root to the node.
- **Level** — depth + 1.
- **Subtree** — a node and all its descendants.
- **Complete Binary Tree** — every level is fully filled except possibly the last, which is filled left to right.
- **Full Binary Tree** — every node has 0 or 2 children.
- **Perfect Binary Tree** — all internal nodes have 2 children and all leaves are at the same level. Has 2ʰ⁺¹ - 1 nodes.
- **Balanced Binary Tree** — height difference between left and right subtrees of every node is at most 1. Guarantees O(log n) height.

**Binary Search Tree (BST):**
A binary tree where for every node:
- All values in the **left** subtree are **less** than the node's value.
- All values in the **right** subtree are **greater** than the node's value.

This property enables O(h) search, insert, and delete where h is the tree height.

| Operation | BST (avg) | BST (worst - skewed) | Balanced BST (AVL/RBT) |
|---|---|---|---|
| Search | O(log n) | O(n) | O(log n) |
| Insert | O(log n) | O(n) | O(log n) |
| Delete | O(log n) | O(n) | O(log n) |
| Inorder | O(n) | O(n) | O(n) |

**Traversals:**
- **Inorder (Left → Root → Right)** — produces sorted order for BST. Used for BST validation, kth smallest.
- **Preorder (Root → Left → Right)** — used to serialize/copy a tree. Root is first.
- **Postorder (Left → Right → Root)** — used to delete a tree. Root is last. Also used for computing sizes/heights bottom-up.
- **Level Order (BFS)** — visit nodes level by level using a queue.

**Common Tree Patterns:**
- **Recursive DFS** — most tree problems decompose into: solve left, solve right, combine. The recursion implicitly uses the call stack.
- **Height computation** — `1 + max(left height, right height)`.
- **Diameter** — longest path between any two nodes = max(left height + right height) at each node.
- **LCA** — lowest common ancestor, found by checking which subtrees contain the targets.
- **Serialize/Deserialize** — convert tree to string and back using preorder with null markers.

### Node Definition

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

### Traversals

```java
// Inorder (Left → Root → Right) — gives sorted order for BST
void inorder(TreeNode root, List<Integer> res) {
    if (root == null) return;
    inorder(root.left, res);
    res.add(root.val);
    inorder(root.right, res);
}

// Preorder (Root → Left → Right)
void preorder(TreeNode root, List<Integer> res) {
    if (root == null) return;
    res.add(root.val);
    preorder(root.left, res);
    preorder(root.right, res);
}

// Postorder (Left → Right → Root)
void postorder(TreeNode root, List<Integer> res) {
    if (root == null) return;
    postorder(root.left, res);
    postorder(root.right, res);
    res.add(root.val);
}
```

### Iterative Inorder

**Idea:** Use an explicit stack. Push all left children, then pop, process, and move to the right child. This mimics the recursive call stack.

```java
List<Integer> inorderIterative(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) { stack.push(curr); curr = curr.left; }
        curr = stack.pop();
        res.add(curr.val);
        curr = curr.right;
    }
    return res;
}
```

### Level Order Traversal (BFS)

**Idea:** Use a queue. Process all nodes at the current level (determined by `queue.size()` at the start of each iteration), then enqueue their children.

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
```

### Height / Max Depth

```java
int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### Diameter of Binary Tree

**Idea:** The diameter passes through some node where `leftHeight + rightHeight` is maximized. Compute height recursively and update the global diameter at each node.

```java
int diameter;

int diameterOfBinaryTree(TreeNode root) {
    diameter = 0;
    height(root);
    return diameter;
}

int height(TreeNode node) {
    if (node == null) return 0;
    int left = height(node.left);
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
}
```

### Check Balanced

**Idea:** A tree is balanced if for every node, the height difference between left and right subtrees is ≤ 1. We compute height bottom-up and return -1 immediately if any subtree is unbalanced (early termination).

```java
boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}

int checkHeight(TreeNode node) {
    if (node == null) return 0;
    int left = checkHeight(node.left);
    if (left == -1) return -1;
    int right = checkHeight(node.right);
    if (right == -1) return -1;
    if (Math.abs(left - right) > 1) return -1;
    return 1 + Math.max(left, right);
}
```

### Lowest Common Ancestor (LCA)

**Theory:** The LCA of two nodes `p` and `q` is the deepest node that has both `p` and `q` as descendants. Recursively search left and right subtrees. If both return non-null, the current node is the LCA. If only one returns non-null, propagate it up.

```java
TreeNode lca(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lca(root.left, p, q);
    TreeNode right = lca(root.right, p, q);
    if (left != null && right != null) return root;
    return left != null ? left : right;
}
```

### BST Operations

**Search:** Go left if target < node, right if target > node. O(h).

**Insert:** Navigate to the correct null position and attach the new node. O(h).

**Delete:** Three cases:
1. **Leaf** — simply remove.
2. **One child** — replace with the child.
3. **Two children** — replace with the **inorder successor** (smallest in right subtree), then delete that successor.

```java
// Search — O(h)
TreeNode search(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    return val < root.val ? search(root.left, val) : search(root.right, val);
}

// Insert — O(h)
TreeNode insert(TreeNode root, int val) {
    if (root == null) return new TreeNode(val);
    if (val < root.val) root.left = insert(root.left, val);
    else root.right = insert(root.right, val);
    return root;
}

// Delete — O(h)
TreeNode delete(TreeNode root, int val) {
    if (root == null) return null;
    if (val < root.val) root.left = delete(root.left, val);
    else if (val > root.val) root.right = delete(root.right, val);
    else {
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        TreeNode min = root.right;
        while (min.left != null) min = min.left;
        root.val = min.val;
        root.right = delete(root.right, min.val);
    }
    return root;
}
```

### Validate BST

**Idea:** Each node must fall within a valid range (min, max). The left child's max is the parent's value; the right child's min is the parent's value. Use `long` to handle edge cases with `Integer.MIN_VALUE` / `Integer.MAX_VALUE`.

```java
boolean isValidBST(TreeNode root) {
    return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean validate(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return validate(node.left, min, node.val) && validate(node.right, node.val, max);
}
```

### Kth Smallest in BST

**Idea:** Inorder traversal of a BST visits nodes in sorted order. Use iterative inorder and decrement k — when k reaches 0, we have our answer. O(H + k) time.

```java
int kthSmallest(TreeNode root, int k) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) { stack.push(curr); curr = curr.left; }
        curr = stack.pop();
        if (--k == 0) return curr.val;
        curr = curr.right;
    }
    return -1;
}
```

### Serialize / Deserialize Binary Tree

**Idea:** Use preorder traversal. Write node values, and "null" for null children. Deserialization reads tokens from a queue and reconstructs the tree in the same preorder sequence.

```java
String serialize(TreeNode root) {
    if (root == null) return "null";
    return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

TreeNode deserialize(String data) {
    Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
    return buildTree(queue);
}

TreeNode buildTree(Queue<String> queue) {
    String val = queue.poll();
    if ("null".equals(val)) return null;
    TreeNode node = new TreeNode(Integer.parseInt(val));
    node.left = buildTree(queue);
    node.right = buildTree(queue);
    return node;
}
```

---

## 9. Heaps / Priority Queues

### Theory

A **heap** is a specialized **complete binary tree** that satisfies the **heap property**:
- **Min-Heap:** Every parent ≤ its children. Root is the minimum.
- **Max-Heap:** Every parent ≥ its children. Root is the maximum.

**Why complete binary tree?** It allows efficient array representation without pointers:
- Parent of index `i`: `(i - 1) / 2`
- Left child of index `i`: `2 * i + 1`
- Right child of index `i`: `2 * i + 2`

**Operations:**

| Operation | Time |
|---|---|
| peek (get min/max) | O(1) |
| offer (insert) | O(log n) — bubble up |
| poll (extract min/max) | O(log n) — bubble down |
| heapify (build heap from array) | O(n) — bottom-up |
| remove arbitrary | O(n) — need to find it first |

**Heapify in O(n):** Start from the last non-leaf node and sift down each node. The math works out to O(n) because most nodes are near the bottom and only sift down a few levels.

**Priority Queue vs Heap:** A priority queue is an abstract data type; a heap is its most common implementation. Java's `PriorityQueue` is a min-heap.

**When to Use:**
- **Kth largest/smallest** — maintain a heap of size k.
- **Top K elements** — use a min-heap of size k for top-k largest.
- **Merge K sorted structures** — push one element from each, pop smallest, push next.
- **Median maintenance** — two heaps (max-heap for lower half, min-heap for upper half).
- **Dijkstra's algorithm** — min-heap for the closest unvisited node.
- **Huffman coding** — always merge the two lowest-frequency nodes.

### Java Essentials

```java
// Min-Heap (default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(5);        // O(log n)
minHeap.poll();          // removes & returns smallest — O(log n)
minHeap.peek();          // view smallest — O(1)
minHeap.size();
minHeap.isEmpty();

// Max-Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// Custom comparator
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
```

### Kth Largest Element

**Idea:** Maintain a min-heap of size k. After processing all elements, the heap root is the kth largest. O(n log k) time, O(k) space. Better than sorting when k << n.

```java
int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) minHeap.poll();
    }
    return minHeap.peek();
}
```

### Top K Frequent Elements

```java
int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int n : nums) freq.merge(n, 1, Integer::sum);

    PriorityQueue<Integer> minHeap = new PriorityQueue<>(
        (a, b) -> freq.get(a) - freq.get(b));

    for (int key : freq.keySet()) {
        minHeap.offer(key);
        if (minHeap.size() > k) minHeap.poll();
    }

    int[] result = new int[k];
    for (int i = k - 1; i >= 0; i--) result[i] = minHeap.poll();
    return result;
}
```

### Merge K Sorted Lists

**Idea:** Put the head of each list into a min-heap. Repeatedly poll the smallest, append to result, and push that node's next (if exists). O(N log k) where N = total elements, k = number of lists.

```java
ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
    for (ListNode l : lists)
        if (l != null) pq.offer(l);

    ListNode dummy = new ListNode(0), curr = dummy;
    while (!pq.isEmpty()) {
        ListNode node = pq.poll();
        curr.next = node;
        curr = curr.next;
        if (node.next != null) pq.offer(node.next);
    }
    return dummy.next;
}
```

### Median Finder (Two Heaps)

**Theory:** Split numbers into two halves:
- **lo** (max-heap) — stores the smaller half. Top = largest of the smaller half.
- **hi** (min-heap) — stores the larger half. Top = smallest of the larger half.

Maintain the invariant: `lo.size() >= hi.size()` and `lo.size() - hi.size() <= 1`.

The median is:
- Odd total: `lo.peek()`
- Even total: `(lo.peek() + hi.peek()) / 2.0`

```java
class MedianFinder {
    PriorityQueue<Integer> lo = new PriorityQueue<>(Collections.reverseOrder()); // max-heap
    PriorityQueue<Integer> hi = new PriorityQueue<>(); // min-heap

    void addNum(int num) {
        lo.offer(num);
        hi.offer(lo.poll());
        if (hi.size() > lo.size()) lo.offer(hi.poll());
    }

    double findMedian() {
        return lo.size() > hi.size() ? lo.peek() : (lo.peek() + hi.peek()) / 2.0;
    }
}
```

---

## 10. Graphs

### Theory

A **graph** G = (V, E) consists of **vertices (nodes)** V and **edges** E connecting pairs of vertices.

**Types:**
- **Directed vs Undirected** — edges have direction or not.
- **Weighted vs Unweighted** — edges have costs or not.
- **Cyclic vs Acyclic** — contains cycles or not. A **DAG** is a Directed Acyclic Graph.
- **Connected vs Disconnected** — every node is reachable from every other node, or not.
- **Dense vs Sparse** — dense: E ≈ V², sparse: E ≈ V. Choose representation accordingly.

**Representations:**

| Representation | Space | Add Edge | Check Edge | Iterate Neighbors |
|---|---|---|---|---|
| Adjacency List | O(V + E) | O(1) | O(degree) | O(degree) |
| Adjacency Matrix | O(V²) | O(1) | O(1) | O(V) |
| Edge List | O(E) | O(1) | O(E) | O(E) |

Use **adjacency list** for sparse graphs (most interview problems). Use **adjacency matrix** for dense graphs or when you need O(1) edge checks.

**DFS vs BFS:**

| Property | DFS | BFS |
|---|---|---|
| Data structure | Stack (or recursion) | Queue |
| Order | Explores as deep as possible first | Explores level by level |
| Time | O(V + E) | O(V + E) |
| Space | O(V) | O(V) |
| Shortest path | No (in unweighted) | Yes (in unweighted) |
| Use cases | Cycle detection, topological sort, connected components, backtracking | Shortest path, level-order, minimum steps |

**Shortest Path Algorithms:**

| Algorithm | Graph Type | Time | Space | Negative Weights |
|---|---|---|---|---|
| BFS | Unweighted | O(V + E) | O(V) | N/A |
| Dijkstra | Non-negative weights | O((V+E) log V) | O(V) | No |
| Bellman-Ford | Any | O(V·E) | O(V) | Yes (detects neg cycles) |
| Floyd-Warshall | All-pairs | O(V³) | O(V²) | Yes (detects neg cycles) |

**Minimum Spanning Tree (MST):**
A subset of edges connecting all vertices with minimum total weight (no cycles).
- **Prim's** — grow the MST from a starting node, always adding the cheapest edge to an unvisited node. Uses a min-heap. Good for dense graphs.
- **Kruskal's** — sort all edges by weight, add them greedily if they don't create a cycle (use Union-Find). Good for sparse graphs.

### Representations

```java
// Adjacency List (most common)
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
adj.get(u).add(v); // directed edge u → v
adj.get(v).add(u); // add this for undirected

// Adjacency List with weights
List<List<int[]>> adj = new ArrayList<>();
adj.get(u).add(new int[]{v, weight});

// Edge List
int[][] edges = {{0,1}, {1,2}, {2,0}};

// Adjacency Matrix
boolean[][] matrix = new boolean[n][n];
matrix[u][v] = true;
```

### DFS (Recursive)

```java
void dfs(List<List<Integer>> adj, int node, boolean[] visited) {
    visited[node] = true;
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor])
            dfs(adj, neighbor, visited);
    }
}
```

### DFS (Iterative)

```java
void dfsIterative(List<List<Integer>> adj, int start) {
    boolean[] visited = new boolean[adj.size()];
    Deque<Integer> stack = new ArrayDeque<>();
    stack.push(start);
    while (!stack.isEmpty()) {
        int node = stack.pop();
        if (visited[node]) continue;
        visited[node] = true;
        for (int neighbor : adj.get(node))
            if (!visited[neighbor]) stack.push(neighbor);
    }
}
```

### BFS

```java
void bfs(List<List<Integer>> adj, int start) {
    boolean[] visited = new boolean[adj.size()];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited[start] = true;
    int level = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int node = queue.poll();
            for (int neighbor : adj.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
        level++;
    }
}
```

### Number of Connected Components

```java
int countComponents(int n, int[][] edges) {
    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
    for (int[] e : edges) { adj.get(e[0]).add(e[1]); adj.get(e[1]).add(e[0]); }

    boolean[] visited = new boolean[n];
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(adj, i, visited);
            count++;
        }
    }
    return count;
}
```

### Number of Islands (Grid DFS)

**Idea:** Treat each cell as a node, connected to 4 neighbors. For each unvisited '1', start a DFS/BFS and mark all connected '1's as visited (sink them to '0'). Each DFS call = one island.

```java
int numIslands(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++)
        for (int j = 0; j < grid[0].length; j++)
            if (grid[i][j] == '1') { sinkIsland(grid, i, j); count++; }
    return count;
}

void sinkIsland(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0')
        return;
    grid[i][j] = '0';
    sinkIsland(grid, i + 1, j);
    sinkIsland(grid, i - 1, j);
    sinkIsland(grid, i, j + 1);
    sinkIsland(grid, i, j - 1);
}
```

### Detect Cycle — Undirected (DFS)

**Idea:** If we visit a neighbor that's already visited AND it's not our parent, we've found a cycle.

```java
boolean hasCycleUndirected(List<List<Integer>> adj, int n) {
    boolean[] visited = new boolean[n];
    for (int i = 0; i < n; i++)
        if (!visited[i] && dfsCycle(adj, i, -1, visited)) return true;
    return false;
}

boolean dfsCycle(List<List<Integer>> adj, int node, int parent, boolean[] visited) {
    visited[node] = true;
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) {
            if (dfsCycle(adj, neighbor, node, visited)) return true;
        } else if (neighbor != parent) return true;
    }
    return false;
}
```

### Detect Cycle — Directed (DFS with coloring)

**Theory:** Use three colors:
- **White (0)** — unvisited
- **Gray (1)** — in the current DFS path (being explored)
- **Black (2)** — fully explored

A cycle exists if we encounter a **gray node** (back edge = cycle).

```java
boolean hasCycleDirected(List<List<Integer>> adj, int n) {
    int[] color = new int[n]; // 0=white, 1=gray, 2=black
    for (int i = 0; i < n; i++)
        if (color[i] == 0 && dfsCycleDirected(adj, i, color)) return true;
    return false;
}

boolean dfsCycleDirected(List<List<Integer>> adj, int node, int[] color) {
    color[node] = 1;
    for (int neighbor : adj.get(node)) {
        if (color[neighbor] == 1) return true; // back edge = cycle
        if (color[neighbor] == 0 && dfsCycleDirected(adj, neighbor, color)) return true;
    }
    color[node] = 2;
    return false;
}
```

### Dijkstra's Shortest Path — O((V + E) log V)

**Theory:** Greedy algorithm for single-source shortest paths in graphs with **non-negative** weights.

1. Initialize all distances to ∞ except source (0).
2. Use a min-heap. Always process the closest unfinalized node.
3. For each neighbor, relax the edge: if `dist[u] + weight < dist[v]`, update `dist[v]`.

**Why it works:** With non-negative weights, once a node is finalized (popped from heap), we've found its shortest path. No future path can improve it.

**Why not negative weights?** A negative edge could make a finalized node's distance wrong.

```java
int[] dijkstra(List<List<int[]>> adj, int src, int n) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[src] = 0;
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, src});

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int d = curr[0], u = curr[1];
        if (d > dist[u]) continue;
        for (int[] edge : adj.get(u)) {
            int v = edge[0], w = edge[1];
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.offer(new int[]{dist[v], v});
            }
        }
    }
    return dist;
}
```

### Bellman-Ford — O(V·E)

**Theory:** Relaxes all edges V-1 times. Works with **negative weights**. After V-1 iterations, if any edge can still be relaxed, a **negative cycle** exists.

**Why V-1 iterations?** The shortest path between any two vertices has at most V-1 edges. Each iteration ensures paths of length i are finalized.

```java
int[] bellmanFord(int[][] edges, int n, int src) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[src] = 0;
    for (int i = 0; i < n - 1; i++)
        for (int[] e : edges)
            if (dist[e[0]] != Integer.MAX_VALUE && dist[e[0]] + e[2] < dist[e[1]])
                dist[e[1]] = dist[e[0]] + e[2];
    // Negative cycle detection
    for (int[] e : edges)
        if (dist[e[0]] != Integer.MAX_VALUE && dist[e[0]] + e[2] < dist[e[1]])
            throw new RuntimeException("Negative cycle detected");
    return dist;
}
```

### Floyd-Warshall — O(V³)

**Theory:** Computes shortest paths between **all pairs** of vertices. The key idea: for each intermediate vertex k, check if going through k improves the path from i to j: `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`.

```java
int[][] floydWarshall(int[][] dist, int n) {
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (dist[i][k] != Integer.MAX_VALUE && dist[k][j] != Integer.MAX_VALUE)
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
    return dist;
}
```

### Prim's MST — O((V + E) log V)

**Idea:** Start from any node. Greedily pick the cheapest edge connecting the MST to a non-MST node. Use a min-heap.

```java
int primMST(List<List<int[]>> adj, int n) {
    boolean[] visited = new boolean[n];
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{0, 0}); // {node, weight}
    int totalWeight = 0, edges = 0;

    while (!pq.isEmpty() && edges < n) {
        int[] curr = pq.poll();
        int u = curr[0], w = curr[1];
        if (visited[u]) continue;
        visited[u] = true;
        totalWeight += w;
        edges++;
        for (int[] edge : adj.get(u))
            if (!visited[edge[0]]) pq.offer(new int[]{edge[0], edge[1]});
    }
    return totalWeight;
}
```

### Kruskal's MST — O(E log E)

**Idea:** Sort all edges by weight. Greedily add the cheapest edge that doesn't create a cycle (use Union-Find to check).

```java
int kruskalMST(int[][] edges, int n) {
    Arrays.sort(edges, (a, b) -> a[2] - b[2]); // sort by weight
    UnionFind uf = new UnionFind(n);
    int totalWeight = 0, edgeCount = 0;
    for (int[] e : edges) {
        if (uf.union(e[0], e[1])) {
            totalWeight += e[2];
            if (++edgeCount == n - 1) break;
        }
    }
    return totalWeight;
}
```

### Bipartite Check (BFS 2-coloring)

**Theory:** A graph is **bipartite** if it can be colored with 2 colors such that no two adjacent nodes share a color. Equivalently: it contains no odd-length cycles. Use BFS and assign alternating colors.

```java
boolean isBipartite(List<List<Integer>> adj, int n) {
    int[] color = new int[n];
    Arrays.fill(color, -1);
    for (int i = 0; i < n; i++) {
        if (color[i] != -1) continue;
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(i);
        color[i] = 0;
        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int neighbor : adj.get(node)) {
                if (color[neighbor] == -1) {
                    color[neighbor] = 1 - color[node];
                    queue.offer(neighbor);
                } else if (color[neighbor] == color[node]) return false;
            }
        }
    }
    return true;
}
```

### Multi-Source BFS (Rotting Oranges)

**Theory:** Instead of starting BFS from a single source, enqueue **all sources at once** before starting. This finds the shortest distance from any source simultaneously. Common in "spreading" problems (rotting, fire, infection).

```java
int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> queue = new LinkedList<>();
    int fresh = 0;

    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) queue.offer(new int[]{i, j});
            else if (grid[i][j] == 1) fresh++;
        }

    int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};
    int minutes = 0;
    while (!queue.isEmpty() && fresh > 0) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] cell = queue.poll();
            for (int[] d : dirs) {
                int nr = cell[0] + d[0], nc = cell[1] + d[1];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    fresh--;
                    queue.offer(new int[]{nr, nc});
                }
            }
        }
        minutes++;
    }
    return fresh == 0 ? minutes : -1;
}
```

### 0-1 BFS

**Theory:** For graphs where edge weights are only 0 or 1, use a **deque** instead of a priority queue. Add 0-weight edges to the **front**, 1-weight edges to the **back**. This gives Dijkstra-like results in O(V + E) instead of O((V+E) log V).

```java
int[] bfs01(List<List<int[]>> adj, int src, int n) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[src] = 0;
    Deque<Integer> deque = new ArrayDeque<>();
    deque.offerFirst(src);

    while (!deque.isEmpty()) {
        int u = deque.pollFirst();
        for (int[] edge : adj.get(u)) {
            int v = edge[0], w = edge[1]; // w is 0 or 1
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                if (w == 0) deque.offerFirst(v);
                else deque.offerLast(v);
            }
        }
    }
    return dist;
}
```

### Clone Graph

**Theory:** Deep copy a graph using DFS + HashMap. The map tracks `original node → cloned node` to avoid cloning the same node twice and to handle cycles.

```java
class Node {
    int val;
    List<Node> neighbors;
    Node(int val) { this.val = val; neighbors = new ArrayList<>(); }
}

Map<Node, Node> visited = new HashMap<>();

Node cloneGraph(Node node) {
    if (node == null) return null;
    if (visited.containsKey(node)) return visited.get(node);
    Node clone = new Node(node.val);
    visited.put(node, clone);
    for (Node neighbor : node.neighbors)
        clone.neighbors.add(cloneGraph(neighbor));
    return clone;
}
```

### Shortest Path in Binary Matrix (8-directional BFS)

```java
int shortestPathBinaryMatrix(int[][] grid) {
    int n = grid.length;
    if (grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;
    int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{0, 0});
    grid[0][0] = 1;
    int dist = 1;

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] cell = queue.poll();
            if (cell[0] == n - 1 && cell[1] == n - 1) return dist;
            for (int[] d : dirs) {
                int nr = cell[0] + d[0], nc = cell[1] + d[1];
                if (nr >= 0 && nr < n && nc >= 0 && nc < n && grid[nr][nc] == 0) {
                    grid[nr][nc] = 1;
                    queue.offer(new int[]{nr, nc});
                }
            }
        }
        dist++;
    }
    return -1;
}
```

### Course Schedule II (Return Order)

**Idea:** Same as Course Schedule I (Kahn's algorithm), but return the actual topological ordering instead of just a boolean.

```java
int[] findOrder(int numCourses, int[][] prerequisites) {
    List<List<Integer>> adj = new ArrayList<>();
    int[] inDegree = new int[numCourses];
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) { adj.get(p[1]).add(p[0]); inDegree[p[0]]++; }

    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++)
        if (inDegree[i] == 0) queue.offer(i);

    int[] order = new int[numCourses];
    int idx = 0;
    while (!queue.isEmpty()) {
        int u = queue.poll();
        order[idx++] = u;
        for (int v : adj.get(u))
            if (--inDegree[v] == 0) queue.offer(v);
    }
    return idx == numCourses ? order : new int[]{};
}
```

---

## 11. Tries

### Theory

A **Trie** (prefix tree / digital tree) is a tree-like data structure for storing strings where each node represents a single character. Paths from root to nodes spell out prefixes.

**Properties:**
- The root is empty.
- Each node has up to 26 children (for lowercase English letters).
- A boolean flag `isEnd` marks the end of a complete word.

**Complexity (for a word of length L):**

| Operation | Time | Space |
|---|---|---|
| Insert | O(L) | O(L) new nodes |
| Search | O(L) | O(1) |
| StartsWith | O(L) | O(1) |
| Delete | O(L) | O(1) (just unmark) |

**Trie vs HashMap of Strings:**
- Trie is better when you need **prefix queries** (autocomplete, spell check).
- HashMap is better for simple key-value lookups.
- Trie enables "find all words with prefix X" in O(prefix length + number of results).

**When to Use:**
- **Autocomplete / type-ahead** — find all words with a given prefix.
- **Spell checker** — check if a word exists or suggest corrections.
- **Word search in grid** — combine with DFS/backtracking (Word Search II).
- **IP routing** — longest prefix match.
- **Word games** — Boggle, Scrabble solvers.

### Implementation

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEnd;
}

class Trie {
    TrieNode root = new TrieNode();

    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null)
                node.children[idx] = new TrieNode();
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    boolean search(String word) {
        TrieNode node = findNode(word);
        return node != null && node.isEnd;
    }

    boolean startsWith(String prefix) {
        return findNode(prefix) != null;
    }

    private TrieNode findNode(String s) {
        TrieNode node = root;
        for (char c : s.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return null;
            node = node.children[idx];
        }
        return node;
    }
}
```

### Word Search II (Trie + Backtracking)

**Idea:** Insert all target words into a Trie. Then DFS from every cell in the grid, following the Trie. This prunes the search early — if a prefix doesn't exist in the Trie, stop. Much more efficient than searching for each word individually.

```java
List<String> findWords(char[][] board, String[] words) {
    Trie trie = new Trie();
    for (String w : words) trie.insert(w);

    Set<String> result = new HashSet<>();
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            dfsBoard(board, i, j, trie.root, new StringBuilder(), result);
    return new ArrayList<>(result);
}

void dfsBoard(char[][] board, int i, int j, TrieNode node, StringBuilder sb, Set<String> result) {
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] == '#')
        return;
    char c = board[i][j];
    if (node.children[c - 'a'] == null) return;
    node = node.children[c - 'a'];
    sb.append(c);
    if (node.isEnd) result.add(sb.toString());

    board[i][j] = '#';
    dfsBoard(board, i + 1, j, node, sb, result);
    dfsBoard(board, i - 1, j, node, sb, result);
    dfsBoard(board, i, j + 1, node, sb, result);
    dfsBoard(board, i, j - 1, node, sb, result);
    board[i][j] = c;
    sb.deleteCharAt(sb.length() - 1);
}
```

---

## 12. Sorting Algorithms

### Theory

**Sorting** arranges elements in a specific order (usually ascending). It's a fundamental operation that enables binary search, two pointers, greedy approaches, and many other techniques.

**Stability:** A sort is **stable** if it preserves the relative order of equal elements. Stable sorts: Merge Sort, Insertion Sort, Counting Sort. Unstable: Quick Sort, Heap Sort, Selection Sort.

**In-place:** A sort is **in-place** if it uses O(1) extra space (excluding the input). In-place: Quick Sort, Heap Sort, Insertion Sort. Not in-place: Merge Sort (O(n) extra).

**Comparison-based lower bound:** Any comparison-based sort must make at least O(n log n) comparisons in the worst case. This is proven via the decision tree model. Merge Sort and Heap Sort achieve this bound.

**Non-comparison sorts** (Counting Sort, Radix Sort) can break the O(n log n) barrier by exploiting the structure of the data (e.g., integers in a bounded range).

**Java's Built-in Sorts:**
- `Arrays.sort(int[])` — Dual-Pivot Quicksort. O(n log n) average, O(n²) worst. NOT stable.
- `Arrays.sort(Object[])` — TimSort (hybrid merge sort + insertion sort). O(n log n) guaranteed. Stable.
- `Collections.sort()` — delegates to `Arrays.sort(Object[])` → TimSort. Stable.

**Choosing a Sort:**
- Small n (< ~50): Insertion Sort (low overhead, cache-friendly).
- General purpose: Merge Sort (stable, guaranteed O(n log n)) or Quick Sort (fast in practice).
- Nearly sorted data: Insertion Sort (O(n) best case) or TimSort.
- Integers in bounded range: Counting Sort or Radix Sort.

### Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n + k) | Yes |

### Merge Sort

**Theory:** Divide the array in half, recursively sort each half, then merge the two sorted halves. The merge step uses a temporary array and runs in O(n). Total: O(n log n) time, O(n) space.

**Why O(n log n)?** There are log n levels of recursion, and each level does O(n) work (merging).

```java
void mergeSort(int[] arr, int l, int r) {
    if (l >= r) return;
    int mid = l + (r - l) / 2;
    mergeSort(arr, l, mid);
    mergeSort(arr, mid + 1, r);
    merge(arr, l, mid, r);
}

void merge(int[] arr, int l, int mid, int r) {
    int[] temp = new int[r - l + 1];
    int i = l, j = mid + 1, k = 0;
    while (i <= mid && j <= r)
        temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= r) temp[k++] = arr[j++];
    System.arraycopy(temp, 0, arr, l, temp.length);
}
```

### Quick Sort

**Theory:** Pick a **pivot**, partition the array so all elements < pivot are left and > pivot are right, then recursively sort each partition.

**Average case O(n log n):** Good pivot splits array roughly in half → log n levels.
**Worst case O(n²):** Bad pivot (always min or max) → n levels. Mitigate with random pivot or median-of-three.

**Lomuto partition** (below): uses the last element as pivot. Simple but slightly slower than Hoare's.
**Hoare's partition**: uses two pointers from both ends. Faster in practice.

```java
void quickSort(int[] arr, int lo, int hi) {
    if (lo >= hi) return;
    int pivot = partition(arr, lo, hi);
    quickSort(arr, lo, pivot - 1);
    quickSort(arr, pivot + 1, hi);
}

int partition(int[] arr, int lo, int hi) {
    int pivot = arr[hi], i = lo;
    for (int j = lo; j < hi; j++)
        if (arr[j] < pivot) swap(arr, i++, j);
    swap(arr, i, hi);
    return i;
}

void swap(int[] arr, int i, int j) {
    int tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
}
```

### Heap Sort

**Theory:** Build a max-heap from the array (O(n)), then repeatedly extract the max (swap root with last unsorted element, then heapify). O(n log n) time, O(1) space. Not stable.

```java
void heapSort(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    for (int i = n - 1; i > 0; i--) {
        swap(arr, 0, i);
        heapify(arr, i, 0);
    }
}

void heapify(int[] arr, int n, int i) {
    int largest = i, l = 2 * i + 1, r = 2 * i + 2;
    if (l < n && arr[l] > arr[largest]) largest = l;
    if (r < n && arr[r] > arr[largest]) largest = r;
    if (largest != i) { swap(arr, i, largest); heapify(arr, n, largest); }
}
```

### Counting Sort

**Theory:** Non-comparison sort for integers in a known range [min, max]. Count occurrences, then reconstruct. O(n + k) where k = range of values. Only practical when k is not much larger than n.

```java
void countingSort(int[] arr) {
    int max = Arrays.stream(arr).max().orElse(0);
    int min = Arrays.stream(arr).min().orElse(0);
    int range = max - min + 1;
    int[] count = new int[range];
    for (int num : arr) count[num - min]++;
    int idx = 0;
    for (int i = 0; i < range; i++)
        while (count[i]-- > 0) arr[idx++] = i + min;
}
```

### Quick Select (Kth Smallest) — O(n) average

**Theory:** Same partitioning as Quick Sort, but only recurse into the partition containing the kth element. Average O(n), worst O(n²). This is the algorithm behind finding the kth smallest/largest element without fully sorting.

```java
int quickSelect(int[] arr, int lo, int hi, int k) {
    int pivot = partition(arr, lo, hi);
    if (pivot == k) return arr[pivot];
    return pivot < k ? quickSelect(arr, pivot + 1, hi, k) : quickSelect(arr, lo, pivot - 1, k);
}
```

---

## 13. Searching Algorithms

### Theory

**Searching** is finding an element (or its position) in a collection.

**Linear Search:** Check every element. O(n). Works on any array/list. No prerequisites.

**Binary Search:** Repeatedly halve the search space. O(log n). **Requires a sorted array** (or a monotonic condition).

**Binary Search Key Insight:** If you can frame a problem as "find the boundary where a condition changes from false to true (or vice versa)", binary search applies. This extends beyond sorted arrays to:
- Search on the **answer space** (e.g., "what's the minimum capacity to ship packages in D days?")
- Rotated sorted arrays
- Peak finding
- Square root, nth root

**Three Binary Search Templates:**

1. **Exact match** — `while (lo <= hi)`, return when found.
2. **Lower bound** — `while (lo < hi)`, `hi = mid` when condition met. Finds first element ≥ target.
3. **Upper bound** — `while (lo < hi)`, `lo = mid + 1` when condition met. Finds first element > target.

**Common pitfall:** Integer overflow in `(lo + hi) / 2`. Always use `lo + (hi - lo) / 2`.

**Binary Search on Answer:** When you're asked to find the minimum/maximum value that satisfies a constraint, binary search over the possible answer range and check feasibility at each midpoint. Time: O(n × log(answer range)).

### Binary Search Variants

```java
// Standard — find exact target
int binarySearch(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

// Lower bound — first index where arr[i] >= target
int lowerBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}

// Upper bound — first index where arr[i] > target
int upperBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] <= target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

### Binary Search on Answer (Template)

```java
// When searching for the minimum value that satisfies a condition
int binarySearchOnAnswer(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (feasible(mid)) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}

// When searching for the maximum value that satisfies a condition
int binarySearchOnAnswerMax(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo + 1) / 2; // round up to avoid infinite loop
        if (feasible(mid)) lo = mid;
        else hi = mid - 1;
    }
    return lo;
}
```

### Search in Rotated Sorted Array

**Idea:** A rotated sorted array has two sorted halves. At each step, determine which half is sorted, then check if the target falls within that sorted range to decide which half to search.

```java
int searchRotated(int[] nums, int target) {
    int lo = 0, hi = nums.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        if (nums[lo] <= nums[mid]) { // left half sorted
            if (nums[lo] <= target && target < nums[mid]) hi = mid - 1;
            else lo = mid + 1;
        } else { // right half sorted
            if (nums[mid] < target && target <= nums[hi]) lo = mid + 1;
            else hi = mid - 1;
        }
    }
    return -1;
}
```

### Find Peak Element

**Idea:** A peak is an element greater than its neighbors. If `nums[mid] < nums[mid+1]`, a peak must exist to the right (upward slope). Otherwise, a peak exists to the left (or mid is a peak). Binary search converges to a peak in O(log n).

```java
int findPeakElement(int[] nums) {
    int lo = 0, hi = nums.length - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] < nums[mid + 1]) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

### Median of Two Sorted Arrays — O(log(min(m,n)))

**Theory:** Binary search on the smaller array to find the correct partition. The partition divides both arrays such that all elements on the left are ≤ all elements on the right, and the left side has exactly `(m + n + 1) / 2` elements.

```java
double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);
    int m = nums1.length, n = nums2.length;
    int lo = 0, hi = m;
    while (lo <= hi) {
        int i = (lo + hi) / 2;
        int j = (m + n + 1) / 2 - i;
        int maxLeft1 = (i == 0) ? Integer.MIN_VALUE : nums1[i - 1];
        int minRight1 = (i == m) ? Integer.MAX_VALUE : nums1[i];
        int maxLeft2 = (j == 0) ? Integer.MIN_VALUE : nums2[j - 1];
        int minRight2 = (j == n) ? Integer.MAX_VALUE : nums2[j];

        if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
            if ((m + n) % 2 == 0)
                return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1, minRight2)) / 2.0;
            return Math.max(maxLeft1, maxLeft2);
        } else if (maxLeft1 > minRight2) hi = i - 1;
        else lo = i + 1;
    }
    throw new IllegalArgumentException();
}
```

---

## 14. Two Pointers

### Theory

**Two Pointers** is a technique where two index variables traverse a data structure (usually an array or linked list) simultaneously. It reduces brute-force O(n²) to O(n) in many problems.

**Variants:**

1. **Opposite Ends (Converging):** One pointer starts at the beginning, the other at the end. They move toward each other. Used for: sorted two-sum, container with most water, palindrome check, trapping rain water.

2. **Same Direction (Fast/Slow):** Both start at the beginning. The fast pointer moves ahead. Used for: remove duplicates, partition, linked list cycle detection, find middle.

3. **Two Arrays:** One pointer in each array. Used for: merge sorted arrays, intersection.

**Prerequisites:**
- Often requires a **sorted** array (for convergence to work correctly).
- For linked lists, no sorting needed (structural problems).

**When to Use:**
- Finding pairs with a specific sum in a sorted array.
- Removing duplicates in-place.
- Partitioning (move elements matching a condition to one side).
- Palindrome verification.
- Container/area problems.

### Two Sum (Sorted Array)

```java
int[] twoSumSorted(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int sum = nums[l] + nums[r];
        if (sum == target) return new int[]{l, r};
        else if (sum < target) l++;
        else r--;
    }
    return new int[]{};
}
```

### Three Sum

**Idea:** Sort the array. Fix one element, then use two pointers on the remaining elements to find pairs that sum to the negation of the fixed element. Skip duplicates to avoid duplicate triplets. O(n²) time.

```java
List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue; // skip duplicates
        int l = i + 1, r = nums.length - 1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                result.add(List.of(nums[i], nums[l], nums[r]));
                while (l < r && nums[l] == nums[l + 1]) l++;
                while (l < r && nums[r] == nums[r - 1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return result;
}
```

### Container With Most Water

**Idea:** Start with widest container (left and right ends). The area is limited by the shorter line. Move the pointer at the shorter line inward — moving the taller line can only reduce width without helping height.

```java
int maxArea(int[] height) {
    int l = 0, r = height.length - 1, max = 0;
    while (l < r) {
        max = Math.max(max, Math.min(height[l], height[r]) * (r - l));
        if (height[l] < height[r]) l++;
        else r--;
    }
    return max;
}
```

### Trapping Rain Water

**Theory:** Water above each bar = `min(maxLeft, maxRight) - height[i]`. Instead of precomputing leftMax and rightMax arrays (O(n) space), use two pointers from both ends. The key insight: we only need to know the **lesser** of the two maximums. If `height[l] < height[r]`, we know `leftMax` is the bottleneck, so process from the left. O(n) time, O(1) space.

```java
int trap(int[] height) {
    int l = 0, r = height.length - 1;
    int leftMax = 0, rightMax = 0, water = 0;
    while (l < r) {
        if (height[l] < height[r]) {
            leftMax = Math.max(leftMax, height[l]);
            water += leftMax - height[l];
            l++;
        } else {
            rightMax = Math.max(rightMax, height[r]);
            water += rightMax - height[r];
            r--;
        }
    }
    return water;
}
```

### Remove Duplicates In-Place

**Idea:** Slow pointer tracks the position of the last unique element. Fast pointer scans ahead. When fast finds a new unique element, copy it to slow+1.

```java
int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int slow = 0;
    for (int fast = 1; fast < nums.length; fast++)
        if (nums[fast] != nums[slow])
            nums[++slow] = nums[fast];
    return slow + 1;
}
```

### Linked List Palindrome Check

**Idea:** Find the middle (fast/slow pointers), reverse the second half, then compare both halves node by node.

```java
boolean isPalindrome(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) { slow = slow.next; fast = fast.next.next; }
    ListNode rev = reverse(slow);
    while (rev != null) {
        if (head.val != rev.val) return false;
        head = head.next;
        rev = rev.next;
    }
    return true;
}
```

---

## 15. Sliding Window

### Theory

**Sliding Window** is a technique that maintains a **window** (contiguous subarray/substring) and slides it across the data. Instead of recomputing everything from scratch for each position, we incrementally add the new element and remove the old one.

**Why it works:** If the problem asks about contiguous subarrays/substrings with some constraint, and adding/removing elements can be done in O(1), sliding window transforms O(n²) brute force into O(n).

**Two Types:**

**1. Fixed-Size Window:**
- Window size is given (e.g., maximum sum of subarray of size k).
- Expand until window reaches size k, then slide by adding right and removing left.
- Template: maintain a running calculation, update at each slide.

**2. Variable-Size Window:**
- Find the longest/shortest subarray satisfying a condition.
- **Expand** the right pointer to include more elements.
- **Shrink** the left pointer when the condition is violated.
- Track the best (longest/shortest) valid window.

**When to Use:**
- "Find the longest/shortest subarray/substring with..."
- "Maximum/minimum sum of subarray of size k"
- "Count subarrays/substrings where..."
- Contiguous elements with constraints on sum, frequency, distinct count, etc.

**Key Insight:** Both left and right pointers only move forward, so total operations = O(n).

### Fixed Size Window Template

```java
int maxSumFixed(int[] nums, int k) {
    int windowSum = 0, maxSum = 0;
    for (int i = 0; i < nums.length; i++) {
        windowSum += nums[i];
        if (i >= k) windowSum -= nums[i - k];
        if (i >= k - 1) maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### Variable Size Window Template

**Pattern:** Expand right → check condition → shrink left while condition violated → update answer.

```java
// Smallest subarray with sum >= target
int minSubArrayLen(int target, int[] nums) {
    int left = 0, sum = 0, minLen = Integer.MAX_VALUE;
    for (int right = 0; right < nums.length; right++) {
        sum += nums[right];
        while (sum >= target) {
            minLen = Math.min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == Integer.MAX_VALUE ? 0 : minLen;
}
```

### Longest Substring Without Repeating Characters

**Idea:** Expand right pointer. If a duplicate is found (tracked by a HashMap storing last seen index), jump the left pointer past the previous occurrence. The window `[left, right]` always contains unique characters.

```java
int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> lastSeen = new HashMap<>();
    int maxLen = 0, left = 0;
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (lastSeen.containsKey(c))
            left = Math.max(left, lastSeen.get(c) + 1);
        lastSeen.put(c, right);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

### Minimum Window Substring

**Theory:** Find the smallest window in `s` that contains all characters of `t`. Use a frequency array for needed characters and a counter for how many distinct characters are fully matched. Expand right until all characters are matched, then shrink left to minimize.

```java
String minWindow(String s, String t) {
    int[] need = new int[128], window = new int[128];
    for (char c : t.toCharArray()) need[c]++;

    int left = 0, matched = 0, start = 0, minLen = Integer.MAX_VALUE;
    int required = 0;
    for (int c : need) if (c > 0) required++;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        window[c]++;
        if (window[c] == need[c]) matched++;

        while (matched == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }
            char d = s.charAt(left++);
            if (window[d] == need[d]) matched--;
            window[d]--;
        }
    }
    return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
}
```

### Longest Repeating Character Replacement

**Idea:** In the window `[left, right]`, the number of characters we need to replace = `windowSize - maxFreq`. If this exceeds k, shrink the window. The key optimization: `maxFreq` doesn't need to decrease when we shrink — it only matters when it increases (because only a larger `maxFreq` can produce a longer valid window).

```java
int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int left = 0, maxFreq = 0, maxLen = 0;
    for (int right = 0; right < s.length(); right++) {
        maxFreq = Math.max(maxFreq, ++count[s.charAt(right) - 'A']);
        while (right - left + 1 - maxFreq > k)
            count[s.charAt(left++) - 'A']--;
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

---

## 16. Recursion & Backtracking

### Theory

**Recursion** is when a function calls itself. Every recursive solution has:
1. **Base case** — the simplest instance that can be answered directly (stops recursion).
2. **Recursive case** — breaks the problem into smaller subproblems and combines results.

**How recursion works:** Each call pushes a new frame onto the **call stack** containing local variables and the return address. When the base case is reached, frames are popped and results flow back up. Stack overflow occurs if recursion is too deep (Java default stack size: ~512KB to 1MB, roughly 5,000-10,000 frames).

**Tail Recursion:** When the recursive call is the **last** operation in the function. Some languages optimize this to a loop (tail-call optimization). Java does NOT optimize tail recursion, so prefer iterative solutions for deep recursion.

**Backtracking** is a refined form of recursion used to **build solutions incrementally** and **abandon (prune) partial solutions** that cannot lead to a valid complete solution.

**Backtracking Template:**
```
function backtrack(state, choices):
    if state is a complete solution:
        record solution
        return
    for each choice in choices:
        if choice is valid:
            make choice (modify state)
            backtrack(updated state, remaining choices)
            undo choice (restore state)  ← this is the "backtrack" step
```

**When to Use Backtracking:**
- **Subsets** — all possible subsets of a set.
- **Permutations** — all orderings of elements.
- **Combinations** — choose k elements from n.
- **Constraint satisfaction** — N-Queens, Sudoku, crosswords.
- **Path finding** — word search in a grid.

**Pruning:** The power of backtracking comes from **cutting off** branches early. Without pruning, backtracking is just brute-force enumeration.

**Time Complexities:**
- Subsets: O(2ⁿ) — each element is included or not.
- Permutations: O(n!) — n choices for first, n-1 for second, etc.
- Combinations C(n,k): O(C(n,k)) — binomial coefficient.

### How to Think Recursively

1. **Define the function's job clearly** — "This function returns X given Y."
2. **Trust the recursion (leap of faith)** — assume the recursive call correctly solves the smaller subproblem. Focus only on what the current call must do.
3. **Identify the base case** — the smallest input where the answer is trivially known.
4. **Relate the current problem to smaller subproblems** — how does the solution for n relate to the solution for n-1?
5. **Draw the recursion tree** — helps visualize the call stack, spot overlapping subproblems, and verify correctness.

### Tower of Hanoi

**Theory:** Move n disks from source to destination using an auxiliary peg. The rule: only one disk moves at a time, and a larger disk can never sit on a smaller one. Requires 2ⁿ - 1 moves.

```java
void towerOfHanoi(int n, char from, char to, char aux) {
    if (n == 0) return;
    towerOfHanoi(n - 1, from, aux, to);
    System.out.println("Move disk " + n + " from " + from + " to " + to);
    towerOfHanoi(n - 1, aux, to, from);
}
```

### Generate All Balanced Parentheses

**Theory:** At each step, we can add `(` if we haven't used all of them, or `)` if the count of `)` is less than `(`. This ensures every generated string is valid. O(4ⁿ / √n) — Catalan number.

```java
List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    backtrackParens(result, new StringBuilder(), 0, 0, n);
    return result;
}

void backtrackParens(List<String> result, StringBuilder sb, int open, int close, int max) {
    if (sb.length() == 2 * max) { result.add(sb.toString()); return; }
    if (open < max) {
        sb.append('(');
        backtrackParens(result, sb, open + 1, close, max);
        sb.deleteCharAt(sb.length() - 1);
    }
    if (close < open) {
        sb.append(')');
        backtrackParens(result, sb, open, close + 1, max);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

### Print All Subsequences

**Idea:** The simplest backtracking warmup. At each index, choose to include or exclude the element.

```java
void printSubsequences(int[] arr, int idx, List<Integer> current) {
    if (idx == arr.length) { System.out.println(current); return; }
    current.add(arr[idx]);
    printSubsequences(arr, idx + 1, current);  // include
    current.remove(current.size() - 1);
    printSubsequences(arr, idx + 1, current);  // exclude
}
```

### Letter Combinations of a Phone Number

```java
private static final String[] PHONE = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    if (digits.isEmpty()) return result;
    backtrackPhone(digits, 0, new StringBuilder(), result);
    return result;
}

void backtrackPhone(String digits, int idx, StringBuilder sb, List<String> result) {
    if (idx == digits.length()) { result.add(sb.toString()); return; }
    for (char c : PHONE[digits.charAt(idx) - '0'].toCharArray()) {
        sb.append(c);
        backtrackPhone(digits, idx + 1, sb, result);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

### Subsets

**Idea:** At each position, we have a choice: include the current element or skip it. We build subsets by iterating from `start` and adding each element.

```java
List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackSubsets(nums, 0, new ArrayList<>(), result);
    return result;
}

void backtrackSubsets(int[] nums, int start, List<Integer> path, List<List<Integer>> result) {
    result.add(new ArrayList<>(path));
    for (int i = start; i < nums.length; i++) {
        path.add(nums[i]);
        backtrackSubsets(nums, i + 1, path, result);
        path.remove(path.size() - 1);
    }
}
```

### Subsets with Duplicates

**Idea:** Sort first. Skip elements that are the same as the previous element at the same recursion level (i.e., `i > start && nums[i] == nums[i-1]`). This avoids generating duplicate subsets.

```java
List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    backtrackSubsetsDup(nums, 0, new ArrayList<>(), result);
    return result;
}

void backtrackSubsetsDup(int[] nums, int start, List<Integer> path, List<List<Integer>> result) {
    result.add(new ArrayList<>(path));
    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]) continue; // skip duplicates
        path.add(nums[i]);
        backtrackSubsetsDup(nums, i + 1, path, result);
        path.remove(path.size() - 1);
    }
}
```

### Permutations

**Idea:** At each step, choose any unused element. Use a `boolean[] used` array to track which elements are in the current path. When the path has all elements, record it.

```java
List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackPermute(nums, new boolean[nums.length], new ArrayList<>(), result);
    return result;
}

void backtrackPermute(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> result) {
    if (path.size() == nums.length) { result.add(new ArrayList<>(path)); return; }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        used[i] = true;
        path.add(nums[i]);
        backtrackPermute(nums, used, path, result);
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

### Permutations with Duplicates

**Idea:** Sort the array. Skip element `i` if it equals `i-1` and `i-1` wasn't used in the current path. This ensures each duplicate is used in order, preventing duplicate permutations.

```java
List<List<Integer>> permuteUnique(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    backtrackPermuteUnique(nums, new boolean[nums.length], new ArrayList<>(), result);
    return result;
}

void backtrackPermuteUnique(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> result) {
    if (path.size() == nums.length) { result.add(new ArrayList<>(path)); return; }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;
        used[i] = true;
        path.add(nums[i]);
        backtrackPermuteUnique(nums, used, path, result);
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

### Combination Sum

**Idea:** Pick candidates starting from index `start`. The same candidate can be reused (pass `i` not `i+1`). Stop when remaining becomes negative.

```java
List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackCombo(candidates, target, 0, new ArrayList<>(), result);
    return result;
}

void backtrackCombo(int[] candidates, int remaining, int start, List<Integer> path, List<List<Integer>> result) {
    if (remaining == 0) { result.add(new ArrayList<>(path)); return; }
    if (remaining < 0) return;
    for (int i = start; i < candidates.length; i++) {
        path.add(candidates[i]);
        backtrackCombo(candidates, remaining - candidates[i], i, path, result); // i not i+1: reuse allowed
        path.remove(path.size() - 1);
    }
}
```

### N-Queens

**Theory:** Place N queens on an N×N chessboard such that no two queens attack each other (same row, column, or diagonal). Place one queen per row, try each column, and backtrack when no valid column exists.

**Pruning:** Check column conflicts, main diagonal (row - col), and anti-diagonal (row + col).

```java
List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    for (char[] row : board) Arrays.fill(row, '.');
    placeQueens(board, 0, result);
    return result;
}

void placeQueens(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        List<String> snapshot = new ArrayList<>();
        for (char[] r : board) snapshot.add(new String(r));
        result.add(snapshot);
        return;
    }
    for (int col = 0; col < board.length; col++) {
        if (isSafe(board, row, col)) {
            board[row][col] = 'Q';
            placeQueens(board, row + 1, result);
            board[row][col] = '.';
        }
    }
}

boolean isSafe(char[][] board, int row, int col) {
    for (int i = 0; i < row; i++) if (board[i][col] == 'Q') return false;
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
        if (board[i][j] == 'Q') return false;
    for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++)
        if (board[i][j] == 'Q') return false;
    return true;
}
```

### Sudoku Solver

**Idea:** Find the first empty cell. Try digits 1-9. If a digit is valid (no conflict in row, column, or 3×3 box), place it and recurse. If no digit works, backtrack (reset to '.').

```java
boolean solveSudoku(char[][] board) {
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++)
            if (board[i][j] == '.') {
                for (char c = '1'; c <= '9'; c++) {
                    if (isValidPlacement(board, i, j, c)) {
                        board[i][j] = c;
                        if (solveSudoku(board)) return true;
                        board[i][j] = '.';
                    }
                }
                return false;
            }
    return true;
}

boolean isValidPlacement(char[][] board, int row, int col, char c) {
    for (int i = 0; i < 9; i++) {
        if (board[row][i] == c) return false;
        if (board[i][col] == c) return false;
        if (board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c) return false;
    }
    return true;
}
```

### Word Search

**Idea:** For each cell matching the first character, DFS in all 4 directions. Mark visited cells (set to '#') to avoid reuse. Backtrack by restoring the original character.

```java
boolean exist(char[][] board, String word) {
    for (int i = 0; i < board.length; i++)
        for (int j = 0; j < board[0].length; j++)
            if (dfsWord(board, word, i, j, 0)) return true;
    return false;
}

boolean dfsWord(char[][] board, String word, int i, int j, int idx) {
    if (idx == word.length()) return true;
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(idx))
        return false;
    char tmp = board[i][j];
    board[i][j] = '#';
    boolean found = dfsWord(board, word, i + 1, j, idx + 1)
                 || dfsWord(board, word, i - 1, j, idx + 1)
                 || dfsWord(board, word, i, j + 1, idx + 1)
                 || dfsWord(board, word, i, j - 1, idx + 1);
    board[i][j] = tmp;
    return found;
}
```

---

## 17. Dynamic Programming

### Theory

**Dynamic Programming (DP)** is an optimization technique that solves problems by breaking them into overlapping subproblems, solving each subproblem only once, and storing the results.

**Two Key Properties for DP to Apply:**

1. **Optimal Substructure:** The optimal solution to the problem contains optimal solutions to its subproblems. Example: the shortest path from A to C through B = shortest(A→B) + shortest(B→C).

2. **Overlapping Subproblems:** The same subproblems are solved repeatedly. Example: Fibonacci — `fib(5)` calls `fib(4)` and `fib(3)`, but `fib(4)` also calls `fib(3)`. Without memoization, this leads to exponential time.

**Two Approaches:**

| Approach | Description | Pros | Cons |
|---|---|---|---|
| **Top-Down (Memoization)** | Recursive + cache results | Intuitive, only solves needed subproblems | Stack overflow risk, recursive overhead |
| **Bottom-Up (Tabulation)** | Iterative, fill table from base cases up | No stack overflow, often faster | Must determine computation order, may solve unnecessary subproblems |

**Steps to Solve a DP Problem:**
1. **Define the state** — What does `dp[i]` (or `dp[i][j]`) represent?
2. **Find the recurrence** — How does `dp[i]` relate to smaller subproblems?
3. **Identify base cases** — What are the trivial cases?
4. **Determine computation order** — Fill the table so dependencies are satisfied.
5. **Optimize space** — Often only the last row/column is needed (rolling array).

**Common DP Patterns:**

| Pattern | Example |
|---|---|
| 1-D DP | Fibonacci, Climbing Stairs, House Robber |
| 2-D DP | Longest Common Subsequence, Edit Distance, Unique Paths |
| Knapsack | 0/1 Knapsack, Unbounded Knapsack, Subset Sum |
| Interval DP | Matrix Chain Multiplication, Burst Balloons |
| Bitmask DP | Travelling Salesman, Assignment Problem |
| DP on Trees | Max path sum, tree diameter |
| String DP | Palindrome partitioning, regex matching |

**DP vs Greedy:** Greedy makes the locally optimal choice and never backtracks. DP considers all options. Use greedy when the greedy choice provably leads to a global optimum. Use DP when greedy doesn't work (e.g., 0/1 knapsack).

**DP vs Recursion:** All DP problems can be solved recursively, but not all recursive problems need DP. DP applies when there are overlapping subproblems. If subproblems are independent (like merge sort), plain recursion (divide & conquer) suffices.

### Fibonacci / Climbing Stairs

```java
// Top-down with memoization — O(n) time, O(n) space
int fib(int n, int[] memo) {
    if (n <= 1) return n;
    if (memo[n] != 0) return memo[n];
    return memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
}

// Bottom-up with space optimization — O(n) time, O(1) space
int climbStairs(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### House Robber

**State:** `dp[i]` = max money robbing from houses 0..i.
**Recurrence:** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])` — either skip house i or rob it.
**Space optimization:** Only need the last two values.

```java
int rob(int[] nums) {
    int prev2 = 0, prev1 = 0;
    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### House Robber II (Circular)

**Idea:** Houses are in a circle, so house 0 and house n-1 are adjacent. Run House Robber twice: once excluding the last house, once excluding the first. Take the maximum.

```java
int robCircular(int[] nums) {
    if (nums.length == 1) return nums[0];
    return Math.max(robRange(nums, 0, nums.length - 2),
                    robRange(nums, 1, nums.length - 1));
}

int robRange(int[] nums, int lo, int hi) {
    int prev2 = 0, prev1 = 0;
    for (int i = lo; i <= hi; i++) {
        int curr = Math.max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### Longest Increasing Subsequence — O(n log n)

**Theory:** Maintain an array `tails` where `tails[i]` is the smallest possible tail element for an increasing subsequence of length `i+1`. For each new element, binary search for its position in `tails`:
- If it's larger than all tails, append it (extends the longest LIS).
- Otherwise, replace the first tail that's ≥ it (keeps `tails` as small as possible).

The length of `tails` is the LIS length. Note: `tails` is NOT the actual LIS.

```java
int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();
    for (int num : nums) {
        int pos = Collections.binarySearch(tails, num);
        if (pos < 0) pos = -(pos + 1);
        if (pos == tails.size()) tails.add(num);
        else tails.set(pos, num);
    }
    return tails.size();
}
```

### Longest Common Subsequence

**State:** `dp[i][j]` = LCS length of `text1[0..i-1]` and `text2[0..j-1]`.
**Recurrence:** If characters match, `dp[i][j] = dp[i-1][j-1] + 1`. Otherwise, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

```java
int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = text1.charAt(i - 1) == text2.charAt(j - 1)
                ? dp[i - 1][j - 1] + 1
                : Math.max(dp[i - 1][j], dp[i][j - 1]);
    return dp[m][n];
}
```

### Edit Distance

**State:** `dp[i][j]` = min operations to convert `word1[0..i-1]` to `word2[0..j-1]`.
**Recurrence:** If characters match, no operation needed (`dp[i-1][j-1]`). Otherwise, take the minimum of: insert (`dp[i][j-1]`), delete (`dp[i-1][j]`), replace (`dp[i-1][j-1]`) — each +1.

```java
int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;

    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            if (word1.charAt(i - 1) == word2.charAt(j - 1))
                dp[i][j] = dp[i - 1][j - 1];
            else
                dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1]));
    return dp[m][n];
}
```

### 0/1 Knapsack

**Theory:** Given items with weights and values, and a capacity limit, maximize total value without exceeding capacity. Each item can be used **at most once**.

**State:** `dp[w]` = max value achievable with capacity w.
**Recurrence:** `dp[w] = max(dp[w], dp[w - weight[i]] + value[i])`.
**Key:** Iterate weight **backwards** (from capacity down to weight[i]) so each item is used at most once.

```java
int knapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    int[] dp = new int[capacity + 1];
    for (int i = 0; i < n; i++)
        for (int w = capacity; w >= weights[i]; w--)
            dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
    return dp[capacity];
}
```

### Unbounded Knapsack

**Theory:** Same as 0/1 Knapsack but each item can be used **unlimited times**.
**Key difference:** Iterate weight **forwards** (from weight[i] to capacity) so items can be reused.

```java
int unboundedKnapsack(int[] weights, int[] values, int capacity) {
    int[] dp = new int[capacity + 1];
    for (int i = 0; i < weights.length; i++)
        for (int w = weights[i]; w <= capacity; w++) // forward loop: reuse items
            dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
    return dp[capacity];
}
```

### Coin Change (Min Coins)

**State:** `dp[a]` = minimum coins to make amount a.
**Recurrence:** `dp[a] = min(dp[a], dp[a - coin] + 1)` for each coin.
This is an unbounded knapsack variant (each coin can be used unlimited times).

```java
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int coin : coins)
        for (int a = coin; a <= amount; a++)
            dp[a] = Math.min(dp[a], dp[a - coin] + 1);
    return dp[amount] > amount ? -1 : dp[amount];
}
```

### Coin Change II (Number of Ways)

**State:** `dp[a]` = number of ways to make amount a.
**Key:** Iterate coins in the outer loop to avoid counting permutations (only combinations).

```java
int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;
    for (int coin : coins)
        for (int a = coin; a <= amount; a++)
            dp[a] += dp[a - coin];
    return dp[amount];
}
```

### Subset Sum (Partition Equal Subset Sum)

**Theory:** Can we partition the array into two subsets with equal sum? Equivalent: can we find a subset summing to `totalSum / 2`? This is a 0/1 knapsack problem with boolean values.

```java
boolean canPartition(int[] nums) {
    int total = Arrays.stream(nums).sum();
    if (total % 2 != 0) return false;
    int target = total / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;
    for (int num : nums)
        for (int j = target; j >= num; j--)
            dp[j] = dp[j] || dp[j - num];
    return dp[target];
}
```

### Unique Paths

**State:** `dp[j]` = number of ways to reach cell (i, j) in a grid.
**Recurrence:** `dp[j] += dp[j-1]` (come from left or above). Space-optimized to 1D.

```java
int uniquePaths(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[j] += dp[j - 1];
    return dp[n - 1];
}
```

### Longest Palindromic Substring

**State:** `dp[i][j]` = true if `s[i..j]` is a palindrome.
**Recurrence:** `dp[i][j] = (s[i] == s[j]) && (j - i < 3 || dp[i+1][j-1])`.
Fill bottom-up (start from the end).

```java
String longestPalindrome(String s) {
    int n = s.length(), start = 0, maxLen = 1;
    boolean[][] dp = new boolean[n][n];
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i; j < n; j++) {
            dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 3 || dp[i + 1][j - 1]);
            if (dp[i][j] && j - i + 1 > maxLen) {
                start = i;
                maxLen = j - i + 1;
            }
        }
    }
    return s.substring(start, start + maxLen);
}
```

### Word Break

**State:** `dp[i]` = true if `s[0..i-1]` can be segmented into dictionary words.
**Recurrence:** `dp[i] = true` if there exists `j < i` such that `dp[j]` is true and `s[j..i-1]` is in the dictionary.

```java
boolean wordBreak(String s, List<String> wordDict) {
    Set<String> dict = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    for (int i = 1; i <= s.length(); i++)
        for (int j = 0; j < i; j++)
            if (dp[j] && dict.contains(s.substring(j, i))) { dp[i] = true; break; }
    return dp[s.length()];
}
```

### Decode Ways

**State:** Number of ways to decode `s[0..i]`.
**Recurrence:** If `s[i]` is valid (1-9), add `dp[i-1]`. If `s[i-1..i]` is valid (10-26), add `dp[i-2]`.

```java
int numDecodings(String s) {
    if (s.charAt(0) == '0') return 0;
    int n = s.length();
    int prev2 = 1, prev1 = 1;
    for (int i = 1; i < n; i++) {
        int curr = 0;
        if (s.charAt(i) != '0') curr = prev1;
        int twoDigit = Integer.parseInt(s.substring(i - 1, i + 1));
        if (twoDigit >= 10 && twoDigit <= 26) curr += prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### Matrix Chain Multiplication

**Theory:** Given a chain of matrices, find the optimal order of multiplication to minimize total scalar multiplications. This is an **interval DP** problem: `dp[i][j]` = min cost to multiply matrices i through j. Try every split point k between i and j.

```java
int matrixChainOrder(int[] dims) {
    int n = dims.length - 1;
    int[][] dp = new int[n][n];
    for (int len = 2; len <= n; len++)
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i; k < j; k++)
                dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k + 1][j] + dims[i] * dims[k + 1] * dims[j + 1]);
        }
    return dp[0][n - 1];
}
```

### Maximum Path Sum in Binary Tree

**Idea:** At each node, the max path can either (a) extend through the node to its parent, or (b) "turn" at this node (include both left and right paths). We track the global max across all nodes. Each recursive call returns the max one-sided path sum (for the parent to use).

```java
int maxPathSum;

int maxPathSum(TreeNode root) {
    maxPathSum = Integer.MIN_VALUE;
    dfsPathSum(root);
    return maxPathSum;
}

int dfsPathSum(TreeNode node) {
    if (node == null) return 0;
    int left = Math.max(0, dfsPathSum(node.left));
    int right = Math.max(0, dfsPathSum(node.right));
    maxPathSum = Math.max(maxPathSum, left + right + node.val);
    return node.val + Math.max(left, right);
}
```

### Longest Palindromic Subsequence

**Theory:** The LPS of string `s` equals the LCS of `s` and `reverse(s)`. Alternatively, define `dp[i][j]` = LPS length in `s[i..j]`. O(n²) time and space.

```java
int longestPalinSubseq(String s) {
    int n = s.length();
    int[][] dp = new int[n][n];
    for (int i = n - 1; i >= 0; i--) {
        dp[i][i] = 1;
        for (int j = i + 1; j < n; j++)
            dp[i][j] = s.charAt(i) == s.charAt(j)
                ? dp[i + 1][j - 1] + 2
                : Math.max(dp[i + 1][j], dp[i][j - 1]);
    }
    return dp[0][n - 1];
}
```

### Palindrome Partitioning — Minimum Cuts

**Theory:** `dp[i]` = min cuts to partition `s[0..i]` into palindromes. For each position, check every valid palindrome ending at `i` and take the minimum. Use a 2D `isPalin` table for O(1) palindrome checks.

```java
int minCut(String s) {
    int n = s.length();
    boolean[][] isPalin = new boolean[n][n];
    int[] dp = new int[n];

    for (int i = 0; i < n; i++) {
        dp[i] = i; // worst case: cut before every char
        for (int j = 0; j <= i; j++) {
            if (s.charAt(j) == s.charAt(i) && (i - j < 3 || isPalin[j + 1][i - 1])) {
                isPalin[j][i] = true;
                dp[i] = (j == 0) ? 0 : Math.min(dp[i], dp[j - 1] + 1);
            }
        }
    }
    return dp[n - 1];
}
```

### Best Time to Buy and Sell Stock I (One Transaction)

**State:** Track the minimum price seen so far. At each day, the max profit = current price - min so far.

```java
int maxProfit(int[] prices) {
    int minPrice = Integer.MAX_VALUE, maxProfit = 0;
    for (int price : prices) {
        minPrice = Math.min(minPrice, price);
        maxProfit = Math.max(maxProfit, price - minPrice);
    }
    return maxProfit;
}
```

### Best Time to Buy and Sell Stock II (Unlimited Transactions)

**Idea:** Collect every upward slope. If price goes up from yesterday, add the difference. Greedy approach.

```java
int maxProfitII(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++)
        if (prices[i] > prices[i - 1])
            profit += prices[i] - prices[i - 1];
    return profit;
}
```

### Best Time to Buy and Sell Stock with Cooldown

**Theory:** Three states at each day: **hold** (holding a stock), **sold** (just sold), **rest** (cooling down or idle). Transitions:
- `hold[i] = max(hold[i-1], rest[i-1] - price[i])`
- `sold[i] = hold[i-1] + price[i]`
- `rest[i] = max(rest[i-1], sold[i-1])`

```java
int maxProfitCooldown(int[] prices) {
    int hold = Integer.MIN_VALUE, sold = 0, rest = 0;
    for (int price : prices) {
        int prevSold = sold;
        sold = hold + price;
        hold = Math.max(hold, rest - price);
        rest = Math.max(rest, prevSold);
    }
    return Math.max(sold, rest);
}
```

### Best Time to Buy and Sell Stock with Transaction Fee

**Theory:** Two states: **cash** (no stock) and **hold** (holding stock).
- `cash[i] = max(cash[i-1], hold[i-1] + price[i] - fee)`
- `hold[i] = max(hold[i-1], cash[i-1] - price[i])`

```java
int maxProfitWithFee(int[] prices, int fee) {
    int cash = 0, hold = -prices[0];
    for (int i = 1; i < prices.length; i++) {
        cash = Math.max(cash, hold + prices[i] - fee);
        hold = Math.max(hold, cash - prices[i]);
    }
    return cash;
}
```

---

## 18. Greedy Algorithms

### Theory

A **greedy algorithm** makes the **locally optimal choice** at each step, hoping to find the **globally optimal solution**. It never reconsiders previous choices (no backtracking).

**When Greedy Works:**
Greedy works when the problem has:
1. **Greedy Choice Property:** A locally optimal choice leads to a globally optimal solution.
2. **Optimal Substructure:** An optimal solution contains optimal solutions to subproblems.

**When Greedy Fails:**
- **0/1 Knapsack** — taking the item with the best value/weight ratio doesn't always give the optimal solution. Use DP instead.
- **Coin Change** (with arbitrary denominations) — greedy (take largest coin first) fails for some coin systems. e.g., coins = [1, 3, 4], amount = 6: greedy gives 4+1+1 (3 coins), optimal is 3+3 (2 coins).

**Greedy vs DP:** If you can prove the greedy choice is always safe, use greedy (simpler, faster). Otherwise, use DP.

**Common Greedy Problems:**
- **Activity Selection / Interval Scheduling** — sort by end time, greedily pick non-overlapping.
- **Huffman Coding** — always merge the two least frequent symbols.
- **Fractional Knapsack** — take items by value/weight ratio (greedy works because items are divisible).
- **Job Scheduling** — sort by deadline, use a priority queue.
- **Gas Station** — if total gas ≥ total cost, a solution exists; find the starting point greedily.

### Jump Game

**Idea:** Track the farthest index reachable. If at any point `i > farthest`, we can't proceed.

```java
boolean canJump(int[] nums) {
    int farthest = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i > farthest) return false;
        farthest = Math.max(farthest, i + nums[i]);
    }
    return true;
}
```

### Jump Game II (Minimum Jumps)

**Idea:** BFS-style level traversal. Each "level" is the range of indices reachable with the current number of jumps. Track the farthest reachable index within each level. When we pass the current level's end, we must jump.

```java
int jump(int[] nums) {
    int jumps = 0, curEnd = 0, farthest = 0;
    for (int i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);
        if (i == curEnd) { jumps++; curEnd = farthest; }
    }
    return jumps;
}
```

### Activity Selection / Meeting Rooms II

**Idea for Meeting Rooms II:** Find the minimum number of rooms needed. Sort start and end times separately. Scan: each start time needs a room; each end time frees a room. The maximum concurrent meetings = rooms needed.

```java
int minMeetingRooms(int[][] intervals) {
    int[] starts = new int[intervals.length], ends = new int[intervals.length];
    for (int i = 0; i < intervals.length; i++) {
        starts[i] = intervals[i][0];
        ends[i] = intervals[i][1];
    }
    Arrays.sort(starts);
    Arrays.sort(ends);
    int rooms = 0, endPtr = 0;
    for (int start : starts) {
        if (start < ends[endPtr]) rooms++;
        else endPtr++;
    }
    return rooms;
}
```

### Task Scheduler

**Theory:** The task with the highest frequency determines the minimum time. The formula: `(maxFreq - 1) * (n + 1) + count_of_max_freq_tasks`. The `(maxFreq - 1)` groups of `(n + 1)` slots are needed, plus one final group for all tasks with max frequency. If total tasks exceed this, no idle time is needed.

```java
int leastInterval(char[] tasks, int n) {
    int[] freq = new int[26];
    for (char t : tasks) freq[t - 'A']++;
    int maxFreq = Arrays.stream(freq).max().orElse(0);
    int maxCount = (int) Arrays.stream(freq).filter(f -> f == maxFreq).count();
    return Math.max(tasks.length, (maxFreq - 1) * (n + 1) + maxCount);
}
```

### Gas Station

**Idea:** If `totalGas >= totalCost`, a solution is guaranteed. To find the start: if `currentSurplus < 0` at station i, the start must be after i (because any start before i would also fail at i). Reset surplus and start from i+1.

```java
int canCompleteCircuit(int[] gas, int[] cost) {
    int totalSurplus = 0, currentSurplus = 0, start = 0;
    for (int i = 0; i < gas.length; i++) {
        totalSurplus += gas[i] - cost[i];
        currentSurplus += gas[i] - cost[i];
        if (currentSurplus < 0) { start = i + 1; currentSurplus = 0; }
    }
    return totalSurplus >= 0 ? start : -1;
}
```

### Partition Labels

**Idea:** Record the last occurrence of each character. The current partition must extend at least to the last occurrence of every character it includes. When the current index reaches the partition's end, cut here.

```java
List<Integer> partitionLabels(String s) {
    int[] lastIndex = new int[26];
    for (int i = 0; i < s.length(); i++)
        lastIndex[s.charAt(i) - 'a'] = i;

    List<Integer> result = new ArrayList<>();
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        end = Math.max(end, lastIndex[s.charAt(i) - 'a']);
        if (i == end) { result.add(end - start + 1); start = end + 1; }
    }
    return result;
}
```

### Huffman Coding (Concept)

**Theory:** Build an optimal prefix-free binary code by repeatedly merging the two lowest-frequency symbols. The more frequent a symbol, the shorter its code. This minimizes the total encoding length. Uses a min-heap.

```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
for (int freq : frequencies)
    pq.offer(new int[]{freq});

int totalCost = 0;
while (pq.size() > 1) {
    int[] a = pq.poll(), b = pq.poll();
    int merged = a[0] + b[0];
    totalCost += merged;
    pq.offer(new int[]{merged});
}
```

---

## 19. Bit Manipulation

### Theory

**Bit manipulation** operates directly on the binary representation of integers. It's extremely fast (single CPU instruction) and uses O(1) space.

**Binary Representation:**
- Positive integers: standard binary (e.g., 5 = `101`).
- Negative integers: **two's complement**. Flip all bits and add 1. e.g., -5 = `...11111011`.
- Java `int` is 32 bits, `long` is 64 bits.

**Bitwise Operators:**

| Operator | Symbol | Description | Example (a=5=101, b=3=011) |
|---|---|---|---|
| AND | `&` | Both bits must be 1 | 101 & 011 = 001 (1) |
| OR | `\|` | At least one bit is 1 | 101 \| 011 = 111 (7) |
| XOR | `^` | Bits must differ | 101 ^ 011 = 110 (6) |
| NOT | `~` | Flip all bits | ~101 = ...11111010 (-6) |
| Left Shift | `<<` | Shift bits left (×2) | 101 << 1 = 1010 (10) |
| Right Shift | `>>` | Shift bits right (÷2), sign-extending | |
| Unsigned Right Shift | `>>>` | Shift right, zero-filling | |

**XOR Properties (crucial for interviews):**
- `a ^ a = 0` (self-cancellation)
- `a ^ 0 = a` (identity)
- Commutative and associative: `a ^ b ^ c = c ^ a ^ b`
- If `a ^ b = c`, then `a ^ c = b` (reversible)

**Useful Bit Tricks:**
- `n & (n - 1)` — removes the lowest set bit. If result is 0, n is a power of 2.
- `n & (-n)` — isolates the lowest set bit.
- `n >> i & 1` — gets the ith bit.
- XOR all elements — finds the unique element when all others appear twice.

**When to Use:**
- Finding unique numbers (XOR).
- Power of 2 checks.
- Generating subsets (bitmask enumeration).
- State compression in DP (bitmask DP).
- Efficient flag/set operations.

### Core Operations

```java
int a = 5;  // 101
int b = 3;  // 011
a & b;      // 001 = 1
a | b;      // 111 = 7
a ^ b;      // 110 = 6
~a;         // ...11111010 (flips all bits)
a << 1;     // 1010 = 10 (multiply by 2)
a >> 1;     // 10 = 2 (divide by 2)
a >>> 1;    // unsigned right shift (fills 0 from left)
```

### Common Tricks

```java
// Check if n is power of 2
boolean isPowerOf2(int n) { return n > 0 && (n & (n - 1)) == 0; }

// Count set bits (Hamming weight / popcount)
int countBits(int n) {
    int count = 0;
    while (n != 0) { n &= (n - 1); count++; }
    return count;
}
// Or simply: Integer.bitCount(n);

// Get ith bit (0-indexed from right)
boolean getBit(int n, int i) { return (n & (1 << i)) != 0; }

// Set ith bit
int setBit(int n, int i) { return n | (1 << i); }

// Clear ith bit
int clearBit(int n, int i) { return n & ~(1 << i); }

// Toggle ith bit
int toggleBit(int n, int i) { return n ^ (1 << i); }

// Lowest set bit
int lowestSetBit(int n) { return n & (-n); }

// Check if even/odd
boolean isEven(int n) { return (n & 1) == 0; }

// Swap without temp
void swap(int a, int b) { a ^= b; b ^= a; a ^= b; }
```

### Single Number (XOR trick)

**Idea:** XOR of a number with itself is 0. XOR of a number with 0 is itself. XOR all elements — duplicates cancel out, leaving the unique one.

```java
int singleNumber(int[] nums) {
    int result = 0;
    for (int n : nums) result ^= n;
    return result;
}
```

### Single Number III (Two unique numbers)

**Idea:** XOR all elements → get `a ^ b`. Find any bit where a and b differ (use lowest set bit). Split all numbers into two groups based on that bit. XOR each group separately to get a and b.

```java
int[] singleNumberIII(int[] nums) {
    int xor = 0;
    for (int n : nums) xor ^= n;
    int diffBit = xor & (-xor); // lowest set bit
    int a = 0, b = 0;
    for (int n : nums) {
        if ((n & diffBit) != 0) a ^= n;
        else b ^= n;
    }
    return new int[]{a, b};
}
```

### Generate All Subsets Using Bitmask

**Idea:** For n elements, there are 2ⁿ subsets. Each subset maps to a binary number from 0 to 2ⁿ-1. The ith bit being 1 means element i is included.

```java
List<List<Integer>> subsets(int[] nums) {
    int n = nums.length;
    List<List<Integer>> result = new ArrayList<>();
    for (int mask = 0; mask < (1 << n); mask++) {
        List<Integer> subset = new ArrayList<>();
        for (int i = 0; i < n; i++)
            if ((mask & (1 << i)) != 0) subset.add(nums[i]);
        result.add(subset);
    }
    return result;
}
```

### Bitmask DP Example — Travelling Salesman

**Theory:** Visit all n cities exactly once and return to start with minimum cost. State: `dp[mask][u]` = min cost to visit cities in `mask` ending at city u. Transition: for each unvisited city v, `dp[mask | (1<<v)][v] = min(dp[mask][u] + dist[u][v])`. Time: O(2ⁿ · n²). Practical for n ≤ 20.

```java
int tsp(int[][] dist) {
    int n = dist.length;
    int[][] dp = new int[1 << n][n];
    for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE / 2);
    dp[1][0] = 0;

    for (int mask = 1; mask < (1 << n); mask++)
        for (int u = 0; u < n; u++) {
            if ((mask & (1 << u)) == 0) continue;
            for (int v = 0; v < n; v++) {
                if ((mask & (1 << v)) != 0) continue;
                dp[mask | (1 << v)][v] = Math.min(dp[mask | (1 << v)][v], dp[mask][u] + dist[u][v]);
            }
        }

    int fullMask = (1 << n) - 1;
    int ans = Integer.MAX_VALUE;
    for (int u = 1; u < n; u++)
        ans = Math.min(ans, dp[fullMask][u] + dist[u][0]);
    return ans;
}
```

---

## 20. Math & Number Theory

### Theory

**Number Theory** deals with properties of integers. Common topics in DSA:

**Divisibility & GCD:**
- `a` divides `b` (written `a | b`) if `b % a == 0`.
- **GCD** (Greatest Common Divisor) — largest number dividing both a and b. Euclidean algorithm: `gcd(a, b) = gcd(b, a % b)`. O(log(min(a,b))).
- **LCM** (Least Common Multiple) — `lcm(a, b) = a * b / gcd(a, b)`. Compute as `a / gcd(a,b) * b` to avoid overflow.

**Prime Numbers:**
- A prime has exactly two divisors: 1 and itself.
- **Trial division**: check divisibility up to √n. O(√n).
- **Sieve of Eratosthenes**: find all primes up to n in O(n log log n). Mark multiples of each prime as composite.
- **Fundamental Theorem of Arithmetic**: every integer > 1 has a unique prime factorization.

**Modular Arithmetic:**
- `(a + b) % m = ((a % m) + (b % m)) % m`
- `(a * b) % m = ((a % m) * (b % m)) % m`
- `(a - b) % m = ((a % m) - (b % m) + m) % m`
- Division requires **modular inverse**: `a / b mod m = a * b⁻¹ mod m`.
- **Fermat's Little Theorem**: if m is prime, `a^(m-1) ≡ 1 (mod m)`, so `a⁻¹ ≡ a^(m-2) (mod m)`.

**Fast Exponentiation (Binary Exponentiation):**
Compute `base^exp mod m` in O(log exp) by squaring the base and halving the exponent at each step.

**Combinatorics:**
- `C(n, k) = n! / (k! * (n-k)!)` — number of ways to choose k items from n.
- **Pascal's Triangle**: `C(n, k) = C(n-1, k-1) + C(n-1, k)`.
- For large n with modular arithmetic, precompute factorials and use modular inverse for division.

### GCD & LCM

```java
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
int lcm(int a, int b) { return a / gcd(a, b) * b; }
```

### Sieve of Eratosthenes

```java
boolean[] sieve(int n) {
    boolean[] isPrime = new boolean[n + 1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= n; i++)
        if (isPrime[i])
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
    return isPrime;
}
```

### Fast Exponentiation (mod)

```java
long power(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) result = result * base % mod;
        exp >>= 1;
        base = base * base % mod;
    }
    return result;
}
```

### Modular Inverse (Fermat's Little Theorem)

```java
// Only when mod is prime
long modInverse(long a, long mod) {
    return power(a, mod - 2, mod);
}
```

### nCr mod p

```java
long nCr(int n, int r, long mod) {
    if (r > n) return 0;
    long[] fact = new long[n + 1];
    fact[0] = 1;
    for (int i = 1; i <= n; i++) fact[i] = fact[i - 1] * i % mod;
    return fact[n] % mod * modInverse(fact[r], mod) % mod * modInverse(fact[n - r], mod) % mod;
}
```

### Check Prime

```java
boolean isPrime(int n) {
    if (n < 2) return false;
    if (n < 4) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    for (int i = 5; i * i <= n; i += 6)
        if (n % i == 0 || n % (i + 2) == 0) return false;
    return true;
}
```

### Prefix XOR

```java
// XOR of all numbers from 1 to n in O(1)
// Pattern repeats every 4: n, 1, n+1, 0
int xorUptoN(int n) {
    return switch (n % 4) {
        case 0 -> n;
        case 1 -> 1;
        case 2 -> n + 1;
        default -> 0;
    };
}
```

---

## 21. Union-Find (Disjoint Set)

### Theory

**Union-Find (Disjoint Set Union / DSU)** is a data structure that tracks a collection of non-overlapping sets. It supports two operations:

- **Find(x):** Determine which set x belongs to (find the root/representative).
- **Union(x, y):** Merge the sets containing x and y.

**Optimizations that make it nearly O(1) per operation:**

1. **Path Compression (on Find):** When finding the root, make every node on the path point directly to the root. This flattens the tree, making future finds faster.

2. **Union by Rank/Size:** Always attach the smaller tree under the root of the larger tree. This keeps the tree height logarithmic.

**With both optimizations:** Amortized O(α(n)) per operation, where α is the inverse Ackermann function — effectively constant (≤ 4 for any practical n).

**When to Use:**
- **Connected components** — dynamically add edges and query connectivity.
- **Cycle detection in undirected graphs** — if `find(u) == find(v)` before adding edge (u,v), adding it creates a cycle.
- **Kruskal's MST** — use Union-Find to efficiently check if adding an edge creates a cycle.
- **Accounts merge, redundant connection, number of provinces** — problems involving merging groups.

### Optimized Implementation

```java
class UnionFind {
    int[] parent, rank;
    int components;

    UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        components = n;
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]); // path compression
        return parent[x];
    }

    boolean union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) { int t = px; px = py; py = t; }
        parent[py] = px;
        if (rank[px] == rank[py]) rank[px]++;
        components--;
        return true;
    }

    boolean connected(int x, int y) { return find(x) == find(y); }
}
```

### Applications

```java
// Number of connected components
int countComponents(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    for (int[] e : edges) uf.union(e[0], e[1]);
    return uf.components;
}

// Detect cycle in undirected graph
boolean hasCycle(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    for (int[] e : edges)
        if (!uf.union(e[0], e[1])) return true;
    return false;
}

// Redundant Connection — find the edge that creates a cycle
int[] findRedundantConnection(int[][] edges) {
    UnionFind uf = new UnionFind(edges.length + 1);
    for (int[] e : edges)
        if (!uf.union(e[0], e[1])) return e;
    return new int[]{};
}
```

---

## 22. Segment Trees & BITs

### Theory

**Segment Tree** and **Binary Indexed Tree (BIT / Fenwick Tree)** are data structures for efficient **range queries** and **point updates** on arrays.

**When You Need Them:**
- The array changes (updates) and you need repeated range queries.
- Without them: range query is O(n), with them: O(log n) per query AND update.

**Segment Tree:**
- A full binary tree where each node stores information about a range of the array.
- Leaf nodes = individual elements. Internal nodes = aggregate of children.
- **Build:** O(n) | **Query:** O(log n) | **Update:** O(log n) | **Space:** O(4n)
- Supports: range sum, range min/max, range GCD, etc.
- Can be extended with **lazy propagation** for range updates in O(log n).

**Binary Indexed Tree (Fenwick Tree):**
- A compact array-based structure that supports prefix queries and point updates.
- **Build:** O(n log n) | **Query:** O(log n) | **Update:** O(log n) | **Space:** O(n)
- Simpler to implement than segment tree, but less flexible (mainly for prefix sums).
- Uses the binary representation of indices to navigate the tree.

**Segment Tree vs BIT:**

| Feature | Segment Tree | BIT |
|---|---|---|
| Implementation | Complex | Simple |
| Space | O(4n) | O(n) |
| Range queries | Any associative operation | Prefix sums only |
| Range updates | Yes (with lazy propagation) | Limited |
| Flexibility | Very flexible | Limited but sufficient for many problems |

### Segment Tree (Range Sum Query + Point Update)

```java
class SegmentTree {
    int[] tree;
    int n;

    SegmentTree(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];
        build(arr, 1, 0, n - 1);
    }

    void build(int[] arr, int node, int start, int end) {
        if (start == end) { tree[node] = arr[start]; return; }
        int mid = (start + end) / 2;
        build(arr, 2 * node, start, mid);
        build(arr, 2 * node + 1, mid + 1, end);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) { tree[node] = val; return; }
        int mid = (start + end) / 2;
        if (idx <= mid) update(2 * node, start, mid, idx, val);
        else update(2 * node + 1, mid + 1, end, idx, val);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;
        if (l <= start && end <= r) return tree[node];
        int mid = (start + end) / 2;
        return query(2 * node, start, mid, l, r) + query(2 * node + 1, mid + 1, end, l, r);
    }

    void update(int idx, int val) { update(1, 0, n - 1, idx, val); }
    int query(int l, int r) { return query(1, 0, n - 1, l, r); }
}
```

### Binary Indexed Tree (Fenwick Tree)

**Key Insight:** The tree structure is implicit in the binary representation of indices. `i & (-i)` gives the lowest set bit, which determines the range each node covers. Update propagates upward, query propagates downward.

```java
class BIT {
    int[] tree;
    int n;

    BIT(int n) { this.n = n; tree = new int[n + 1]; }

    void update(int i, int delta) {
        for (i++; i <= n; i += i & (-i)) tree[i] += delta;
    }

    int query(int i) {
        int sum = 0;
        for (i++; i > 0; i -= i & (-i)) sum += tree[i];
        return sum;
    }

    int rangeQuery(int l, int r) {
        return query(r) - (l > 0 ? query(l - 1) : 0);
    }
}
```

---

## 23. Intervals

### Theory

**Interval problems** deal with ranges `[start, end]` and their relationships (overlap, merge, gap).

**Two intervals [a, b] and [c, d] overlap if:** `a <= d && c <= b` (assuming sorted: `a <= b`, `c <= d`).

**Key Technique:** Almost always **sort intervals by start time** (or end time for scheduling problems).

**Common Interval Patterns:**
1. **Merge overlapping intervals** — sort by start, merge consecutive overlaps.
2. **Insert a new interval** — add intervals before, merge overlapping, add intervals after.
3. **Find max overlapping intervals** — sweep line or sort start/end separately.
4. **Minimum removals for non-overlapping** — sort by end time (greedy: keep intervals that end earliest).

**Sweep Line Technique:** Convert intervals into events (start, end). Sort events. Scan left to right, tracking active intervals. Useful for finding maximum overlap, skyline problems, etc.

### Merge Intervals

```java
int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> merged = new ArrayList<>();
    for (int[] interval : intervals) {
        if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0])
            merged.add(interval);
        else
            merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], interval[1]);
    }
    return merged.toArray(new int[0][]);
}
```

### Insert Interval

```java
int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0, n = intervals.length;
    while (i < n && intervals[i][1] < newInterval[0])
        result.add(intervals[i++]);
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.add(newInterval);
    while (i < n) result.add(intervals[i++]);
    return result.toArray(new int[0][]);
}
```

### Non-Overlapping Intervals (Min Removals)

**Idea:** Sort by end time. Greedily keep intervals that end earliest — this leaves the most room for future intervals. Count how many we must remove (i.e., those that overlap with the kept one).

```java
int eraseOverlapIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]); // sort by end time
    int count = 0, prevEnd = Integer.MIN_VALUE;
    for (int[] interval : intervals) {
        if (interval[0] >= prevEnd) prevEnd = interval[1];
        else count++;
    }
    return count;
}
```

---

## 24. Monotonic Stack / Queue

### Theory

A **monotonic stack** is a stack where elements are always in increasing (or decreasing) order. When a new element would break the order, we pop elements from the stack (and process them) until the order is maintained.

**Why O(n)?** Each element is pushed and popped **at most once** across the entire traversal, giving O(n) total operations.

**Monotonic Stack Variants:**
- **Decreasing Stack** — finds the **next greater** element for each position. Pop when current > top.
- **Increasing Stack** — finds the **next smaller** element for each position. Pop when current < top.

**Common Applications:**
- **Next Greater Element** — for each element, find the first larger element to its right.
- **Next Smaller Element** — for each element, find the first smaller element to its right.
- **Largest Rectangle in Histogram** — use an increasing stack to track bar boundaries.
- **Daily Temperatures** — how many days until a warmer day.
- **Stock Span** — consecutive days with price ≤ today's.
- **Maximal Rectangle in Binary Matrix** — histogram approach per row.

**Monotonic Deque:** A deque that maintains monotonic order on both ends. Used for **sliding window min/max** in O(n).

### Next Greater Element (Circular Array)

**Idea:** For a circular array, iterate `2 * n` times (two passes over the array using modulo) to wrap around. Only push indices during the first pass.

```java
int[] nextGreaterElements(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < 2 * n; i++) {
        while (!stack.isEmpty() && nums[stack.peek()] < nums[i % n])
            result[stack.pop()] = nums[i % n];
        if (i < n) stack.push(i);
    }
    return result;
}
```

### Daily Temperatures

**Idea:** Maintain a decreasing stack of indices. When a warmer temperature is found, pop all cooler temperatures and set their result to the day difference.

```java
int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] result = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i])
            result[stack.peek()] = i - stack.pop();
        stack.push(i);
    }
    return result;
}
```

### Stock Span

```java
int[] stockSpan(int[] prices) {
    int n = prices.length;
    int[] span = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && prices[stack.peek()] <= prices[i])
            stack.pop();
        span[i] = stack.isEmpty() ? i + 1 : i - stack.peek();
        stack.push(i);
    }
    return span;
}
```

---

## 25. Topological Sort

### Theory

**Topological Sort** produces a linear ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge u → v, u comes before v in the ordering.

**Key Facts:**
- Only possible for **DAGs** (Directed Acyclic Graphs). If a cycle exists, no valid ordering exists.
- A DAG may have **multiple valid** topological orderings.
- Used to model **dependencies** (tasks, courses, build systems, package managers).

**Two Algorithms:**

**1. Kahn's Algorithm (BFS-based):**
1. Compute in-degree for each node.
2. Enqueue all nodes with in-degree 0.
3. Process queue: for each node, add to result, decrement in-degree of neighbors. Enqueue neighbors that reach in-degree 0.
4. If result size ≠ number of nodes → cycle exists.

**2. DFS-based:**
1. Run DFS from each unvisited node.
2. After exploring all neighbors of a node, push it onto a stack (postorder).
3. The stack (reversed) gives the topological order.

**Kahn's vs DFS:**
- Kahn's naturally detects cycles (result size < n).
- DFS gives one valid ordering directly on the stack.
- Kahn's can find all nodes at each "level" (useful for parallel scheduling).

### Kahn's Algorithm (BFS)

```java
List<Integer> topologicalSort(int n, List<List<Integer>> adj) {
    int[] inDegree = new int[n];
    for (List<Integer> neighbors : adj)
        for (int v : neighbors) inDegree[v]++;

    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++)
        if (inDegree[i] == 0) queue.offer(i);

    List<Integer> order = new ArrayList<>();
    while (!queue.isEmpty()) {
        int u = queue.poll();
        order.add(u);
        for (int v : adj.get(u))
            if (--inDegree[v] == 0) queue.offer(v);
    }
    return order.size() == n ? order : List.of(); // empty = cycle detected
}
```

### DFS-Based Topological Sort

```java
List<Integer> topologicalSortDFS(int n, List<List<Integer>> adj) {
    boolean[] visited = new boolean[n];
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < n; i++)
        if (!visited[i]) dfsTopoSort(adj, i, visited, stack);
    return new ArrayList<>(stack);
}

void dfsTopoSort(List<List<Integer>> adj, int u, boolean[] visited, Deque<Integer> stack) {
    visited[u] = true;
    for (int v : adj.get(u))
        if (!visited[v]) dfsTopoSort(adj, v, visited, stack);
    stack.push(u);
}
```

### Course Schedule (Cycle Detection via Topological Sort)

**Idea:** If we can topologically sort all courses (process all of them), all prerequisites can be satisfied. If not (cycle exists), it's impossible.

```java
boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> adj = new ArrayList<>();
    int[] inDegree = new int[numCourses];
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) { adj.get(p[1]).add(p[0]); inDegree[p[0]]++; }

    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++)
        if (inDegree[i] == 0) queue.offer(i);

    int count = 0;
    while (!queue.isEmpty()) {
        int u = queue.poll();
        count++;
        for (int v : adj.get(u))
            if (--inDegree[v] == 0) queue.offer(v);
    }
    return count == numCourses;
}
```

---

## 26. Common Patterns Quick Reference

### Pattern Index

| # | Pattern | When to Use | Time |
|---|---|---|---|
| 1 | **Sliding Window** | Subarray/substring with size k or a constraint | O(n) |
| 2 | **Two Pointers** | Sorted array, pair search, partitioning | O(n) |
| 3 | **Fast & Slow Pointers** | Cycle detection, middle of linked list | O(n) |
| 4 | **Merge Intervals** | Overlapping intervals | O(n log n) |
| 5 | **Cyclic Sort** | Numbers in range [0, n] or [1, n] | O(n) |
| 6 | **In-place Reversal of LL** | Reverse part of a linked list | O(n) |
| 7 | **Tree BFS** | Level-order traversal | O(n) |
| 8 | **Tree DFS** | Inorder, preorder, postorder, path sum | O(n) |
| 9 | **Two Heaps** | Median in a stream, scheduling | O(n log n) |
| 10 | **Subsets / Backtracking** | Permutations, combinations, power set | O(2ⁿ) or O(n!) |
| 11 | **Modified Binary Search** | Sorted/rotated array, boundary search | O(log n) |
| 12 | **Top K Elements** | Kth largest, most frequent | O(n log k) |
| 13 | **K-Way Merge** | Merge K sorted lists/arrays | O(N log k) |
| 14 | **Topological Sort** | Task scheduling, prerequisites | O(V + E) |
| 15 | **Union-Find** | Connected components, cycle in undirected graph | O(α(n)) |
| 16 | **Monotonic Stack** | Next greater/smaller element | O(n) |
| 17 | **Prefix Sum** | Range sum queries | O(n) build, O(1) query |
| 18 | **Bit Manipulation** | XOR tricks, subsets, power of 2 | O(n) or O(1) |
| 19 | **DP** | Optimal substructure + overlapping subproblems | Varies |
| 20 | **Greedy** | Local optimum leads to global optimum | Varies |

### How to Identify the Right Pattern

```
Is the input sorted or can it be sorted?
├── Yes → Two Pointers, Binary Search, or Merge Intervals
└── No
    Is it a subarray/substring problem?
    ├── Yes → Sliding Window or Prefix Sum
    └── No
        Is it a tree/graph?
        ├── Tree → DFS or BFS
        ├── Graph with weights → Dijkstra or Bellman-Ford
        ├── Graph unweighted → BFS for shortest path
        ├── DAG → Topological Sort
        └── No
            Does it involve making choices with overlapping subproblems?
            ├── Yes → Dynamic Programming
            └── No
                Does a local optimal choice lead to global optimal?
                ├── Yes → Greedy
                └── No
                    Generate all combinations/permutations?
                    ├── Yes → Backtracking
                    └── Bit Manipulation, Math, or custom approach
```

### Complexity Cheatsheet for Java Collections

| Collection | Add | Remove | Get | Contains | Notes |
|---|---|---|---|---|---|
| ArrayList | O(1)* | O(n) | O(1) | O(n) | *amortized |
| LinkedList | O(1) | O(1)** | O(n) | O(n) | **if you have the node |
| HashMap | O(1)* | O(1)* | O(1)* | O(1)* | *amortized avg |
| TreeMap | O(log n) | O(log n) | O(log n) | O(log n) | Sorted keys |
| HashSet | O(1)* | O(1)* | — | O(1)* | *amortized avg |
| TreeSet | O(log n) | O(log n) | — | O(log n) | Sorted |
| PriorityQueue | O(log n) | O(log n) | O(1)*** | O(n) | ***peek only |
| ArrayDeque | O(1)* | O(1)* | O(1)† | O(n) | †first/last only |

### Java I/O for Competitive Programming

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        int n = Integer.parseInt(br.readLine().trim());
        StringTokenizer st = new StringTokenizer(br.readLine());
        int[] arr = new int[n];
        for (int i = 0; i < n; i++)
            arr[i] = Integer.parseInt(st.nextToken());

        // ... solve ...

        sb.append(result).append('\n');
        System.out.print(sb);
    }
}
```

---

## 27. Java Tips & Pitfalls for DSA

### Type Conversions

```java
// String ↔ int
int num = Integer.parseInt("123");
String str = String.valueOf(123);  // or Integer.toString(123)

// String ↔ char[]
char[] chars = "hello".toCharArray();
String s = new String(chars);

// int[] ↔ List<Integer>
int[] arr = {1, 2, 3};
List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
int[] back = list.stream().mapToInt(Integer::intValue).toArray();

// List<Integer> → int[] (alternative)
int[] back2 = list.stream().mapToInt(i -> i).toArray();

// String[] ↔ List<String>
List<String> sList = Arrays.asList("a", "b");     // fixed-size
List<String> sList2 = new ArrayList<>(Arrays.asList("a", "b")); // mutable

// 2D List → 2D array
List<int[]> listOfArrays = new ArrayList<>();
int[][] result = listOfArrays.toArray(new int[0][]);

// char → int (digit)
int digit = '7' - '0'; // 7

// int → char (digit)
char c = (char) ('0' + 7); // '7'
```

### Comparator Patterns

```java
// Sort by single field
Arrays.sort(arr, (a, b) -> a - b);                       // ascending
Arrays.sort(arr, (a, b) -> b - a);                       // descending
Arrays.sort(arr, Comparator.reverseOrder());              // descending (wrapper types)

// DANGER: a - b can overflow! Use this for safety:
Arrays.sort(arr, Integer::compare);                       // safe ascending
Arrays.sort(arr, (a, b) -> Integer.compare(b, a));       // safe descending

// Sort by multiple fields
Arrays.sort(people, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);

// Using Comparator.comparing (for objects)
list.sort(Comparator.comparing(Person::getAge)
                     .thenComparing(Person::getName)
                     .reversed());

// Sort 2D array: by start, then by end descending
Arrays.sort(intervals, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
```

### Collections Utilities

```java
Collections.sort(list);                     // sort ascending
Collections.sort(list, Collections.reverseOrder()); // sort descending
Collections.reverse(list);                  // reverse in place
Collections.swap(list, i, j);              // swap two elements
Collections.min(list);                      // minimum element
Collections.max(list);                      // maximum element
Collections.frequency(list, element);       // count occurrences
Collections.fill(list, value);             // fill all with value
Collections.nCopies(n, value);             // immutable list of n copies
Collections.unmodifiableList(list);        // read-only view
Collections.singletonList(x);             // immutable single-element list
```

### Integer Overflow Prevention

```java
// DANGER: a + b can overflow int
int a = Integer.MAX_VALUE, b = 1;
int bad = a + b;          // overflows to Integer.MIN_VALUE!

// FIX 1: Use long for intermediate calculations
long safe = (long) a + b;

// FIX 2: Mid-point calculation
int mid = lo + (hi - lo) / 2;      // safe
int bad_mid = (lo + hi) / 2;       // can overflow!

// FIX 3: Multiplication overflow
long product = (long) a * b;       // cast BEFORE multiply

// FIX 4: Use Math.addExact / Math.multiplyExact (throws on overflow)
try {
    int result = Math.addExact(a, b);
} catch (ArithmeticException e) {
    // overflow occurred
}

// Common overflow traps:
// - Comparator: (a, b) -> a - b overflows. Use Integer.compare(a, b).
// - Sum of array elements: use long accumulator.
// - n * (n - 1) / 2: use long.
```

### Useful Tricks

```java
// Initialize array with value
int[] arr = new int[n];
Arrays.fill(arr, -1);

// Initialize 2D array with value
int[][] grid = new int[m][n];
for (int[] row : grid) Arrays.fill(row, -1);

// Swap two variables
int tmp = a; a = b; b = tmp;

// Ceiling division without floating point
int ceil = (a + b - 1) / b;    // equivalent to Math.ceil((double)a / b)

// Check if char is alphanumeric
Character.isLetterOrDigit(c);

// Convert between bases
Integer.toBinaryString(10);      // "1010"
Integer.toHexString(255);        // "ff"
Integer.parseInt("1010", 2);     // 10

// Infinity values
int INF = Integer.MAX_VALUE;
long LINF = Long.MAX_VALUE;
double DINF = Double.MAX_VALUE;

// Pair workaround (Java has no built-in Pair)
Map.Entry<Integer, Integer> pair = Map.entry(1, 2);
// Or use int[]{a, b} for lightweight pairs
```

---

## 28. Common Mistakes & Edge Cases

### Must-Check Edge Cases

| Category | Edge Cases to Test |
|---|---|
| **Array** | Empty array, single element, all same elements, already sorted, reverse sorted |
| **String** | Empty string `""`, single char, all same chars, unicode/special chars |
| **Linked List** | null head, single node, two nodes, cycle at head |
| **Tree** | null root, single node, skewed (all left or all right), perfect tree |
| **Graph** | Disconnected components, single node, self-loops, parallel edges |
| **Numbers** | 0, 1, -1, `Integer.MIN_VALUE`, `Integer.MAX_VALUE`, negative numbers |
| **Matrix** | 1×1, single row, single column, all same values |

### Top 10 Coding Mistakes

**1. Off-by-One Errors**
```java
// WRONG: ArrayIndexOutOfBoundsException
for (int i = 0; i <= arr.length; i++) // should be <

// WRONG: missing the last element
for (int i = 0; i < arr.length - 1; i++) // sometimes intentional, often a bug

// Binary search: lo <= hi vs lo < hi — know which template you're using
```

**2. Integer Overflow**
```java
// WRONG
int mid = (lo + hi) / 2;
// RIGHT
int mid = lo + (hi - lo) / 2;

// WRONG: comparator overflow
Arrays.sort(arr, (a, b) -> a - b); // fails near Integer.MIN/MAX
// RIGHT
Arrays.sort(arr, Integer::compare);
```

**3. Modifying Collection During Iteration**
```java
// WRONG: ConcurrentModificationException
for (int x : list) { if (x == 5) list.remove(x); }
// RIGHT: use iterator or collect indices first
list.removeIf(x -> x == 5);
```

**4. String Comparison with ==**
```java
// WRONG: compares references, not content
if (s == "hello")
// RIGHT
if (s.equals("hello"))
if ("hello".equals(s))   // null-safe
```

**5. Not Handling null / Empty Input**
```java
if (root == null) return ...;      // tree problems
if (head == null) return null;     // linked list problems
if (nums == null || nums.length == 0) return ...;
```

**6. Forgetting to Clone Path in Backtracking**
```java
// WRONG: all entries in result point to same list
result.add(path);
// RIGHT: create a copy
result.add(new ArrayList<>(path));
```

**7. HashMap: getOrDefault vs computeIfAbsent**
```java
// For counting: merge or getOrDefault
map.merge(key, 1, Integer::sum);
// For grouping: computeIfAbsent
map.computeIfAbsent(key, k -> new ArrayList<>()).add(value);
```

**8. Array vs ArrayList Confusion**
```java
int[] arr;         // arr.length (field)
String s;          // s.length() (method)
List<> list;       // list.size() (method)
```

**9. Priority Queue Default is Min-Heap**
```java
PriorityQueue<Integer> pq = new PriorityQueue<>(); // MIN heap
// For max heap:
PriorityQueue<Integer> maxPq = new PriorityQueue<>(Collections.reverseOrder());
```

**10. BFS: Mark Visited Before Enqueueing, Not After Dequeueing**
```java
// WRONG: may enqueue same node multiple times
queue.offer(node);
// ... later ...
if (visited[node]) continue;
visited[node] = true;

// RIGHT: mark visited when enqueueing
visited[node] = true;
queue.offer(node);
```

---

## 29. Must-Do 50 Problems — Revision Checklist

Curated list of 50 classic problems covering every pattern. Check them off as you solve them.

### Arrays & Hashing (5)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 1 | Two Sum | HashMap | Easy |
| 2 | Best Time to Buy and Sell Stock | Kadane's / Greedy | Easy |
| 3 | Product of Array Except Self | Prefix/Suffix | Medium |
| 4 | Top K Frequent Elements | Heap / Bucket Sort | Medium |
| 5 | Longest Consecutive Sequence | HashSet | Medium |

### Two Pointers & Sliding Window (5)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 6 | Valid Palindrome | Two Pointers | Easy |
| 7 | 3Sum | Sort + Two Pointers | Medium |
| 8 | Container With Most Water | Two Pointers | Medium |
| 9 | Longest Substring Without Repeating Characters | Sliding Window | Medium |
| 10 | Minimum Window Substring | Sliding Window | Hard |

### Linked Lists (4)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 11 | Reverse Linked List | Iteration / Recursion | Easy |
| 12 | Merge Two Sorted Lists | Dummy Node | Easy |
| 13 | Linked List Cycle | Fast & Slow | Easy |
| 14 | LRU Cache | HashMap + DLL | Medium |

### Stacks & Queues (3)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 15 | Valid Parentheses | Stack | Easy |
| 16 | Daily Temperatures | Monotonic Stack | Medium |
| 17 | Largest Rectangle in Histogram | Monotonic Stack | Hard |

### Binary Trees (5)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 18 | Maximum Depth of Binary Tree | DFS | Easy |
| 19 | Invert Binary Tree | DFS | Easy |
| 20 | Binary Tree Level Order Traversal | BFS | Medium |
| 21 | Lowest Common Ancestor | DFS | Medium |
| 22 | Serialize and Deserialize Binary Tree | Preorder + Queue | Hard |

### BST (2)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 23 | Validate BST | DFS with range | Medium |
| 24 | Kth Smallest Element in BST | Inorder | Medium |

### Heaps (2)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 25 | Kth Largest Element | Min-heap of size k | Medium |
| 26 | Find Median from Data Stream | Two Heaps | Hard |

### Graphs (6)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 27 | Number of Islands | DFS / BFS on grid | Medium |
| 28 | Clone Graph | DFS + HashMap | Medium |
| 29 | Course Schedule | Topological Sort | Medium |
| 30 | Rotting Oranges | Multi-Source BFS | Medium |
| 31 | Word Ladder | BFS | Hard |
| 32 | Alien Dictionary | Topological Sort | Hard |

### Tries (1)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 33 | Word Search II | Trie + Backtracking | Hard |

### Backtracking (4)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 34 | Subsets | Backtracking | Medium |
| 35 | Permutations | Backtracking | Medium |
| 36 | Combination Sum | Backtracking | Medium |
| 37 | N-Queens | Backtracking + Pruning | Hard |

### Dynamic Programming (8)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 38 | Climbing Stairs | 1D DP | Easy |
| 39 | House Robber | 1D DP | Medium |
| 40 | Coin Change | Unbounded Knapsack | Medium |
| 41 | Longest Increasing Subsequence | Binary Search + DP | Medium |
| 42 | Longest Common Subsequence | 2D DP | Medium |
| 43 | Word Break | 1D DP + HashSet | Medium |
| 44 | Partition Equal Subset Sum | 0/1 Knapsack | Medium |
| 45 | Edit Distance | 2D DP | Medium |

### Greedy (2)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 46 | Jump Game | Greedy | Medium |
| 47 | Task Scheduler | Greedy + Math | Medium |

### Intervals (1)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 48 | Merge Intervals | Sort + Merge | Medium |

### Bit Manipulation & Math (2)

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| 49 | Single Number | XOR | Easy |
| 50 | Counting Bits | DP + Bit Manipulation | Easy |

### Solve Time Guide

| Difficulty | Target Time | Max Time |
|---|---|---|
| Easy | 10-15 min | 20 min |
| Medium | 20-30 min | 40 min |
| Hard | 35-45 min | 60 min |

> **Revision strategy:** Solve each problem once. After 1-2 weeks, revisit any problem you couldn't solve within the target time. After a month, do a full pass through the checklist — you should be able to recall the approach for every problem within 2 minutes.

---

## License

This cheatsheet is free to use for learning purposes. Star the repo if you found it useful!
#   d s a c h e a t s h e e t  
 