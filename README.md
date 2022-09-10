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
