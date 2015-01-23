#Error: ValidationError: value must be one of null (Hapi Joi)

Hapi plugin Joi might return error message similar to this one:

```
Error: ValidationError: value must be one of null
    at module.exports (/Users/Marek/Workspace/application/app/config/index.js:37:9)
```

Problem most likely lies in invalid values passed into `Joi.validate(value, schema, callback)` method.  

When you look at the [validate method signature](https://github.com/hapijs/joi#validatevalue-schema-options-callback). Just make sure you are passing object as first paramether and schema as second one.