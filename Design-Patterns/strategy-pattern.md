# Strategy Pattern

策略模式是指如果一个系统有许多的类，而且它们之间的区别仅在于它们的行为，那么使用策略模式可以动态的让一个对象在许多行为中选择一种

## How it works

例如我们要对几个表单进行验证，验证这个动作，对于每个表单都需要，但是验证的具体逻辑实现都不一样，在不使用策略模式的时候，我们一般使用 `swith` 语句来实现

```javascript
var validator = {
    validate: function (value, type) {
        switch (type) {
            case 'isNonEmpty':
                return value !== '';
            case 'isNumber':
                return !isNaN(value);
            case 'isAlphaNum':
                return !/[^a-z0-9]/i.test(value);
            default:
                return true;
        }
    }
};

console.log(validator.validate('123', 'isNonEmpty');
```

但如果增加需求的话，就要不断地修改这段代码以增加逻辑，而且不能做到配置化，哪个表单需要哪个验证，这样逻辑就会越来越多

这时我可以把我的 `validator` 写成策略模式，把验证的类型的逻辑放在一个对象里，把配置放在一个对象里

```javascript
var validator = {
    types: {},
    errMessage: [],
    config: {},
    validate: function (data) {
        var i, msg, type, checker, result;
        this.errMessage = [];
        
        for (i in data) {
            if (data.hasOwnProperty(i)) {
                type = this.config[i]; // 得到特定字段的验证类型
                checker = this.types[type]; // 得到验证类型的类 
                
                if (!type) {
                    continue; // 该字段没有验证要求
                }
                if (!checker) {
                    throw {
                        name: 'ValidationError',
                        message: 'No handler to validate type ' + type
                    };
                }
                
                result = checker.validate(data[i]); // 验证通过与否
                if (!result) {
                    msg = 'Invalid value for ' + i + ', ' +
                            checker.instructions;
                    this.errMessage.push(msg);
                }
            }
        }
        return this.hasErrors();
    },
    hasErrors: function () {
        return this.errMessage.length !== 0;
    }
};
```

此时我们可以根据我们的要求，在 `validator.types` 添加自己需要用到的验证类型

```javascript
validator.types.isNonEmpty = {
    validate: function (value) {
        return value !== "";
    },
    instructions: "传入的值不能为空"
};

validator.types.isNumber = {
    validate: function (value) {
        return !isNaN(value);
    },
    instructions: "传入的值只能是合法的数字，例如：1, 3.14 or 2010"
};

validator.types.isAlphaNum = {
    validate: function (value) {
        return !/[^a-z0-9]/i.test(value);
    },
    instructions: "传入的值只能保护字母和数字，不能包含特殊字符"
};
```

还有我们对每个字段的配置，就是每个字段对应的验证类型

```javascript
validator.config = {
    first_name: 'isNonEmpty',
    age: 'isNumber',
    username: 'isAlphaNum'
};
```

这时当我们拿到我们的数据，就可以直接验证了

```javascript
var data = {
    firstName: 'Jason',
    lastName: 'Liao',
    age: 'undefined',
    username: 'JasonLiao'
};

validator.validate(data);

if (validator.hasErrors()) {
    console.log(validator.message.join('\n'));
}
```

虽然看起来要写多更多的东西，但是其实我们的 `validator.types` 是可以先写好的，有哪几种验证类型，然后根据用户的需求，动态配置好我们的 `config`，最后传入数据，就可以实现多个数据的验证处理了