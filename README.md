# 10-Common-Algorithms-FCC


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
