
>自动绑定函数

#auto-bind-until

*转载需加来源*

```
export default () => {
    return (target, key, { value: fn }) => {
        if (typeof fn !== 'function') {
            throw new SyntaxError(`@autobind can only be used on functions, not: ${fn}`)
        }
        return {
            configurable: true,
            enumerable: false,

            get () {
                const boundFn = fn.bind(this);

                Object.defineProperty(this, key, {
                    configurable: true,
                    writable: true,
                    // NOT enumerable when it's a bound method
                    enumerable: false,
                    value: boundFn
                })
                return boundFn
            },
            set (newValue) {
                Object.defineProperty(this, key, {
                    configurable: true,
                    writable: true,
                    // IS enumerable when reassigned by the outside word
                    enumerable: true,
                    value: newValue
                })
                return newValue
            }
        }
    }
}

```
