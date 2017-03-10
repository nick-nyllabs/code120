

```javascript
function Dog(breed, name){
  this.breed = breed;
  this.name = name;
}
Dog.prototype.getName = function(){
  return this.name;
};

var lab = new Dog("Lab", "Pepper");
var getName = lab.getName;

console.log( getName() );
```
