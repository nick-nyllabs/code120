
### What is logged on the last line?
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

console.log( getName() ); // => ???
```

<details><summary>Answer</summary>
**Answer**: `undefined`.

**Expanation**: As we know, when a method (such as `lab.getName` is invoked, `getName` is first checked on the actual instance. If not found, it moves up the prototypechain until it arrives at `Dog.prototype.getName`, and would then invoke the method at that object, preserving `this` as the instance `lab`. However, when `lab.getName` is assigned to a variable, this binding will rely on the call site of where the variable was invoked. In the next line, the call site is the global `window` object, which, assuming this is the only code running and that `name` does not natively exist globally, is `undefined`.
</details>

### Do the last two functions return the same object?
```javascript
var obj = {
  name: "parent",
  children: []
};
obj.children.push({
  name: "child",
  children: []
});
obj.children[0].children.push({
  name: "grandchild",
  children:[],
  getThis: function() { return this; }
});

var getThisVar = obj.children[0].children[0].getThis;

getThisVar();  // equal to below?
obj.children[0].children[0].getThis(); // equal to above?
```
