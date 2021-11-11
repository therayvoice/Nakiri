# Nakiri
Create circular JSON objects from .json files using a built-in unix style referencing scheme.

# Usage
You can import and use the library using vanilla javascript:

```
const nakiri = require('nakiri');

const data = nakiri.parse({
  name: 'Nakiri',
  fav_word: '${{/name}}'
});

console.log(data);
// logs: { name: 'Nakiri', fav_word: 'Nakiri' }

```

You can also determine a symless path for a cleaner outlook, just pass the path string as a second argument to `nakiri.parse()`:

```
const nakiri = require('nakiri');

const data = nakiri.parse({
  name: 'Nakiri',
  info: { age: '1 month' },
  last_commit: '${{age}}'
}, '~/info/');

console.log(data);
// logs: { name: 'Nakiri', info: { age: '1 month' }, last_commit: '1 month' }

```

You can also determine a custom path seprator like `.` by passing it as the third argument (beta):

```
const nakiri = require('nakiri');

const data = nakiri.parse({
  name: 'Nakiri',
  info: { age: '1 month' },
  last_commit: '${{age}}'
}, '~.info.', '.');

console.log(data);
// logs: { name: 'Nakiri', info: { age: '1 month' }, last_commit: '1 month' }

```

You can use a simpler ``mapEntries()``` method to map an object's keys and values. E.g.  

```
const data4 = {
  name: 'David',
  age: 22,
  employement: {
    title: 'Sr Manager',
    pay: '60k'
  }
}

console.log(nakiri.mapEntries(data4, entry => [entry[0]+1, entry[1]+2]));

console.log(nakiri.mapEntries(data4, entry => [entry[0]+1, (typeof(entry[1]) === 'object' && !Array.isArray(entry[1]) && entry[1] !== null ? nakiri.mapEntries(entry[1], _entry => [_entry[0]+1, _entry[1]+2]):entry[1]+2)]));

```

This method is simpler if you don't want a lot of control over your iterations. Also, the recursive example is very verbose (even after refactoring).


Symbolic Legend
===============

1. /          = root of Object
2. ~          = first object below root relative to the current position
3. EMPTY      = binded to whatever needed in stringify and parse (root is DEFAULT)
4. .          = current object // beta
5. ..         = parent object // beta
6. SYMBOL*    = key of SYMBOL // comming soon in v2.0 

### Path examples

Following are a few path examples
```
/person/name
~/name
```

### Features

1. *nakiri.parse(_OBJ, _SYMLESS, _SEPRATOR)* is used to parse a circular object.
2. *nakiri.stringify()* is used to stringify a circular object. (comming soon)
3. *nakiri.mapObjectRecursively(_DEPTH, _OBJ, _keyModFunction, _valueModFunction, _metaData)* is used to recurcively map an object.

> If not sure use ```0``` as **_DEPTH**.  
> **_OBJ** is the object you want to map.  
> If not sure use ```(_key, _metadata) => _key``` as **_keyModFunction**. The **_metadata** argument is an object with information about the object passed, including the passed object itself. E.g. ```_metadata.PARENT``` will return the object presently iterating.  
> If not sure use ```(_val, _metadata) => _val``` as **_valueModFunction**. The **_metadata** argument is an object with information about the object passed, including the passed object itself. E.g. ```_metadata.KEY``` the key of the value. ```_metadata.PIGGYBACK``` can access properties of the *_metaData* argument if the argument goes like ```{ PIGGYBACK: { /* stuff here */ } }```.  
> If not sure use ```{}``` as **_metadata**. Use this argument to provide extra data to a pirticular node.  

4. *nakiri.mapEntries(_obj, _mapFunction)* is used to map entries of an object. The *_mapFunction* argument must be a valid javascript function that returns a 2-D array, in which each member is a valid entry, e.g. ```entry => ['key', 'value']```.  

More documentation comming soon, if you have questions shoot them at *rayvanet@gmail.com* and I will get to it ASAP.

# LICENSE
MIT License

Copyright (c) 2021 Ray Voice

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

