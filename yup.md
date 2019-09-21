---
layout: page
title: Yup
---

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
let addressFormData1 = {
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
 checkoutAddressSchema.isValid(addressFormData1).then(function(valid) {
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
checkoutAddressSchema.validate(addressFormData1).catch(function(err) {
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
