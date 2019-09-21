---
layout: page
title: Yup
---


## Importing Yup

```import * as yup from "yup";```

or

```import { string, required } from 'yup';```

## The Schema


```
import * as yup from "yup";

const checkoutAddressSchema = yup.object().shape({
  email_address: yup
    .string()
    .email()
    .required(),
  full_name: yup.string().required(),
  house_no: yup
    .number()
    .required()
    .positive()
    .integer(),
  address_1: yup.string().required(),
  address_2: yup.string(),
  post_code: yup.string().required(),
  timestamp: yup.date().default(() => {
    return new Date();
  })
});
```

## The object to validate


```
let addressFormData = {
  email_address: "ross@jkrbinvestments.com",
  full_name: "Ross Bulat",
  house_no: null,
  address_1: "My Street Name",
  post_code: "AB01 0AB"
};
```

 As you can see, the address_2 and timestamp fields are missing from here — but they are not required in our schema.
 
## isValid
 
 ```
 checkoutAddressSchema.isValid(addressFormData).then(function(valid) {
  //valid - true or false
  console.log("000", valid); // => false
});

let schema = yup.mixed().oneOf(['jimmy', 42]);
 
await schema.isValid(42); // => true
await schema.isValid('jimmy'); // => true
await schema.isValid(new Date()); // => false
```
## validate

```
checkoutAddressSchema.validate(addressFormData).catch(function(err) {
  console.error("111 err.name", err.name); // => ValidationError
  console.error("111 err.errors", err.errors); // => ["house_no must be a `number` type, but the final value was: `NaN` (cast from the value `NaN`)."]
});

let schema = yup.object().shape({
  name: yup.string(),
  age: yup.number().min(18),
});

schema.validate({ name: 'jimmy', age: 24 }).then(function(value) {
  value; // => { name: 'jimmy',age: 24 }
});
 
schema.validate({ name: 'jimmy', age: 'hi' }).catch(function(err) {
  err.name; // => 'ValidationError'
  err.errors; // => ['age must be a number']
});
```

## cast

```
let schema = yup.array().of(yup.number().min(2));
 
await schema.isValid([2, 3]); // => true
await schema.isValid([1, -24]); // => false
 
schema.cast(['2', '3']); // => [2, 3]
```

## concat

Creates a new instance of the schema by combining two schemas. Only schemas of the same type can be concatenated.
Perhaps you are combining multiple objects into one API call body, and therefore want to validate the entire body with one Yup object. concat() will come in handy for this.

## abortEarly

Aborting validation early. By default Yup runs through an entire validation process and collects errors as it runs, and returns them all once validation is complete. We may not want to do this. We can use abortEarly to stop validation execution as soon as the first error crops up. abortEarly is passed as an options argument within validate():

```await checkoutAddressSchema.validate(addressFormData, { abortEarly: false })```

* Yup has some great string utilities, such as trim(), that removes whitespace around a string. uppercase() and lowercase() are available to enforce capitalising or not.
* Yup also has some great number utilities, such as min() and max(), as well as round(), morethan(), lessthan(), and truncate().
* Yup supports regex, allowing you to utilise regular expressions.
