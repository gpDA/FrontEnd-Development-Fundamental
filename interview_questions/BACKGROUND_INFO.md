
#TODO:1.
### Heapify and Heap Sort

```javascript
//  heapify
// [0,1,2,3,4] INDEX
// [3,9,1,4,7]

// BUBBLING DOWN
// MAX HEAP
// From the back index
// for the region...

// across the region...

function heapsort(arr){
  heapify(arr);
  // max heap
  // console.log(arr);
  let wall =arr.length;
  while(wall > 0){
    [arr[0], arr[wall-1]] = [arr[wall-1], arr[0]];
    wall--;
    bubbleDown(arr, 0, wall);
  }
  return arr;
}

function heapify(arr){
  for(let i = arr.length -1; i> -1; i--){
    bubbleDown(arr,i,arr.length);
  }  
}

function bubbleDown(arr, parentIndex, wall){
  let childIndex = getChildIndex(arr, parentIndex, wall);

  while(childIndex < wall && arr[parentIndex] < arr[childIndex]){
    [arr[parentIndex] , arr[childIndex]] = [arr[childIndex] , arr[parentIndex]];
    parentIndex = childIndex;
    childIndex = getChildIndex(arr, parentIndex, wall);
  }
}

// child1: 2n+1 / child2: 2n+2
function getChildIndex(arr, parentIndex, wall){
  let childIndex1 = 2 * parentIndex + 1;
  let childIndex2 = 2 * parentIndex + 2;

  // ??? why return childIndex even it is `out of index`
  if(childIndex1 >= wall){
    return childIndex1;

  }else if(childIndex2 >= wall){
    return childIndex1;
  }else if(arr[childIndex1] > arr[childIndex2]){
    return childIndex1;
  }else{
    return childIndex2;
  }
}

// WHY HEAPSORT
// RUNTIME : O(n log n)

// heapify : O(n)
  // 

// convert maxheap to sorted array O(n log n)


// WORST CASE
  // MERGESORT : O(n log(n))
  // QUICKSORT : O(n^2)
  // HEAPSORT : O(n log(n))
// AVERAGE CASE
  // MERGESORT : O(n log(n)) 
  // QUICKSORT : O(n log(n))
  // HEAPSORT : O(n log(n))

// HEAPSORT over MERGESORT
// - MERGESORT uses extra space
// while HEAPSORT does not (it is in-place)
```

#TODO:2.
### Create an object with values as a key and key as a list of values
```javascript
/**
 * Given a source object, return a new object that has a property for each unique key in the source object, and an array of values
 * corresponding to those keys in the source object.
 */

function grouper(obj) {
  const keys_ = Object.keys(obj)
  let res = {}
  for(let i = 0; i < keys_.length ;i++){
    let key = keys_[i]
    // console.log(key)
    // shorthair
    // bengal
    // husky
    // ...
    
    let vals = obj[key]
    // console.log(vals)
    // cat
    // cat
    // dog
    // cat
    // ...
    
    if (vals in res){
    res[vals].push(key)
    }else{
    res[vals] = [key]
    }
  }
  return res
 }
 
 var source = {
   'shorthair': 'cat', 
   'bengal': 'cat',
   'husky': 'dog',
   'siamese': 'cat',
   'bulldog': 'dog',
   'beagle': 'dog'
 }
 
 console.log(grouper(source))
 // expect(grouper(source)).to.deep.equal({
 //   { dog: ['husky', 'bulldog', 'beagle'], cat: ['shorthair', 'bengal', 'siamese'] }
 // })
 ```