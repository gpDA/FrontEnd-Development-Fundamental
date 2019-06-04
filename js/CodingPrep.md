#TODO:1.
### Initialize Object with frequency of appearances

```javascript
//A... array of letters
let obj = {}
for(let i = 0; i < A.length; i++){
    let splt = A[i].split('')
    for(let j = 0; j < splt.length; j++){
        obj[splt[j]] = obj[splt[j]] ? obj[splt[j]] + 1 : 1
    }
}

```

#TODO:2.
### Concat string

```javascript
const repeated = 'abc'.repeat(3)
///abcabcabc
```

ddd