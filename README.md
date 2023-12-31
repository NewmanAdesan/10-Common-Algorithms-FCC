# 10-Common-Algorithms-FCC
10 common algorithms we would likely encounter in real life. Lets improve our logic and programming skills. showing a clear and concise explanations of how these different algorithms work.

<br><br>

# Table of Contents
1. [Algorithm Question 3: No Repeats Please](#algorithm-question-3-no-repeats-please)
   - [Problem Description](#problem-description)
   - [Problem Breakdown](#problem-breakdown)
     - [Permutations](#permutations)
     - [Approach](#approach)
     - [Key Function](#key-function)
     - [Check for Consecutive Letters](#check-for-consecutive-letters)
   - [Problem TestCases](#problem-testcases)

2. [Algorithm Question 4: Pairwise](#algorithm-question-4-pairwise)
   - [Problem Description](#problem-description-1)
   - [Problem Breakdown](#problem-breakdown-1)
   - [Approach 1 (O(n^2))](#approach-1-on2)
     - [Implementation Steps](#implementation-steps)
   - [Approach 2 (O(n))](#approach-2-on)
     - [Implementation Steps](#implementation-steps-1)
   - [Problem TestCases](#problem-testcases-1)


<br><br>

# Algorithm Question 3: No Repeats Please

## Problem Description
Return the number of total permutations of the provided string that don't have repeated consecutive letters. Assume that all characters in the provided string are each unique.

## Problem Breakdown
### Permutations
To solve this problem, we need to generate all permutations of the given string. A permutation is an arrangement of the characters in a string. For example, the permutations of "abc" are: 'abc', 'bac', 'bca', 'acb', 'cab', 'cba'.
<br>
This means 1 x 2 x 3 = 6 permutations <br>
"c" has only 1 slot <br>
"b" has choice of 2 slot <br>
"a" has choice of 3 slot <br>

```javascript
"c"      |         "b"       |       "a"
____________________________________________
         |                   |
"c"      |         "bc"      |       "abc"
         |                   |       "bac"
         |                   |       "bca"
         |         "cb"      |       "acb"
         |                   |       "cab"
         |                   |       "cba"
```
<br>

```javascript
permutation([""], "c")          // ["c"]
permutation(["c"], "b")         // ["bc", "cb"]
permutation(["bc", "cb"], "a")  //[ 'abc', 'bac', 'bca', 'acb', 'cab', 'cba' ]
```


### Approach
1. We will generate all possible permutations of the input string.
2. We will filter out permutations that have consecutive repeated letters.

### Key Function
The key to our solution is the `getPermutation` function. It generates all permutations recursively by taking the first character and inserting it at different positions in the permutations of the rest of the string.

```javascript
function getPermutation(str) {
  if (str.length == 0) return [""];
  
  let new_char = str[0];
  let current_permutation = getPermutation(str.slice(1));
  
  let new_permutation = []
  for (let perm of current_permutation){
    for (let i=0; i<=perm.length; i++){
      let new_perm = perm.slice(0, i) + new_char + perm.slice(i)
      new_permutation.push(new_perm);
    }
  }

  return new_permutation;
}
```

### Check for Consecutive Letters
We use the `hasConsecutiveLetters` function to check if a permutation has consecutive repeated letters.
```javascript
function hasConsecutiveLetters(str) {
  for (let i=0; i<str.length-1; i++){
      if (str[i] == str[i+1]) return true;
  }
  return false
}
```

## Problem TestCases
Here are some example calls to the permAlone function with different input strings:
```javascript
permAlone("aab")    // Returns 2
permAlone("aaa")    // Returns 0
permAlone("aabb")   // Returns 8
permAlone("abcdefa") // Returns 3600
permAlone("abfdefa") // Returns 2640
permAlone("zzzzzzzz") // Returns 0
permAlone("a")      // Returns 1
permAlone("aaab")   // Returns 0
permAlone("aaabb")  // Returns 12
```


<br/><br/><br />


# Algorithm Question 4: Pairwise

## Problem Description
Given an array `arr`, find element pairs whose sum equals the second argument `arg` and return the sum of their indices. You may use multiple pairs that have the same numeric elements but different indices. Once an element has been used, it cannot be reused to pair with another element. Each pair should use the lowest possible available indices.

## Problem Breakdown
Following the question, we have four major tasks:
1. Find a pair (a, b)
2. Validate the pair
3. Utilize the pair
4. Eliminate the pair

## Approach 1 (O(n^2))
We loop through each item in the array, taking it as one of the pairs (`a`). Then, we loop through each item in the array again to find the other item of the pair (`b`). We check if the pair is valid and eliminate it by marking `a` & `b` as null in the array.
<br>
A valid pair passes this test:
  - `a` has not been paired with another
  - `b` has not been paired with another
  - `a` has a different index from `b`
  - `a + b = arg`

### Implementation Steps
1. **Initialization:**
   - Initialize a variable `sum` to keep track of the total sum of indices: 
   ```javascript
      let sum = 0;
   ```

2. **Pair Search - First Loop:**
   - Loop through the array to find the first element of a pair, which we'll call `a`.
   
   ```javascript
      for (let aIndex = 0; aIndex < arr.length; aIndex++) {
        ...
      }
   ```

3. **Pair Validation - a:**
   - Ensure that `a` is not already part of a pair (`a != null`).
   ```javascript 
      let a = arr[aIndex];
      if (a === null) continue;
   ```

4. **Pair Search - Second Loop:**
   - Start a nested loop to find the second element of a pair, which we'll call `b`.
   ```javascript             
      for (let bIndex = 0; bIndex < arr.length; bIndex++) {
        ...
      }
   ```

5. **Pair Validation - b:**
   - Validate the pair (a, b) by checking:
        - a and b are not the same value (aIndex !== bIndex).
        - b is not already in a pair (arr[bIndex] != null).
        - The sum of a and b equals the target value arg.
   ```javascript

      let b = arr[bIndex];
      if (aIndex !== bIndex && b !== null && a + b === arg) {
        ...
      }
   ```

6. **Utilize Valid Pair:**
   - If a valid pair is found, add the indices of `a` and `b` to the `sum` variable.
   ```javascript
      sum += aIndex + bIndex;
   ```

7. **Pair Elimination:**
   - Eliminate the valid pair by setting their corresponding elements in the array to null.
   ```javascript
      arr[aIndex] = null;
      arr[bIndex] = null;
   ```

8. **Next Pair:**
   - After a valid pair is found, utilized & eliminated, we no longer continue the second loop.
   ```javascript
      break;
   ```
   
9. **Return the Accumulated Sum:**
   - Return the `sum`.
   ```javascript
      return sum;
   ```
   

## Approach 2 (O(n))

1. **Selecting Pairs:**
   - We take an element from the `arr`, let's call it `a`.
   - We find its complement, let's call it `b`, such that `b = arg - a`.
   - We now have a pair `(a, b)`.

2. **Validating Pairs:**
   - To determine if the pair is valid, we check if `b` is in `arr`.
   - It's crucial to ensure that `b` is within the array `arr`.
   - There might be duplicate instances of `b` in the array with the same numeric value but different indices.
   - Our goal is to select the smallest `b` based on its position in the array.
   - Additionally, we need to confirm that the chosen `b` has not been previously paired with another element.
   - Keep in mind that the search for `b` considers that `a` is effectively removed from the array. If `a` and `b` have the same numeric value, a valid pair should have distinct indices.

3. **Optimizing Pair Search with a New Data Structure:**
   - To optimize the search for the complement `b` and achieve O(1) complexity, we create a new data structure `arrNew` from `arr`.
   - `arrNew` is an object where keys represent unique values in the array, and each key has associated information.
   - Example:
     ```javascript
     arr = [0, 0, 0, 0, 1, 1]
     arrNew = { 
       '0': {
         indices: [0, 1, 2, 3],
         pointer: 0
       }, 
       '1': {
         indices: [4, 5],
         pointer: 0 
       }
     }
     ```
   - In this example, '0' and '1' are keys representing unique values in the array.
   - `indices: [0, 1, 2, 3]` indicates the indices of occurrences for each value ('0' or '1').
   - `pointer: 0` is a pointer to the index that is free to use. If the pointer increases to 1, that means the index 0 has been used in a pair, and the next available version is at index 1.


### Implementation Steps
1. **Initialization:**
   - Initialize a variable `sum` to 0: 
   ```javascript
      let sum = 0;
   ```
   
2. **Object a New Datastructure:**
   - Create `arrNew` from `arr` to efficiently track indices.
   ```javascript
      let arrNew = {};
      arr.forEach((item, index)=>{
        if (arrNew[item]) arrNew[item].indices.push(index);
        else arrNew[item] = {indices: [index], pointer: 0]};
      })
   ```
   
3. **Inspect each element in the Initial Data Structure:**
   - Iterate through each element `a` in the array `arr`.
   ```javascript
      for (let a of arr){
          ...
      }
   ```
   
4. **Compute the Complement b:**
   - Compute `b = arg - a`.

5. **Retrieve `b` Information & Retrieve `a` Information:**
   - If `b` is a key in `arrNew`, get the indices of `b` and its pointer & get the indices of `a` and its pointer.
   ```javascript
      if (arrNew[b]){
        const bIndices = arrNew[b].indices;
        let bPointer = arrNew[b].pointer;
        const aIndices = arrNew[a].indices;
        let aPointer = arrNew[a].pointer;

        ...
      }
   ```
   
6. **Handle Edge Case:**
   - If `b` and `a` have the same numeric value, adjust the pointer for `b` to avoid using the same index.
   ```javascript
      if (b === a) bPointer = bPointer + 1;
   ```
   
7. **Check Availability:**
   - Ensure there are available indices for both `a` and `b`.
   ```javascript
      if (aPointer < aIndices.length && bPointer < bIndices.length){
        ...
      }
   ```
   
8. **Pairing and Update:**
   - If available, add both indices to the global `sum` variable and update pointers for both `a` and `b`.
   ```javascript
      sum += aIndices[aPointer] + bIndices[bPointer];
      arrNew[a].pointer += 1;
      arrNew[b].pointer += 1;
   ```
   

## Problem TestCases
Here are some example calls to the pairwise function with different input arrays:
```javascript
pairwise([1, 1, 2], 3);  // Returns 1
pairwise([1, 3, 2, 4], 4);  // Returns 1

```


<br/><br/><br />



