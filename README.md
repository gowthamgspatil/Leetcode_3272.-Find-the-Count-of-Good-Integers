# Leetcode_3272.-Find-the-Count-of-Good-Integers

QUESTION:

![1](https://github.com/user-attachments/assets/b3ba6e1e-a0e4-4102-a5a1-91e250d8a34b)
![2](https://github.com/user-attachments/assets/5b8cc415-369b-487c-ad9a-64db0e8b0945)
![3](https://github.com/user-attachments/assets/4781d09f-621f-4e62-9662-0e331b3f6b66)

###  Imports and Class Definition
```python
from collections import Counter
```
- Imports the `Counter` class from Python's `collections` module, which helps count occurrences of each digit easily.

```python
class Solution:
```
- Defines a class `Solution` — common LeetCode-style structure.

---

###  Main Method: `countGoodIntegers`
```python
    def countGoodIntegers(self, n, k):
```
- This is the main method.
- `n` is the number of digits in the integer.
- `k` is the divisor that the palindrome must be divisible by.

---

###  Step 1: Generate All Palindromes of Length `n`
```python
        def generate_palindromes(n):
            palindromes = []
            half_len = (n + 1) // 2
            start = 10**(half_len - 1)
            end = 10**half_len
```
- `generate_palindromes(n)` is a helper function.
- We only generate half of the digits (`half_len`) and then mirror them to form the full palindrome.
- `start` and `end` are the boundaries for this half. We use `10**(half_len - 1)` to avoid leading zeros.

```python
            for half in range(start, end):
                half_str = str(half)
                if n % 2 == 0:
                    full_str = half_str + half_str[::-1]
                else:
                    full_str = half_str + half_str[:-1][::-1]
                palindromes.append(int(full_str))
            return palindromes
```
- For each possible left-half:
  - If `n` is even: mirror the whole half.
  - If `n` is odd: mirror the half *except the last digit* to avoid repeating the middle digit.
- Append the full palindrome to the list.

---

###  Step 2: Filter Valid Palindromes
```python
        palindromes = generate_palindromes(n)
        valid_digit_counts = set()
```
- Generate all palindromes of length `n`.
- `valid_digit_counts` will store digit-count "signatures" (using `Counter`) of valid palindromes.

```python
        for p in palindromes:
            if p % k == 0:
                digit_count = tuple(sorted(Counter(str(p)).items()))
                valid_digit_counts.add(digit_count)
```
- For each palindrome:
  - If it's divisible by `k`, we count its digits.
  - We store the sorted digit count as a `tuple` (so it's hashable and can go in a `set`).
  - This forms the "fingerprint" of valid palindromes.

---

###  Step 3: Backtrack Through All n-digit Numbers
```python
        digits = '0123456789'
        good_count = [0]  # mutable container to avoid nonlocal
```
- Prepare a string of digits to loop through.
- `good_count` is a list so it can be modified inside a nested function (Python quirk).

```python
        def backtrack(pos, counter):
            if pos == n:
                digit_count = tuple(sorted(counter.items()))
                if digit_count in valid_digit_counts:
                    good_count[0] += 1
                return
```
- Recursive backtracking function:
  - If we've built `n` digits, check if the digit frequency matches any valid palindrome’s frequency.
  - If yes, it's a *good* integer → increment the counter.

```python
            for d in digits:
                if pos == 0 and d == '0':
                    continue
                counter[d] += 1
                backtrack(pos + 1, counter)
                counter[d] -= 1
                if counter[d] == 0:
                    del counter[d]
```
- Try placing each digit at the current position.
- Skip leading zero (`pos == 0` and `d == '0'`).
- Recurse with updated digit count.
- After backtracking, clean up the counter (undo the change).

---

###  Step 4: Run the Backtracking and Return Result
```python
        backtrack(0, Counter())
        return good_count[0]
```
- Start the backtracking from position 0, with an empty digit counter.
- Return the total number of good integers.

---

###  Example
For `n = 3` and `k = 5`:
- Generates all 3-digit palindromes.
- Filters only those divisible by 5.
- Then finds all 3-digit numbers that can be rearranged into any of those palindromes.

CORRECT ANSWER:


![ans 1](https://github.com/user-attachments/assets/3cd0e9ce-7493-4325-8c8c-3fa8c8bbb989)
![ans 2](https://github.com/user-attachments/assets/b41bdaee-5a01-42d0-a274-40f683162460)


BUT TAKE A LESS TIME TO DO IT****

![time limit exceeded](https://github.com/user-attachments/assets/536c84a6-3085-4d43-836a-6dfbd94ab6da)




