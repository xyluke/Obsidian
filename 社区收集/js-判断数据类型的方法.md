![[../img/Pasted image 20231211101925.png]]

~~~js
export class TypeUtil {
    static normal(param) {
        return Object.prototype.toString.call(param).replace(/\[|\]/g, '').split(' ')[1];
    }


    static isObject(param) {
        return this.normal(param) === 'Object';
    }


    static isArray(param) {
        return this.normal(param) === 'Array';
    }


    static isFunction(param) {
        return this.normal(param) === 'Function';
    }


    static isString(param) {
        return this.normal(param) === 'String';
    }

    static isBoolean(param) {
        return this.normal(param) === 'Boolean';
    }

    static isUndefined(param) {
        return this.normal(param) === 'Undefined';
    }


    static isNull(param) {
        return this.normal(param) === 'Null';
    }
}
~~~