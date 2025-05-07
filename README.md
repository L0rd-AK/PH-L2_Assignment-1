# 1. What is the use of the `keyof` keyword in TypeScript?

## Introduction

TypeScript makes JavaScript better by adding static types, helping us catch errors early and write more reliable code. One of its cool features is the `keyof` keyword, which lets us create types based on the property names of existing objects. Let’s break it down and see why it’s useful with a simple example.

---

## What is `keyof`?

The `keyof` operator takes an object type and gives us a union of its keys. Basically, if you have a type or interface `T`, `keyof T` gives you a type that includes all the property names of `T`.

```ts
interface User {
    id: number;
    name: string;
    email: string;
}

type UserKeys = keyof User; 
```

Here, `UserKeys` will be `"id" | "name" | "email"`.

---

## Why use `keyof`?

1. **Type Safety**: It ensures you only use valid property names of an object.
2. **Dynamic Access**: Great for writing generic functions that work with object properties.
3. **Easy Refactoring**: If you change the object’s properties, `keyof` updates automatically.

---

## Example: Generic Property Getter

Here’s an example of a function `getProperty` that safely gets a value from an object using a key. Thanks to `keyof`, it only allows valid keys.

```ts
interface Product {
    id: number;
    name: string;
    price: number;
}

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const product: Product = {
    id: 101,
    name: "Laptop",
    price: 1500
};

// Valid:
const productName = getProperty(product, "name");  // type: string
const productPrice = getProperty(product, "price"); // type: number

// Invalid (compile-time error):
// const invalid = getProperty(product, "color");
```

In this example:
* `K extends keyof T` ensures `key` is a valid property name.
* `T[K]` infers the return type based on the key.

---

## Conclusion

The `keyof` keyword is super handy for creating types from object keys. It makes your code safer, easier to maintain, and more flexible. If you’re working with TypeScript, give `keyof` a try—it’s a game-changer!

# 2. Using Union and Intersection Types in TypeScript

## Introduction

TypeScript has some awesome features like **union** and **intersection** types. These let us handle complex data structures easily. Unions allow a value to be one of several types, while intersections combine multiple types into one. Let’s dive into both with examples.

---

## Union Types

A **union type** means a value can be one of several types. You use the `|` symbol to define it.

```ts
type StringOrNumber = string | number;

let value: StringOrNumber;
value = "Hello"; // OK
value = 42;      // OK
//value = true;  // Error: boolean is not assignable
```

### When to use Union Types

* **Flexible APIs**: Accept different input types.
* **Polymorphic functions**: Handle multiple types in one function.
* **Error handling**: Represent success or error states.

#### Example: Handling Multiple Input Types

```ts
function format(input: string | number): string {
    if (typeof input === "number") {
        return input.toFixed(2); // Format number
    }
    return input.toUpperCase(); // Format string
}

console.log(format(3.1415)); // "3.14"
console.log(format("hello")); // "HELLO"
```

---

## Intersection Types

An **intersection type** combines multiple types into one. Use the `&` symbol to define it.

```ts
interface HasName {
    name: string;
}

interface HasAge {
    age: number;
}

type Person = HasName & HasAge;

const p: Person = {
    name: "Alice",
    age: 30,
};
```

### When to use Intersection Types

* **Composing interfaces**: Combine smaller types into a bigger one.
* **Mixins**: Add multiple behaviors to objects.
* **Type narrowing**: Refine types with guards.

#### Example: Combining User and Admin Permissions

```ts
interface User {
    id: string;
    username: string;
}

interface Admin {
    isAdmin: boolean;
}

type AdminUser = User & Admin;

function login(user: AdminUser) {
    console.log(`Welcome ${user.username}`);
    if (user.isAdmin) {
        console.log("Access level: ADMIN");
    }
}

const currentUser: AdminUser = {
    id: "123",
    username: "jdoe",
    isAdmin: true,
};

login(currentUser);
```

---

## Practical Tips

* **Union Types**: Use them when a value can be one of several types.
* **Intersection Types**: Use them when a value needs to satisfy multiple types.
* Combine them for advanced use cases:

    ```ts
    type Response =
        | { success: true; data: string }
        | { success: false; error: Error };

    type DetailedResponse = Response & { timestamp: Date };
    ```

---

## Conclusion

Union and intersection types are powerful tools in TypeScript. They help you design flexible, type-safe code that’s easy to maintain. Start using them to make your TypeScript projects more robust and expressive!
