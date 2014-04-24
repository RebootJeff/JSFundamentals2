# Review: The 4 Class Patterns

Compare them based on:
- Code complexity/simplicity
- Memory efficiency
- Popularity/convention

# Review? Prototype Chains

## Prototypes w/ Raw Objects

```JavaScript
var x = {};
x.greeting = 'Hello';

var y = Object.create(x);
y.farewell = 'Bye';

console.log(y.greeting);
console.log(x.farewell);

console.log(y.__proto__); // Why not `y.prototype`?

// Pop quiz: What's the prototype of a function?
var User = function(name){
  this.name = name;
};

console.log(User.prototype); // Why not `__proto__`?
```

# Continuing OOP: Subclasses

There are 3 steps to writing a pseudoclassical subclass.
1. Call superclass constructor using context of subclass constructor.
2. Set subclass prototype to object that delegates to superclass prototype.
3. Reassign subclass prototype constructor.

```JavaScript
var User = function(name){
  console.log('User constructor invoked');
  this.name = name;
};

User.prototype.shoutName = function(){
  console.log(this.name + '!');
};

var Admin = function(name){
  console.log('Admin constructor invoked');
  User.call(this, name);
};

Admin.prototype = Object.create(User.prototype); // what if we forego `.prototype`?
Admin.prototype.constructor = Admin; // not very useful; just a best practice

Admin.prototype.ban = function(user){
  console.log('I HEREBY BAN...', user.name);
};

var dude = new User('Dude');
var me = new Admin('Jeff');

console.log('`me` is an admin:', me instanceof Admin);
console.log('`me` is a user:', me instanceof User);

me.ban(dude);
```

## Considerations

Note that because the subclass inherits via prototype chain, you could add methods to the superclass whenever and then the subclass will get them too.

```JavaScript
function changeThingsUp(){
  User.prototype.farewell = function(){
    console.log('Bye bye!');
  };

  me.farewell();
}

changeThingsUp(); // Let's get dynamic
```

Note that inheritance is NOT always ideal.
Consider this video game example: Car superclass, SportsCar subclass, Hybrid subclass. What if I wanted a car that is both a sports car and a hybrid? The code to make it is in two different subclasses.
Possible solution: alternatives to inheritance such as mixins and decorators.
