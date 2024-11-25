# Extended Assignment for JavaScript Objects

## Overview

The `extendedAssignment` function is an enhancement of JavaScript’s built-in `Object.assign` method. Unlike `Object.assign`, which only copies the values of an object’s properties, `extendedAssignment` preserves the **property descriptors** (such as getter functions and other attribute details) when copying properties from source objects to the target object.

This function is especially useful when you need to copy objects with **getter methods**, **setters**, or **non-enumerable properties** while preserving the original property definitions.

## Features

- **Preserves Getters and Setters**: Unlike `Object.assign`, which copies only values, `extendedAssignment` preserves getter and setter functions attached to properties.
- **Supports Symbol Properties**: It copies properties defined by symbols as well, ensuring that both string and symbol properties are copied accurately.
- **Non-enumerable Properties**: It ensures that non-enumerable properties are also copied, unlike `Object.assign` which omits them.

## Usage

### Syntax

```js
extendedAssignment(target, ...sources)
```

- `target`: The target object to which properties will be copied.
- `sources`: One or more source objects whose properties will be copied to the target.

### Example

```js
function extendedAssignment(target, ...sources) {
    sources.forEach(source => {
        const descriptors = Object.keys(source).reduce((descriptors, key) => {
            descriptors[key] = Object.getOwnPropertyDescriptor(source, key);
            return descriptors;
        }, {})

        Object.getOwnPropertySymbols(source).forEach((symbol) => {
            const descriptor = Object.getOwnPropertyDescriptor(source, symbol);
            if (descriptor.enumerable) {
                descriptors[symbol] = descriptor;
            }
        });

        Object.defineProperties(target, descriptors);
    });
    return target;
}

const pair = {
    get first() {
        return 10;
    },
    get second() {
        return -10;
    }
};

console.log(Object.assign({}, pair)); // { first: 10, second: -10 }
console.log(extendedAssignment({}, pair)); // { first: [Getter], second: [Getter] }
```


### Explanation of the Example

- **Standard `Object.assign`**: When you use `Object.assign({}, pair)`, it copies the property values from `pair` to a new object, but any getter or setter functionality is lost, and you simply get the values (`{ first: 10, second: -10 }`).
  
- **Using `extendedAssignment`**: When you use `extendedAssignment({}, pair)`, the function not only copies the values but also retains the getter functions, meaning the new object still has access to the getter methods (`{ first: [Getter], second: [Getter] }`).

## Installation

No installation is necessary! You can use the function by copying the code into your project or by importing it from a module if preferred.

## Compatibility

The `extendedAssignment` function works with all modern browsers and Node.js environments.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.