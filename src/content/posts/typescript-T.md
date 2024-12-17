---
title: TypeScript
published: 2024-02-18
description: '理解TypeScript中的范型应用，提升TypeScript项目的开发效率以及代码复用率'
image: ''
tags: [TypeScript]
category: 'TypeScript'
draft: false 
---

### TypeScript 泛型详解与实际应用

随着项目复杂度的提升，前端开发中封装通用函数的需求变得尤为重要。在 TypeScript 的环境下，我们还需要在确保类型安全的前提下处理多种参数类型。这时，泛型（Generic）就成为了解决这一问题的关键工具。

#### 一、泛型的概念

泛型是一种在编写代码时无需提前指定具体类型，而是在使用时根据实际情况动态传递类型的机制。它就像代码中的“占位符”，只有在实际调用时才确定最终的类型。

##### 1.1 为什么需要泛型？

假设我们需要一个函数来获取数组的第一个元素，常规写法如下：

```typescript
function getFirstElement(arr: number[]): number {
    return arr[0];
}
```

此函数只能处理 `number[]` 类型。如果要处理其他类型的数组，需要重复定义类似的函数，或者使用联合类型：

```typescript
function getFirstElement(arr: number[] | string[] | boolean[]): number | string | boolean {
    return arr[0];
}
```

虽然这种方法扩展性稍强，但仍存在问题：

1. **可扩展性差**：增加新的类型（如 `Date[]` 或 `Object[]`）需要不断修改函数签名。
2. **类型安全性不足**：返回值的类型推断不够准确。例如：

```typescript
const result = getFirstElement([1, 2, 3]); // result 类型为 number | string | boolean
```

为了解决这些问题，TypeScript 提供了泛型支持。

##### 1.2 使用泛型优化代码

泛型能够让函数适配任意类型数组，同时保证类型安全：

```typescript
function getFirstElement<T>(arr: T[]): T {
    return arr[0];
}
```

在这里，`<T>` 是泛型参数，`T` 表示任意类型。当调用此函数时，TypeScript 会根据实际传入的数组类型推断出 `T` 的具体类型。例如：

```typescript
const num = getFirstElement([1, 2, 3]); // T 为 number
const str = getFirstElement(["a", "b", "c"]); // T 为 string
const bool = getFirstElement([true, false]); // T 为 boolean
```

通过泛型，我们实现了代码复用，同时保持了类型安全。

#### 二、泛型在函数中的应用

在实际开发中，泛型能够灵活处理各种数据类型的函数和属性，减少代码冗余。

##### 2.1 泛型处理对象属性

结合 `keyof`，我们可以提取对象中某个属性的值，并保证属性名和返回值类型的正确性：

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
```

示例：

```typescript
const user = { name: "John", age: 30, isAdmin: true };

const userName = getProperty(user, "name"); // 类型为 string
const userAge = getProperty(user, "age"); // 类型为 number
const isAdmin = getProperty(user, "isAdmin"); // 类型为 boolean
```

如果尝试访问不存在的属性，编译器会报错：

```typescript
// getProperty(user, "address"); // Error: 'address' 不存在于类型 'user' 中
```

##### 2.2 更新对象属性值

通过泛型和 `keyof` 的结合，可以确保更新的属性和值是类型安全的：

```typescript
function setProperty<T, K extends keyof T>(obj: T, key: K, value: T[K]): void {
    obj[key] = value;
}
```

示例：

```typescript
const product = { id: 1, name: "Laptop", price: 1200 };

setProperty(product, "price", 1300); // 正确
// setProperty(product, "price", "1300"); // Error: 类型不匹配
// setProperty(product, "discount", 10); // Error: 'discount' 不存在
```

#### 三、泛型在类与接口中的应用

泛型不仅适用于函数，在类和接口中也同样强大。

##### 3.1 泛型接口

泛型可以让接口更加灵活。例如，键值对结构的接口：

```typescript
interface KeyValuePair<K, V> {
    key: K;
    value: V;
}
```

使用示例：

```typescript
const stringPair: KeyValuePair<string, string> = { key: "name", value: "Alice" };
const numberPair: KeyValuePair<string, number> = { key: "age", value: 30 };
```

##### 3.2 泛型类

以一个栈（Stack）类为例，使用泛型可以适配不同类型的数据：

```typescript
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }
}
```

使用示例：

```typescript
const numberStack = new Stack<number>();
numberStack.push(10);
numberStack.push(20);
console.log(numberStack.pop()); // 输出 20

const stringStack = new Stack<string>();
stringStack.push("hello");
console.log(stringStack.pop()); // 输出 "hello"
```

#### 四、泛型的高级应用

##### 4.1 条件类型与泛型

条件类型可与泛型结合，用于动态推导类型。例如：

```typescript
type IsArray<T> = T extends Array<any> ? true : false;

const check1: IsArray<number[]> = true; // 正确
const check2: IsArray<string> = false; // 正确
```

##### 4.2 映射类型

映射类型可以将对象的属性设置为可选或只读。例如，自定义 `Partial` 类型：

```typescript
type Partial<T> = {
    [P in keyof T]?: T[P];
};

interface User {
    name: string;
    age: number;
    email: string;
}

function updateUser(id: number, updates: Partial<User>): void {
    console.log(`更新用户 ${id} 的信息:`, updates);
}

updateUser(1, { age: 30 }); // 更新年龄
updateUser(2, { name: "Bob" }); // 更新姓名
```

#### 五、总结

泛型是 TypeScript 提供的强大工具，能够提升代码的灵活性、复用性，并确保类型安全。在日常开发中，通过结合 `keyof`、条件类型等特性，泛型帮助开发者更高效地编写维护性强的代码。