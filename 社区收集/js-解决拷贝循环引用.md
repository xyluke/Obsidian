~~~js
 let getCircularReplacer = () => {
        const seen = new WeakSet();
        return (key, value) => {
            if (typeof value === 'object' && value !== null) {
                if (seen.has(value)) {
                    return undefined;
                }
                seen.add(value);
            }
            return value;
        }
    }
let a = {b: '', c: function(){}}
a.p = a
let globalCopy = JSON.parse(JSON.stringify(a, getCircularReplacer()));
console.log(globalCopy);
~~~