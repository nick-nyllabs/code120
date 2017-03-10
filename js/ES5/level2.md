
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
<details><summary>Answer</summary>
Not Equal.

**Explanation**: `getThisVar` looks up `getThis` correctly through the chain of children, but invokation happens at `getThisVar` in the global context, not the context where the function was originally taken from. `this` gets bound to the call site (global scope) and returns `window`. The second invokation, on the other hand, invokes at the actual grandchild level. The call site is still the global scope, _but_ the function is explicitly called from the object. Implicit binding is used, and the "owner" of the `getThis` property, resolving to the function, is the object with `name` equeal to `"grandchild"`.
</details>
