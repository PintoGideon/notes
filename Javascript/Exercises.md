```javascript
function fizzbuzz() {
  let counter = 1;
  while (counter <= 100) {
    if (counter % 3 == 0 && counter % 5 == 0) {
      console.log("fizzbuzz");
    } else if (counter % 3 == 0) {
      console.log("fizz");
    } else if (counter % 5 == 0) {
      console.log("buzz");
    } else console.log(counter);
    counter += 1;
  }
}
```

```javascript
function findSuffix(suffix,stringArray){

let result;

searchBlock:{
	for(const str of stringArray){
		if(str.endsWith(suffix)){
			result=str;
			break search_block;
		}
	} // for
	// Failure
	result='(untitled)'
} // search_block
return {suffix,result}

}

assert.deepEqual(findSuffix([
	'foo.txt','bar.html'
]))








}





```
