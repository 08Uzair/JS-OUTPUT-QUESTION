# JavaScript Output Questions – Complete Interview Prep

> A structured collection of output-based questions for JavaScript interviews.  
> **How to use:** Try predicting the output first, then check the answer.

---

## Table of Contents

1. [Promises](#1-promises)
2. [Event Loop](#2-event-loop)
3. [this Keyword](#3-this-keyword)
4. [Loops & State](#4-loops--state)
5. [Hoisting](#5-hoisting)
6. [Closures & Scope](#6-closures--scope)
7. [Array Methods](#7-array-methods)

---

## 1. Promises

### Q1 – Basic Promise Execution Order

```js
console.log("Start");

const promise = new Promise((resolve, reject) => {
  console.log("Inside Promise");
  resolve("Resolved");
});

promise.then(res => console.log(res));

console.log("End");
```

**Output:**
```
Start
Inside Promise
End
Resolved
```

---

### Q2 – Promise vs setTimeout

```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("End");
```

**Output:**
```
Start
End
Promise
Timeout
```

---

### Q3 – Multiple `.then()` Chaining

```js
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => {
    console.log(x);
    return x * 2;
  })
  .then(x => console.log(x));
```

**Output:**
```
2
4
```

---

### Q4 – Missing `return` in `.then()`

```js
Promise.resolve(1)
  .then(x => {
    x + 1;
  })
  .then(x => console.log(x));
```

**Output:**
```
undefined
```

> ⚠️ No `return` statement means the next `.then()` receives `undefined`.

---

### Q5 – Promise Chaining with setTimeout

```js
Promise.resolve()
  .then(() => {
    console.log("First");
    setTimeout(() => console.log("Timeout"), 0);
  })
  .then(() => console.log("Second"));
```

**Output:**
```
First
Second
Timeout
```

---

### Q6 – Error Handling with `.catch()`

```js
Promise.reject("Error")
  .then(() => console.log("Then"))
  .catch(err => console.log(err));
```

**Output:**
```
Error
```

---

### Q7 – Error Propagation

```js
Promise.resolve()
  .then(() => {
    throw new Error("Oops");
  })
  .then(() => console.log("Then"))
  .catch(err => console.log(err.message));
```

**Output:**
```
Oops
```

---

### Q8 – `finally()` Behavior

```js
Promise.resolve("Success")
  .finally(() => console.log("Finally"))
  .then(res => console.log(res));
```

**Output:**
```
Finally
Success
```

---

### Q9 – `finally()` Does NOT Change Value

```js
Promise.resolve("A")
  .finally(() => "B")
  .then(res => console.log(res));
```

**Output:**
```
A
```

> ⚠️ `finally()` never changes the resolved value.

---

### Q10 – `Promise.all()` Success

```js
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3)
]).then(res => console.log(res));
```

**Output:**
```
[1, 2, 3]
```

---

### Q11 – `Promise.all()` with Rejection

```js
Promise.all([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
])
.then(res => console.log(res))
.catch(err => console.log(err));
```

**Output:**
```
Error
```

---

### Q12 – `Promise.race()`

```js
Promise.race([
  new Promise(res => setTimeout(() => res("First"), 100)),
  new Promise(res => setTimeout(() => res("Second"), 50))
]).then(res => console.log(res));
```

**Output:**
```
Second
```

---

### Q13 – `Promise.resolve` Inside `.then()`

```js
Promise.resolve()
  .then(() => Promise.resolve("Hello"))
  .then(res => console.log(res));
```

**Output:**
```
Hello
```

---

### Q14 – Microtask Queue Priority

```js
setTimeout(() => console.log("Timeout"), 0);

Promise.resolve()
  .then(() => console.log("Promise 1"))
  .then(() => console.log("Promise 2"));

console.log("End");
```

**Output:**
```
End
Promise 1
Promise 2
Timeout
```

---

### Q15 – Nested Promises

```js
Promise.resolve()
  .then(() => {
    return new Promise(res => {
      console.log("Inner");
      res();
    });
  })
  .then(() => console.log("Outer"));
```

**Output:**
```
Inner
Outer
```

---

### Q16 – Async/Await with Promise

```js
async function test() {
  console.log("Start");
  await Promise.resolve();
  console.log("After Await");
}

test();
console.log("End");
```

**Output:**
```
Start
End
After Await
```

---

### Q17 – Await with setTimeout

```js
async function test() {
  await new Promise(res => setTimeout(res, 0));
  console.log("After Timeout");
}

test();
console.log("End");
```

**Output:**
```
End
After Timeout
```

---

### Q18 – Promise Inside setTimeout

```js
setTimeout(() => {
  console.log("Timeout");
  Promise.resolve().then(() => console.log("Promise"));
}, 0);
```

**Output:**
```
Timeout
Promise
```

---

### Q19 – Multiple Resolved Promises

```js
Promise.resolve().then(() => console.log(1));
Promise.resolve().then(() => console.log(2));
Promise.resolve().then(() => console.log(3));
```

**Output:**
```
1
2
3
```

---

### Q20 – Tricky Execution Order

```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
  setTimeout(() => console.log("Timeout 2"), 0);
});

Promise.resolve().then(() => console.log("Promise 2"));

console.log("End");
```

**Output:**
```
Start
End
Promise 1
Promise 2
Timeout
Timeout 2
```

---

### Q21 – Promise Chain Return Value

```js
Promise.resolve(5)
  .then(x => x * 2)
  .then(x => x + 3)
  .then(console.log);
```

**Output:**
```
13
```

---

### Q22 – Catch Recovers Chain

```js
Promise.reject("fail")
  .catch(err => "recovered")
  .then(res => console.log(res));
```

**Output:**
```
recovered
```

---

### Q23 – `Promise.allSettled()`

```js
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("err"),
  Promise.resolve(3)
]).then(res => console.log(res.map(r => r.status)));
```

**Output:**
```
["fulfilled", "rejected", "fulfilled"]
```

---

### Q24 – Async Function Always Returns Promise

```js
async function test() {
  return 42;
}

test().then(console.log);
```

**Output:**
```
42
```

---

### Q25 – Await Pauses Only Inside Async

```js
async function test() {
  const val = await Promise.resolve(10);
  console.log(val);
}

test();
console.log("Outside");
```

**Output:**
```
Outside
10
```

---

### Q26 – Error in Async/Await

```js
async function test() {
  try {
    await Promise.reject("Error");
  } catch (e) {
    console.log(e);
  }
}

test();
```

**Output:**
```
Error
```

---

### Q27 – Multiple Awaits

```js
async function test() {
  const a = await Promise.resolve(1);
  const b = await Promise.resolve(2);
  console.log(a + b);
}

test();
```

**Output:**
```
3
```

---

### Q28 – `Promise.any()` – First Fulfilled

```js
Promise.any([
  Promise.reject("err1"),
  Promise.resolve("ok"),
  Promise.resolve("ok2")
]).then(console.log);
```

**Output:**
```
ok
```

---

### Q29 – Chained Catch

```js
Promise.resolve()
  .then(() => { throw "A"; })
  .catch(e => { console.log(e); throw "B"; })
  .catch(e => console.log(e));
```

**Output:**
```
A
B
```

---

### Q30 – Finally Before Catch

```js
Promise.reject("error")
  .finally(() => console.log("finally"))
  .catch(e => console.log(e));
```

**Output:**
```
finally
error
```

---

## 2. Event Loop

### Q1 – Basic Execution Order

```js
console.log("A");
console.log("B");
console.log("C");
```

**Output:**
```
A
B
C
```

---

### Q2 – setTimeout Basic

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
```

**Output:**
```
Start
End
Timeout
```

---

### Q3 – Promise vs setTimeout

```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("End");
```

**Output:**
```
Start
End
Promise
Timeout
```

---

### Q4 – Multiple Promises

```js
Promise.resolve().then(() => console.log("P1"));
Promise.resolve().then(() => console.log("P2"));

console.log("End");
```

**Output:**
```
End
P1
P2
```

---

### Q5 – Nested setTimeout

```js
setTimeout(() => {
  console.log("Outer");

  setTimeout(() => {
    console.log("Inner");
  }, 0);

}, 0);
```

**Output:**
```
Outer
Inner
```

---

### Q6 – Promise Inside setTimeout

```js
setTimeout(() => {
  console.log("Timeout");

  Promise.resolve().then(() => console.log("Promise"));
}, 0);
```

**Output:**
```
Timeout
Promise
```

---

### Q7 – setTimeout Inside Promise

```js
Promise.resolve().then(() => {
  console.log("Promise");

  setTimeout(() => console.log("Timeout"), 0);
});
```

**Output:**
```
Promise
Timeout
```

---

### Q8 – Microtask Queue Priority

```js
setTimeout(() => console.log("T1"), 0);

Promise.resolve().then(() => console.log("P1"));

Promise.resolve().then(() => console.log("P2"));

setTimeout(() => console.log("T2"), 0);
```

**Output:**
```
P1
P2
T1
T2
```

---

### Q9 – Mixed Execution

```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
}).then(() => {
  console.log("Promise 2");
});

console.log("End");
```

**Output:**
```
Start
End
Promise 1
Promise 2
Timeout
```

---

### Q10 – Nested Promises

```js
Promise.resolve().then(() => {
  console.log("P1");

  Promise.resolve().then(() => console.log("P2"));
});
```

**Output:**
```
P1
P2
```

---

### Q11 – Complex Order

```js
console.log("Start");

setTimeout(() => console.log("T1"), 0);

Promise.resolve().then(() => {
  console.log("P1");
  setTimeout(() => console.log("T2"), 0);
});

Promise.resolve().then(() => console.log("P2"));

console.log("End");
```

**Output:**
```
Start
End
P1
P2
T1
T2
```

---

### Q12 – Async/Await Basics

```js
async function test() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
}

test();
console.log("C");
```

**Output:**
```
A
C
B
```

---

### Q13 – Async with setTimeout

```js
async function test() {
  await new Promise(res => setTimeout(res, 0));
  console.log("After");
}

test();
console.log("End");
```

**Output:**
```
End
After
```

---

### Q14 – Await + Multiple Promises

```js
async function test() {
  console.log("Start");

  await Promise.resolve();

  console.log("Middle");

  await Promise.resolve();

  console.log("End");
}

test();
console.log("Outside");
```

**Output:**
```
Start
Outside
Middle
End
```

---

### Q15 – Tricky Order

```js
console.log("Start");

setTimeout(() => console.log("T1"), 0);

Promise.resolve().then(() => {
  console.log("P1");
}).then(() => {
  console.log("P2");
});

setTimeout(() => console.log("T2"), 0);

console.log("End");
```

**Output:**
```
Start
End
P1
P2
T1
T2
```

---

### Q16 – Multiple Layers

```js
setTimeout(() => {
  console.log("T1");

  Promise.resolve().then(() => console.log("P1"));
}, 0);

Promise.resolve().then(() => {
  console.log("P2");
});
```

**Output:**
```
P2
T1
P1
```

---

### Q17 – Queue Mixing

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

**Output:**
```
A
D
C
B
```

---

### Q18 – Nested Microtasks

```js
Promise.resolve().then(() => {
  console.log("P1");

  Promise.resolve().then(() => {
    console.log("P2");
  });
});
```

**Output:**
```
P1
P2
```

---

### Q19 – Deep Nesting

```js
setTimeout(() => {
  console.log("T1");

  Promise.resolve().then(() => {
    console.log("P1");

    setTimeout(() => console.log("T2"), 0);
  });
}, 0);
```

**Output:**
```
T1
P1
T2
```

---

### Q20 – Ultimate Tricky One

```js
console.log("Start");

setTimeout(() => console.log("T1"), 0);

Promise.resolve().then(() => {
  console.log("P1");

  setTimeout(() => console.log("T2"), 0);

  Promise.resolve().then(() => console.log("P2"));
});

console.log("End");
```

**Output:**
```
Start
End
P1
P2
T1
T2
```

---

### Q21 – setInterval vs Promise

```js
let i = 0;
const id = setInterval(() => {
  console.log("interval", i++);
  if (i === 2) clearInterval(id);
}, 0);

Promise.resolve().then(() => console.log("promise"));
```

**Output:**
```
promise
interval 0
interval 1
```

---

### Q22 – Three setTimeout + One Promise

```js
setTimeout(() => console.log("T1"), 0);
setTimeout(() => console.log("T2"), 0);
setTimeout(() => console.log("T3"), 0);
Promise.resolve().then(() => console.log("P1"));
```

**Output:**
```
P1
T1
T2
T3
```

---

### Q23 – Microtask from Macrotask

```js
setTimeout(() => {
  Promise.resolve().then(() => console.log("Inner P"));
  console.log("Outer T");
}, 0);
```

**Output:**
```
Outer T
Inner P
```

---

### Q24 – Async/Await in Loop

```js
async function test() {
  for (let i = 0; i < 3; i++) {
    await Promise.resolve();
    console.log(i);
  }
}

test();
console.log("Outside");
```

**Output:**
```
Outside
0
1
2
```

---

### Q25 – Nested Async Functions

```js
async function inner() {
  await Promise.resolve();
  console.log("inner");
}

async function outer() {
  console.log("outer start");
  await inner();
  console.log("outer end");
}

outer();
console.log("global");
```

**Output:**
```
outer start
global
inner
outer end
```

---

### Q26 – setTimeout 0 vs 100

```js
setTimeout(() => console.log("T0"), 0);
setTimeout(() => console.log("T100"), 100);
Promise.resolve().then(() => console.log("P"));
console.log("Sync");
```

**Output:**
```
Sync
P
T0
T100
```

---

### Q27 – Promise Chain Then Timeout

```js
Promise.resolve()
  .then(() => {
    setTimeout(() => console.log("T"), 0);
    return "P";
  })
  .then(console.log);
```

**Output:**
```
P
T
```

---

### Q28 – queueMicrotask

```js
queueMicrotask(() => console.log("microtask"));
setTimeout(() => console.log("timeout"), 0);
console.log("sync");
```

**Output:**
```
sync
microtask
timeout
```

---

### Q29 – Async IIFE

```js
(async () => {
  console.log("A");
  await null;
  console.log("B");
})();

console.log("C");
```

**Output:**
```
A
C
B
```

---

### Q30 – Full Mix

```js
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => {
  console.log("3");
  setTimeout(() => console.log("4"), 0);
});
queueMicrotask(() => console.log("5"));
console.log("6");
```

**Output:**
```
1
6
3
5
2
4
```

---

## 3. `this` Keyword

### Q1 – Global Context

```js
console.log(this);
```

**Output:**
```
Browser → window
Node.js → {}
```

---

### Q2 – Simple Function

```js
function test() {
  console.log(this);
}

test();
```

**Output:**
```
Browser → window
Strict mode → undefined
```

---

### Q3 – Method in Object

```js
const obj = {
  name: "Uzair",
  getName: function () {
    console.log(this.name);
  }
};

obj.getName();
```

**Output:**
```
Uzair
```

---

### Q4 – Method Detached

```js
const obj = {
  name: "Uzair",
  getName: function () {
    console.log(this.name);
  }
};

const fn = obj.getName;
fn();
```

**Output:**
```
undefined
```

---

### Q5 – Arrow Function in Object

```js
const obj = {
  name: "Uzair",
  getName: () => {
    console.log(this.name);
  }
};

obj.getName();
```

**Output:**
```
undefined
```

> ⚠️ Arrow functions inherit `this` from outer (lexical) scope.

---

### Q6 – Arrow Inside Method

```js
const obj = {
  name: "Uzair",
  getName: function () {
    const inner = () => {
      console.log(this.name);
    };
    inner();
  }
};

obj.getName();
```

**Output:**
```
Uzair
```

---

### Q7 – setTimeout with Function

```js
const obj = {
  name: "Uzair",
  getName: function () {
    setTimeout(function () {
      console.log(this.name);
    }, 0);
  }
};

obj.getName();
```

**Output:**
```
undefined
```

---

### Q8 – setTimeout with Arrow

```js
const obj = {
  name: "Uzair",
  getName: function () {
    setTimeout(() => {
      console.log(this.name);
    }, 0);
  }
};

obj.getName();
```

**Output:**
```
Uzair
```

---

### Q9 – `call()`

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Uzair" };

greet.call(user);
```

**Output:**
```
Uzair
```

---

### Q10 – `apply()`

```js
function greet(age) {
  console.log(this.name, age);
}

const user = { name: "Uzair" };

greet.apply(user, [21]);
```

**Output:**
```
Uzair 21
```

---

### Q11 – `bind()`

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Uzair" };

const fn = greet.bind(user);
fn();
```

**Output:**
```
Uzair
```

---

### Q12 – `bind` vs `call`

```js
const obj = {
  name: "Uzair",
  getName: function () {
    console.log(this.name);
  }
};

const fn = obj.getName.bind({ name: "Ali" });
fn();
```

**Output:**
```
Ali
```

---

### Q13 – Nested Object

```js
const obj = {
  name: "Uzair",
  inner: {
    name: "Ali",
    getName: function () {
      console.log(this.name);
    }
  }
};

obj.inner.getName();
```

**Output:**
```
Ali
```

---

### Q14 – Arrow in Nested Object

```js
const obj = {
  name: "Uzair",
  inner: {
    name: "Ali",
    getName: () => {
      console.log(this.name);
    }
  }
};

obj.inner.getName();
```

**Output:**
```
undefined
```

---

### Q15 – Constructor Function

```js
function User(name) {
  this.name = name;
}

const u1 = new User("Uzair");
console.log(u1.name);
```

**Output:**
```
Uzair
```

---

### Q16 – Without `new`

```js
function User(name) {
  this.name = name;
}

User("Uzair");
console.log(this.name);
```

**Output:**
```
Browser → "Uzair" (pollutes global)
Strict mode → TypeError
```

---

### Q17 – Class Method

```js
class User {
  constructor(name) {
    this.name = name;
  }

  getName() {
    console.log(this.name);
  }
}

const u = new User("Uzair");
u.getName();
```

**Output:**
```
Uzair
```

---

### Q18 – Method Extraction from Class

```js
class User {
  constructor(name) {
    this.name = name;
  }

  getName() {
    console.log(this.name);
  }
}

const u = new User("Uzair");
const fn = u.getName;
fn();
```

**Output:**
```
undefined
```

---

### Q19 – Implicit Binding Loss

```js
const obj = {
  name: "Uzair",
  getName: function () {
    console.log(this.name);
  }
};

setTimeout(obj.getName, 0);
```

**Output:**
```
undefined
```

---

### Q20 – Chained Object with Arrow

```js
const obj = {
  name: "Uzair",
  getName() {
    return {
      name: "Ali",
      print: () => console.log(this.name)
    };
  }
};

obj.getName().print();
```

**Output:**
```
Uzair
```

---

### Q21 – `this` in Callback

```js
const obj = {
  val: 42,
  getVal: function () {
    [1].forEach(function () {
      console.log(this.val);
    });
  }
};

obj.getVal();
```

**Output:**
```
undefined
```

---

### Q22 – `this` in Callback with Arrow

```js
const obj = {
  val: 42,
  getVal: function () {
    [1].forEach(() => {
      console.log(this.val);
    });
  }
};

obj.getVal();
```

**Output:**
```
42
```

---

### Q23 – Double Bind

```js
function test() {
  console.log(this.x);
}

const a = test.bind({ x: 1 });
const b = a.bind({ x: 2 });
b();
```

**Output:**
```
1
```

> ⚠️ Once bound, `this` cannot be re-bound.

---

### Q24 – `call` with null

```js
function test() {
  console.log(this);
}

test.call(null);
```

**Output:**
```
Browser → window
Strict mode → null
```

---

### Q25 – Prototype Method

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(this.name);
};

const a = new Animal("Dog");
a.speak();
```

**Output:**
```
Dog
```

---

### Q26 – Prototype Method Detached

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(this.name);
};

const a = new Animal("Dog");
const fn = a.speak;
fn();
```

**Output:**
```
undefined
```

---

### Q27 – Class Arrow Method

```js
class Counter {
  constructor() {
    this.count = 0;
    this.inc = () => {
      this.count++;
      console.log(this.count);
    };
  }
}

const c = new Counter();
const fn = c.inc;
fn();
```

**Output:**
```
1
```

---

### Q28 – Getter with `this`

```js
const obj = {
  _name: "Uzair",
  get name() {
    return this._name;
  }
};

console.log(obj.name);
```

**Output:**
```
Uzair
```

---

### Q29 – Method Shorthand

```js
const obj = {
  x: 10,
  getX() {
    return this.x;
  }
};

console.log(obj.getX());
```

**Output:**
```
10
```

---

### Q30 – `this` with Destructuring

```js
const obj = {
  name: "Uzair",
  getName() {
    console.log(this.name);
  }
};

const { getName } = obj;
getName();
```

**Output:**
```
undefined
```

> ⚠️ Destructuring loses the `this` binding.

---

## 4. Loops & State

### Q1 – Basic `for` Loop

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
}
```

**Output:**
```
0
1
2
```

---

### Q2 – `var` vs `let` (Sync)

```js
for (var i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i);
```

**Output:**
```
0
1
2
3
```

---

### Q3 – `let` Scope After Loop

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i);
```

**Output:**
```
0
1
2
ReferenceError
```

---

### Q4 – Classic `setTimeout` + `var`

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

**Output:**
```
3
3
3
```

---

### Q5 – `setTimeout` + `let`

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

**Output:**
```
0
1
2
```

---

### Q6 – Fix Using IIFE

```js
for (var i = 0; i < 3; i++) {
  (function (j) {
    setTimeout(() => console.log(j), 0);
  })(i);
}
```

**Output:**
```
0
1
2
```

---

### Q7 – Closure Capturing State (`var`)

```js
function test() {
  let arr = [];

  for (var i = 0; i < 3; i++) {
    arr.push(() => console.log(i));
  }

  return arr;
}

const fns = test();
fns[0]();
fns[1]();
fns[2]();
```

**Output:**
```
3
3
3
```

---

### Q8 – Closure Capturing State (`let`)

```js
function test() {
  let arr = [];

  for (let i = 0; i < 3; i++) {
    arr.push(() => console.log(i));
  }

  return arr;
}

const fns = test();
fns[0]();
fns[1]();
fns[2]();
```

**Output:**
```
0
1
2
```

---

### Q9 – Loop with Promise (`let`)

```js
for (let i = 0; i < 3; i++) {
  Promise.resolve().then(() => console.log(i));
}
```

**Output:**
```
0
1
2
```

---

### Q10 – `var` + Promise

```js
for (var i = 0; i < 3; i++) {
  Promise.resolve().then(() => console.log(i));
}
```

**Output:**
```
3
3
3
```

---

### Q11 – Mixed Sync + Async

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
  setTimeout(() => console.log(i), 0);
}
```

**Output:**
```
0
1
2
0
1
2
```

---

### Q12 – `break` Statement

```js
for (let i = 0; i < 5; i++) {
  if (i === 3) break;
  console.log(i);
}
```

**Output:**
```
0
1
2
```

---

### Q13 – `continue` Statement

```js
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i);
}
```

**Output:**
```
0
1
3
4
```

---

### Q14 – `forEach` + Async

```js
[1, 2, 3].forEach(async (num) => {
  await Promise.resolve();
  console.log(num);
});
```

**Output:**
```
1
2
3
```

> ⚠️ Runs asynchronously but order is not sequentially guaranteed.

---

### Q15 – `for...of` + Await

```js
async function test() {
  for (let num of [1, 2, 3]) {
    await Promise.resolve();
    console.log(num);
  }
}
test();
```

**Output:**
```
1
2
3
```

---

### Q16 – `while` Loop

```js
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
```

**Output:**
```
0
1
2
```

---

### Q17 – `do...while`

```js
let i = 5;

do {
  console.log(i);
  i++;
} while (i < 3);
```

**Output:**
```
5
```

---

### Q18 – Nested Loops

```js
for (let i = 0; i < 2; i++) {
  for (let j = 0; j < 2; j++) {
    console.log(i, j);
  }
}
```

**Output:**
```
0 0
0 1
1 0
1 1
```

---

### Q19 – Labelled `break`

```js
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (j === 1) break outer;
    console.log(i, j);
  }
}
```

**Output:**
```
0 0
```

---

### Q20 – Tricky Combination

```js
console.log("Start");

for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
  Promise.resolve().then(() => console.log(i));
}

console.log("End");
```

**Output:**
```
Start
End
3
3
3
3
3
3
```

---

### Q21 – `for...in` on Array

```js
const arr = [10, 20, 30];
for (let key in arr) {
  console.log(key);
}
```

**Output:**
```
0
1
2
```

> ⚠️ `for...in` returns keys (indices as strings), not values.

---

### Q22 – `for...of` on Array

```js
const arr = [10, 20, 30];
for (let val of arr) {
  console.log(val);
}
```

**Output:**
```
10
20
30
```

---

### Q23 – `for...in` on Object

```js
const obj = { a: 1, b: 2 };
for (let key in obj) {
  console.log(key);
}
```

**Output:**
```
a
b
```

---

### Q24 – Loop with `const`

```js
for (const i of [1, 2, 3]) {
  console.log(i);
}
```

**Output:**
```
1
2
3
```

---

### Q25 – Array Mutation in Loop

```js
const arr = [1, 2, 3];
for (let i = 0; i < arr.length; i++) {
  arr.push(i);
  if (i === 2) break;
}
console.log(arr);
```

**Output:**
```
[1, 2, 3, 0, 1, 2]
```

---

### Q26 – `var` Leaking Out of Block

```js
{
  var x = 10;
}
console.log(x);
```

**Output:**
```
10
```

---

### Q27 – `let` Block Scoped

```js
{
  let x = 10;
}
console.log(x);
```

**Output:**
```
ReferenceError
```

---

### Q28 – While with Closure

```js
let fns = [];
let i = 0;
while (i < 3) {
  let j = i;
  fns.push(() => console.log(j));
  i++;
}
fns[0]();
fns[2]();
```

**Output:**
```
0
2
```

---

### Q29 – `for` Loop Return Value

```js
const result = (() => {
  for (let i = 0; i < 3; i++) {
    if (i === 2) return i;
  }
})();
console.log(result);
```

**Output:**
```
2
```

---

### Q30 – Generator-like Iteration

```js
const obj = {
  [Symbol.iterator]() {
    let n = 0;
    return {
      next() {
        return n < 3 ? { value: n++, done: false } : { done: true };
      }
    };
  }
};

for (let val of obj) {
  console.log(val);
}
```

**Output:**
```
0
1
2
```

---

## 5. Hoisting

### Q1 – `var` Hoisting

```js
console.log(a);
var a = 10;
```

**Output:**
```
undefined
```

---

### Q2 – `let` Hoisting (TDZ)

```js
console.log(a);
let a = 10;
```

**Output:**
```
ReferenceError
```

---

### Q3 – `const` Hoisting (TDZ)

```js
console.log(a);
const a = 10;
```

**Output:**
```
ReferenceError
```

---

### Q4 – `var` Declared but Not Assigned

```js
var a;
console.log(a);
```

**Output:**
```
undefined
```

---

### Q5 – Function Declaration Hoisting

```js
greet();

function greet() {
  console.log("Hello");
}
```

**Output:**
```
Hello
```

---

### Q6 – Function Expression with `var`

```js
greet();

var greet = function () {
  console.log("Hello");
};
```

**Output:**
```
TypeError: greet is not a function
```

---

### Q7 – Function Expression with `let`

```js
greet();

let greet = function () {
  console.log("Hello");
};
```

**Output:**
```
ReferenceError
```

---

### Q8 – `var` vs Function Priority

```js
var a = 1;

function a() {
  console.log("Function");
}

console.log(a);
```

**Output:**
```
1
```

---

### Q9 – Function vs `var` (Reverse Order)

```js
function a() {
  console.log("Function");
}

var a = 1;

console.log(a);
```

**Output:**
```
1
```

---

### Q10 – Duplicate Function Declarations

```js
function a() {
  console.log(1);
}

function a() {
  console.log(2);
}

a();
```

**Output:**
```
2
```

---

### Q11 – Variable Shadowing (`var`)

```js
var a = 10;

function test() {
  console.log(a);
  var a = 20;
}

test();
```

**Output:**
```
undefined
```

---

### Q12 – `let` Shadowing (TDZ)

```js
let a = 10;

function test() {
  console.log(a);
  let a = 20;
}

test();
```

**Output:**
```
ReferenceError
```

---

### Q13 – Block Scope with `let`

```js
{
  console.log(a);
  let a = 5;
}
```

**Output:**
```
ReferenceError
```

---

### Q14 – `var` Inside Block

```js
{
  var a = 5;
}

console.log(a);
```

**Output:**
```
5
```

---

### Q15 – `let` Inside Block

```js
{
  let a = 5;
}

console.log(a);
```

**Output:**
```
ReferenceError
```

---

### Q16 – Hoisting with Assignment

```js
console.log(a);

var a = 10;

console.log(a);
```

**Output:**
```
undefined
10
```

---

### Q17 – Function Inside Function

```js
function test() {
  console.log(a);
  var a = 10;

  function a() {}
}

test();
```

**Output:**
```
[Function: a]
```

---

### Q18 – Function + `var` Conflict

```js
function test() {
  var a = 10;

  function a() {}

  console.log(a);
}

test();
```

**Output:**
```
10
```

---

### Q19 – Arrow Function (No Hoisting)

```js
test();

var test = () => {
  console.log("Hello");
};
```

**Output:**
```
TypeError: test is not a function
```

---

### Q20 – Complex TDZ

```js
let a = 10;

function test() {
  console.log(a);
  let a = 20;
}

test();
```

**Output:**
```
ReferenceError
```

---

### Q21 – `var` in `if` Block

```js
if (true) {
  var x = 5;
}

console.log(x);
```

**Output:**
```
5
```

---

### Q22 – `let` in `if` Block

```js
if (true) {
  let x = 5;
}

console.log(x);
```

**Output:**
```
ReferenceError
```

---

### Q23 – `const` Without Init

```js
const a;
console.log(a);
```

**Output:**
```
SyntaxError: Missing initializer in const declaration
```

---

### Q24 – Hoisting Order in Function

```js
function test() {
  console.log(typeof a);
  var a = 5;
}

test();
```

**Output:**
```
undefined
```

---

### Q25 – Function Hoisted Over `var`

```js
console.log(typeof foo);

var foo = "variable";

function foo() {}
```

**Output:**
```
function
```

---

### Q26 – Class Not Hoisted

```js
const obj = new MyClass();

class MyClass {
  constructor() {
    this.x = 10;
  }
}
```

**Output:**
```
ReferenceError
```

---

### Q27 – Hoisting in Nested Functions

```js
var x = 1;

function outer() {
  var x = 2;

  function inner() {
    console.log(x);
    var x = 3;
  }

  inner();
}

outer();
```

**Output:**
```
undefined
```

---

### Q28 – `typeof` with Undeclared

```js
console.log(typeof undeclaredVar);
```

**Output:**
```
undefined
```

---

### Q29 – `let` vs `var` Timing

```js
let a = "outer";

function test() {
  console.log(a);
}

test();
```

**Output:**
```
outer
```

---

### Q30 – Multiple Declarations

```js
var a = 1;
var a = 2;
console.log(a);
```

**Output:**
```
2
```

---

## 6. Closures & Scope

### Q1 – Basic Closure

```js
function outer() {
  let a = 10;

  function inner() {
    console.log(a);
  }

  return inner;
}

const fn = outer();
fn();
```

**Output:**
```
10
```

---

### Q2 – Closure with Updated Value

```js
function outer() {
  let a = 10;

  function inner() {
    console.log(a);
  }

  a = 20;

  return inner;
}

outer()();
```

**Output:**
```
20
```

---

### Q3 – Independent Closures

```js
function counter() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

const c1 = counter();
const c2 = counter();

c1();
c1();
c2();
```

**Output:**
```
1
2
1
```

---

### Q4 – Shared Closure

```js
function test() {
  let a = 0;

  return {
    inc: () => ++a,
    get: () => console.log(a)
  };
}

const obj = test();

obj.inc();
obj.inc();
obj.get();
```

**Output:**
```
2
```

---

### Q5 – Closure Inside Loop (`var`)

```js
function test() {
  let arr = [];

  for (var i = 0; i < 3; i++) {
    arr.push(() => console.log(i));
  }

  return arr;
}

const fns = test();
fns[0]();
fns[1]();
fns[2]();
```

**Output:**
```
3
3
3
```

---

### Q6 – Closure Inside Loop (`let`)

```js
function test() {
  let arr = [];

  for (let i = 0; i < 3; i++) {
    arr.push(() => console.log(i));
  }

  return arr;
}

const fns = test();
fns[0]();
fns[1]();
fns[2]();
```

**Output:**
```
0
1
2
```

---

### Q7 – Nested Closures

```js
function a(x) {
  return function (y) {
    return function (z) {
      console.log(x + y + z);
    };
  };
}

a(1)(2)(3);
```

**Output:**
```
6
```

---

### Q8 – Closure with setTimeout (`var`)

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

**Output:**
```
3
3
3
```

---

### Q9 – Closure with setTimeout (`let`)

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

**Output:**
```
0
1
2
```

---

### Q10 – IIFE Fix

```js
for (var i = 0; i < 3; i++) {
  (function (j) {
    setTimeout(() => console.log(j), 0);
  })(i);
}
```

**Output:**
```
0
1
2
```

---

### Q11 – Closure Remembers Reference

```js
let a = 10;

function test() {
  console.log(a);
}

a = 20;

test();
```

**Output:**
```
20
```

---

### Q12 – Shadowing

```js
let a = 10;

function test() {
  let a = 20;
  console.log(a);
}

test();
```

**Output:**
```
20
```

---

### Q13 – Scope Chain

```js
let a = 1;

function outer() {
  let b = 2;

  function inner() {
    let c = 3;
    console.log(a, b, c);
  }

  inner();
}

outer();
```

**Output:**
```
1 2 3
```

---

### Q14 – Closure with Mutation

```js
function test() {
  let a = 1;

  return function () {
    a++;
    console.log(a);
  };
}

const fn = test();
fn();
fn();
```

**Output:**
```
2
3
```

---

### Q15 – Multiple Closures Sharing State

```js
function test() {
  let a = 0;

  return [
    () => ++a,
    () => ++a,
    () => console.log(a)
  ];
}

const [f1, f2, f3] = test();

f1();
f2();
f3();
```

**Output:**
```
2
```

---

### Q16 – Closure + Async

```js
function test() {
  let a = 0;

  setTimeout(() => console.log(a), 100);

  a = 5;
}

test();
```

**Output:**
```
5
```

---

### Q17 – Closure with Object

```js
function test() {
  let obj = { value: 10 };

  return function () {
    obj.value++;
    console.log(obj.value);
  };
}

const fn = test();
fn();
fn();
```

**Output:**
```
11
12
```

---

### Q18 – Reassigning Object

```js
function test() {
  let obj = { value: 10 };

  return function () {
    obj = { value: 20 };
    console.log(obj.value);
  };
}

const fn = test();
fn();
fn();
```

**Output:**
```
20
20
```

---

### Q19 – Closure with Default Param

```js
function test(a = 10) {
  return function () {
    console.log(a);
  };
}

test()();
```

**Output:**
```
10
```

---

### Q20 – Tricky Mixed Case

```js
let a = 1;

function outer() {
  let a = 2;

  return function () {
    console.log(a);
  };
}

const fn = outer();

a = 100;

fn();
```

**Output:**
```
2
```

---

### Q21 – Closure Factory

```js
function multiplier(x) {
  return (y) => x * y;
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5));
console.log(triple(5));
```

**Output:**
```
10
15
```

---

### Q22 – Private Variable Pattern

```js
function createBank() {
  let balance = 100;
  return {
    deposit(n) { balance += n; },
    getBalance() { return balance; }
  };
}

const acc = createBank();
acc.deposit(50);
console.log(acc.getBalance());
```

**Output:**
```
150
```

---

### Q23 – Stale Closure

```js
function test() {
  let val = 0;
  const log = () => console.log(val);
  val = 42;
  return log;
}

test()();
```

**Output:**
```
42
```

---

### Q24 – Closure in Recursion

```js
function makeAdder(x) {
  return function add(y) {
    if (y === 0) return x;
    return add(y - 1) + 1;
  };
}

console.log(makeAdder(5)(3));
```

**Output:**
```
8
```

---

### Q25 – Immediately Returned Closure

```js
const fn = (function () {
  let x = 0;
  return () => ++x;
})();

console.log(fn());
console.log(fn());
console.log(fn());
```

**Output:**
```
1
2
3
```

---

### Q26 – Closure vs Global

```js
let x = "global";

function outer() {
  let x = "outer";

  return function () {
    console.log(x);
  };
}

const fn = outer();
x = "changed";
fn();
```

**Output:**
```
outer
```

---

### Q27 – Block Scope Closure

```js
{
  let secret = 42;
  var getSecret = () => secret;
}

console.log(getSecret());
```

**Output:**
```
42
```

---

### Q28 – Async Closure

```js
async function test() {
  let val = 1;

  await Promise.resolve();

  val = 2;

  return val;
}

test().then(console.log);
```

**Output:**
```
2
```

---

### Q29 – Closure Argument Capture

```js
function test(x) {
  return function (y) {
    x += y;
    console.log(x);
  };
}

const fn = test(10);
fn(5);
fn(5);
```

**Output:**
```
15
20
```

---

### Q30 – Module Pattern

```js
const module = (() => {
  let count = 0;

  return {
    inc: () => ++count,
    reset: () => { count = 0; },
    get: () => count
  };
})();

module.inc();
module.inc();
module.inc();
module.reset();
module.inc();
console.log(module.get());
```

**Output:**
```
1
```

---

## 7. Array Methods

### Q1 – `map()`

```js
const arr = [1, 2, 3];
const res = arr.map(x => x * 2);
console.log(res);
```

**Output:**
```
[2, 4, 6]
```

---

### Q2 – `map()` Without `return`

```js
const arr = [1, 2, 3];
const res = arr.map(x => { x * 2 });
console.log(res);
```

**Output:**
```
[undefined, undefined, undefined]
```

---

### Q3 – `filter()`

```js
const arr = [1, 2, 3, 4];
const res = arr.filter(x => x % 2 === 0);
console.log(res);
```

**Output:**
```
[2, 4]
```

---

### Q4 – `filter()` Always True

```js
const arr = [1, 2, 3];
const res = arr.filter(() => true);
console.log(res);
```

**Output:**
```
[1, 2, 3]
```

---

### Q5 – `reduce()` Sum with Initial Value

```js
const arr = [1, 2, 3];
const res = arr.reduce((acc, cur) => acc + cur, 0);
console.log(res);
```

**Output:**
```
6
```

---

### Q6 – `reduce()` Without Initial Value

```js
const arr = [1, 2, 3];
const res = arr.reduce((acc, cur) => acc + cur);
console.log(res);
```

**Output:**
```
6
```

---

### Q7 – `reduce()` Product

```js
const arr = [1, 2, 3, 4];
const res = arr.reduce((acc, cur) => acc * cur, 1);
console.log(res);
```

**Output:**
```
24
```

---

### Q8 – `forEach()` Return Value

```js
const arr = [1, 2, 3];
const res = arr.forEach(x => x * 2);
console.log(res);
```

**Output:**
```
undefined
```

---

### Q9 – `slice()`

```js
const arr = [1, 2, 3, 4];
const res = arr.slice(1, 3);
console.log(res);
```

**Output:**
```
[2, 3]
```

---

### Q10 – `splice()`

```js
const arr = [1, 2, 3, 4];
const res = arr.splice(1, 2);
console.log(res, arr);
```

**Output:**
```
[2, 3] [1, 4]
```

---

### Q11 – `concat()`

```js
const a = [1, 2];
const b = [3, 4];
console.log(a.concat(b));
```

**Output:**
```
[1, 2, 3, 4]
```

---

### Q12 – `push()` Return Value

```js
const arr = [1, 2];
const res = arr.push(3);
console.log(res, arr);
```

**Output:**
```
3 [1, 2, 3]
```

---

### Q13 – `pop()` Return Value

```js
const arr = [1, 2, 3];
const res = arr.pop();
console.log(res, arr);
```

**Output:**
```
3 [1, 2]
```

---

### Q14 – `shift()` Return Value

```js
const arr = [1, 2, 3];
const res = arr.shift();
console.log(res, arr);
```

**Output:**
```
1 [2, 3]
```

---

### Q15 – `unshift()` Return Value

```js
const arr = [2, 3];
const res = arr.unshift(1);
console.log(res, arr);
```

**Output:**
```
3 [1, 2, 3]
```

---

### Q16 – `includes()`

```js
const arr = [1, 2, 3];
console.log(arr.includes(2));
```

**Output:**
```
true
```

---

### Q17 – `find()`

```js
const arr = [1, 2, 3, 4];
const res = arr.find(x => x > 2);
console.log(res);
```

**Output:**
```
3
```

---

### Q18 – `findIndex()`

```js
const arr = [1, 2, 3];
const res = arr.findIndex(x => x === 2);
console.log(res);
```

**Output:**
```
1
```

---

### Q19 – `sort()` (Lexicographic)

```js
const arr = [10, 2, 5];
arr.sort();
console.log(arr);
```

**Output:**
```
[10, 2, 5]
```

> ⚠️ Default sort is lexicographic (string comparison).

---

### Q20 – `sort()` with Comparator

```js
const arr = [10, 2, 5];
arr.sort((a, b) => a - b);
console.log(arr);
```

**Output:**
```
[2, 5, 10]
```

---

### Q21 – `reverse()`

```js
const arr = [1, 2, 3];
arr.reverse();
console.log(arr);
```

**Output:**
```
[3, 2, 1]
```

---

### Q22 – `flat()`

```js
const arr = [1, [2, [3]]];
console.log(arr.flat(2));
```

**Output:**
```
[1, 2, 3]
```

---

### Q23 – `map()` + `filter()` Chain

```js
const arr = [1, 2, 3, 4];
const res = arr
  .map(x => x * 2)
  .filter(x => x > 4);

console.log(res);
```

**Output:**
```
[6, 8]
```

---

### Q24 – `reduce()` to Object

```js
const arr = ["a", "b", "a"];
const res = arr.reduce((acc, cur) => {
  acc[cur] = (acc[cur] || 0) + 1;
  return acc;
}, {});

console.log(res);
```

**Output:**
```
{ a: 2, b: 1 }
```

---

### Q25 – Tricky Mutation in `map()`

```js
const arr = [1, 2, 3];
const res = arr.map((x, i) => {
  arr.push(x);
  return x;
});

console.log(res);
```

**Output:**
```
[1, 2, 3]
```

> ⚠️ `map()` uses the original array length; pushed items are ignored.

---

### Q26 – `some()`

```js
const arr = [1, 2, 3];
console.log(arr.some(x => x > 2));
```

**Output:**
```
true
```

---

### Q27 – `every()`

```js
const arr = [2, 4, 6];
console.log(arr.every(x => x % 2 === 0));
```

**Output:**
```
true
```

---

### Q28 – `flatMap()`

```js
const arr = [1, 2, 3];
console.log(arr.flatMap(x => [x, x * 2]));
```

**Output:**
```
[1, 2, 2, 4, 3, 6]
```

---

### Q29 – `indexOf()` vs `findIndex()`

```js
const arr = [1, 2, NaN];
console.log(arr.indexOf(NaN));
console.log(arr.findIndex(x => Number.isNaN(x)));
```

**Output:**
```
-1
2
```

---

### Q30 – `reduceRight()`

```js
const arr = [[1, 2], [3, 4], [5, 6]];
const res = arr.reduceRight((acc, cur) => acc.concat(cur), []);
console.log(res);
```

**Output:**
```
[5, 6, 3, 4, 1, 2]
```

---

## 💡 Core Concepts Quick Reference

| Topic | Key Rule |
|---|---|
| **Promises** | Microtasks run before macrotasks |
| **Event Loop** | Sync → Microtasks → Macrotasks |
| **this** | Depends on *how* a function is called, not *where* |
| **Loops** | `var` = function scope, `let` = block scope |
| **Hoisting** | `var` → `undefined`, `let`/`const` → TDZ |
| **Closures** | Captures reference, not value |
| **Array Methods** | `map`/`filter`/`reduce` return new; `splice` mutates |

---

## ⚡ Interview Tip

Most interview questions are combinations of:
- Closure + Loop (`var` vs `let`)
- Promise + Event Loop
- `this` + Function Call Style
- Hoisting + TDZ

Master these patterns and you can solve 90% of JS output questions instantly.
