envaridator
=====

Envaridator is a tiny environment variable validation library. It's main purpose is
parsing, registering and validating environment variables.

An example validation with envaridator would look like this:

NOTE: 

- This example uses https://github.com/hf/toi validators.
- Validators are of type: Validator<T> = (value: string | undefined) => T;

```
let envaridator = new Envaridator();

process.env['CORRECT_URL'] = 'https://google.com';
process.env['WRONG_URL'] = 'htttps://google.com';
export const okURL = envaridator.register('CORRECT_URL', toi.required().and(toix.str.url({ protocol: "https:" }))) // SHOULD PASS
export const wrongURL = envaridator.register('WRONG_URL', toi.required().and(toix.str.url({ protocol: "https:" }))) // SHOULD FAIL


process.env['SOME_VARIABLE'] = 'I exist!';
export const undefinedVariable = envaridator.register('V@R1ABL3', toi.required().and(toi.str.is())); // SHOULD FAIL
export const existingVariable = envaridator.register('SOME_VARIABLE', toi.required().and(toi.str.is())); // SHOULD PASS
 
process.env['SOME_OTHER_VARIABLE'] = 'I exist as well!';
export const objMyb = { 
  VARIABLE3: envaridator.register('SOME_OTHER_VARIABLE', toi.required().and(toi.str.is())), // SHOULD PASS
}

try {
  envaridator.validate();
} catch (err) {
  console.log(err.message);
}

```

This would result in a 'status report' which would look like this:

```
The following environment variables are invalid:

WRONG_URL - Invalid protocol: https:
V@R1ABL3 - value is null or undefined
```

Contributing
------------

TODO

LICENSE
-------

TODO
