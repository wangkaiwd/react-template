## `TypeScript`接口
在`TypeScript`中，接口使用来描述对象应该有哪些属性以及属性对应的类型。  

这里我们通过一个打印姓名的函数来进行接口使用的代码演示：
```typescript
// 接口是用来描述对象应该有哪些属性以及属性对应的类型
// const printName = (human: { name: string }) => {
//   console.log(human.name);
// };
// // 这里我们传入了很多属性，但是typescript只会检查必要的属性(label)是否存在，并且其类型是否匹配
// const human = { name: 'wangkaiwd' };
// printName(human);

// 用接口改写这个例子
interface Human {
  name: string;
}

const printLabel = (human: Human) => {
  console.log(human.name);
};

const human = { name: 'wangkaiwd' };
printLabel(human);
```

### 可选属性和只读属性
接口中的可选属性是在属性后加`?`,而只读属性是在属性前加`readonly`。下面是一个例子：  
```typescript
interface Human {
  name?: string;
  age?: number;
  readonly gender: string;
}
// 可选属性的好处：
//    1. 对可能存在的属性进行预先定义，可以让开发者更加清楚的直到哪些参数可能会不传
//    2. 当我们使用接口中不存在的对象属性时候会有错误提示，而不是得到undefined
const printNameOrAge = (human: Human) => {
  let newHuman: Human = { gender: 'woman' };
  // 只读属性不可以重新赋值来改变
  // human.gender = 'woman';
  if (human.name) {
    newHuman.name = human.name;
  }
  if (human.age) {
    newHuman.age = human.age;
  }
  console.log(newHuman);
};
const human = { name: 'wangkaiwd', age: 12, gender: 'man' };
printNameOrAge(human);
```
### 复杂对象
接下来我们用接口来描述一个稍微复杂一些的对象：  
```typescript
interface Human {
  name: string;
  age: number;
  readonly gender: string;
  score: {
    music: number,
    sport: number
  };
  likeGames?: string[];

  say (word: string): void;
}

// 定义一个稍微复杂一些的对象
const human: Human = {
  name: 'wangkaiwd',
  age: 12,
  gender: 'man',
  score: { music: 10, sport: 6 },
  likeGames: ['吃鸡', '王者荣耀', '古代战争'],
  say (word: string) {
    console.log(`${this.name} say: ${word}`);
  }
};
human.say('I am saying');
```
这里我们通过一个`human`的对象的`interface`来学习如何通过接口来定义对象属性是一个对象、数组以及函数，进一步加深我们对接口的理解。

### 用接口来描述函数

我们先写一个求和的函数，然后用接口来描述它的类型：  
```typescript
interface Add {
  // 描述当前函数：参数和返回值
  (a: number, b: number): number;
}
const add: Add = (a, b) => {
  return a + b;
};
console.log('add', add(1, 2)); // 3
```

但是如果这个函数的属性依旧是一个函数该怎么办呢？  
```typescript
interface Add {
  // 描述当前函数：参数和返回值
  (a: number, b: number): number;

  // 当前函数的属性opposite也是一个函数
  opposite (a: number, b: number): number
}
const add: Add = (() => {
  const x: any = (a: number, b: number) => {
    return a + b;
  };
  x.opposite = (a: number, b: number) => {
    return a - b;
  };
  return x;
})();
console.log('add', add.opposite(2, 1));
```
这里我们通过一个自执行函数来让返回值符合接口描述的类型

### 可索引类型
我们也可以通过接口来描述一个对象`key`和`value`分别对应的类型，下面是一个例子： 
```typescript
// 对象的属性为任意字符串，属性值为数值
interface NumberObject {
  [key: string]: number
}

// 这里的属性值必须为number类型
// const obj1: NumberObject = { name: 'wangkaiwd', age: 12 }; // Type 'string' is not assignable to type 'number'.
const obj1: NumberObject = { name: 11, age: 12 }; // ok

// 数组中每一项都是字符串
interface StringArray {
  [key: number]: string
}

// string类型不能分配给number
// const arr: StringArray = [1,2]; //TS2322: Type 'string' is not assignable to type 'number'.

const arr: StringArray = ['1', '2']; // OK
```
这里需要注意的一点是，当我们使用索引类型的时候，它会要求接口中所有属性和属性值都必须与索引类型项匹配：
```typescript
interface NumberObject {
  [key: string]: number;
  hairColor: string // TS2411: Property 'hairColor' of type 'string' is not assignable to string index type 'number'.
}
```
上面我们定义了`hairColor`属性的属性值得类型为`string`，`TypeScript`会提示我们不能将字符串类型分配给`number`

### 接口继承
接口之间是可以相互继承的，这样我们能从一个接口里复制成员到另一个接口中，使接口运用更加灵活：  
```typescript
interface Biology {
  think (): void;
}

interface Animal {
  // 函数声明
  sleep (): void;
}

// 继承一个接口
// interface Human extends Animal {
//   name: string;
//   age: number;
// }

// 可以通过逗号分割来继承多个类
interface Human extends Animal, Biology {
  name: string;
  age: number;
}

const human: Human = {
  name: 'wangkaiwd',
  age: 12,
  sleep () {
    console.log('I am sleeping');
  },
  think () {
    console.log('I am thinking');
  }
};
human.sleep();
human.think();
```
这里我们可以看到，接口不仅可以继承一个接口，也可以继承多个接口，这极大的提升了代码的复用性和灵活性
