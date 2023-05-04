## What is a utility function?
A JavaScript utility function is a modular and reusable function that performs a specific task related to general programming needs. It is designed to simplify complex operations into simple and concise code, and improve code readability and maintainability.

A utility function should: 
1. Two or fewer arguments  - Limiting the amount of arguments makes testing easier
2. Do one thing - When a function is isolated to one action, it can be read, refactored, and tested easier.
3. Say what they do - It is very important for debugging that we know what a function does just by the name. For example, the function name `randomInt`, allows the reader to understand exactly what the function does, it returns a random number. What if the function name was called `random`. Random what? It could be a random object, array, integer, boolean, etc.

## How to use utility functions
Other developers might set them up differently, but I put all my utility functions for a project in one file (usually in `/src/utility/utility.js`). 

The file looks something similar to this:

```javascript
function a() {
// function a code
}

function b() {
// function b code
}

function c() {
// function c code
}

const utilityService = { a, b, c };

export default utilityService;
// Don't forget the End of File extra line
```

## Array Functions

### chunk

Splits an array into smaller arrays of a specified size.

```javascript
function chunk(arr, size) {
  return Array.from({ length: Math.ceil(arr.length / size) }, (_, i) =>
    arr.slice(i * size, i * size + size)
  );
}
```

### flatten

Flattens an array of arrays into a single array.

```javascript
function flatten(arr) {
  return arr.reduce((acc, val) => acc.concat(val), []);
}
```

### compact

Removes falsy values from an array.

```javascript
function compact(arr) {
  return arr.filter(Boolean);
}
```

### difference

Returns the difference between two arrays.

```javascript
function difference(arr1, arr2) {
  return arr1.filter((val) => !arr2.includes(val));
}
```

### intersection

Returns the intersection of two arrays.

```javascript
function intersection(arr1, arr2) {
  return arr1.filter((val) => arr2.includes(val));
}
```

## Object Functions

### parseJwt

Decodes and returns object from encoded jwt token

```javascript
function parseJwt(token) {
  if (!token) {
    return {};
  }
  let base64Url = token.split('.')[1];
  let base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
  let jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function (c) {
    return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
  }).join(''));

  return JSON.parse(jsonPayload);
};
```

### pick

Returns a new object with only the specified properties.

```javascript
function pick(obj, keys) {
  return keys.reduce((acc, key) => {
    if (obj.hasOwnProperty(key)) {
      acc[key] = obj[key];
    }
    return acc;
  }, {});
}
```

### omit

Returns a new object with the specified properties removed.

```javascript
function omit(obj, keys) {
  return Object.keys(obj)
    .filter((key) => !keys.includes(key))
    .reduce((acc, key) => {
      acc[key] = obj[key];
      return acc;
    }, {});
}
```

### isEqual

Checks if two objects are equal.

```javascript
function isEqual(obj1, obj2) {
  return JSON.stringify(obj1) === JSON.stringify(obj2);
}
```

### isEmpty

Checks if an object is empty.

```javascript
function isEmpty(obj) {
  return Object.keys(obj).length === 0;
}
```

### deepClone

Returns a deep copy of an object.

```javascript
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```

## String Functions

### capitalize

Capitalizes the first letter of a string.

```javascript
function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}
```

### truncate

Truncates a string to a specified length and adds an ellipsis if it exceeds that length.

```javascript
function truncate(str, len) {
  return str.length > len ? str.slice(0, len) + '...' : str;
}
```

### stripTags

Removes HTML tags from a string.

```javascript
function stripTags(str) {
  return str.replace(/<[^>]*>?/gm, '');
}
```

### slugify

Converts a string to a slug.

```javascript
function slugify(str) {
  return str
    .toLowerCase()
    .replace(/[^a-zA-Z0-9]+/g, '-')
    .replace(/(^-|-$)+/g, '');
}
```

### reverse

Reverses a string.

```javascript
function reverse(str) {
  return str.split('').reverse().join('');
}
```

## Number Functions

### round

Rounds a number to a specified number of decimal places.

```javascript
function round(num, places) {
  return +(Math.round(num + 'e+' + places) + 'e-' + places);
}
```

### randomInt

Generates a random integer between two numbers.

```javascript
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

### factorial

Calculates the factorial of a number.

```javascript
function factorial(num) {
  if (num < 0) return undefined;
  if (num === 0) return 1;
  return num * factorial(num - 1);
}
```

### fibonacci - specific number

Generates the nth number in the Fibonacci sequence.

```javascript
function fibonacci(n) {
  if (n < 1) return 0;
  if (n <= 2) return 1;
  let prev = 1,
    curr = 1;
  for (let i = 3; i <= n; i++) {
    let next = prev + curr;
    prev = curr;
    curr = next;
  }
  return curr;
}
```

### fibonacci - sequence

Generates the fibonacci sequence and stopping at a specific number.

```Javascript
function fibonacciSequence(num) {
	const sequence = [0, 1];
	let i = 2;
		
	while (sequence[i - 1] + sequence[i - 2] <= num) {
		sequence[i] = sequence[i - 1] + sequence[i - 2];
		i++;
	}
	return sequence;
}
```

### isPrime

Checks if a number is prime.

```javascript
function isPrime(num) {
  if (num <= 1) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```

## Date Functions

### formatDate

Formats a date as a string.

```javascript
function formatDate(date) {
  const d = new Date(date);
  const year = d.getFullYear();
  const month = ('0' + (d.getMonth() + 1)).slice(-2);
  const day = ('0' + d.getDate()).slice(-2);
  return `${year}-${month}-${day}`;
}
```

### isLeapYear

Checks if a year is a leap year.

```javascript
function isLeapYear(year) {
  return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0;
}
```

### daysBetween

Calculates the number of days between two dates.

```javascript
function daysBetween(date1, date2) {
  const oneDay = 24 * 60 * 60 * 1000;
  const d1 = new Date(date1);
  const d2 = new Date(date2);
  return Math.round(Math.abs((d1 - d2) / oneDay));
}
```

### age

Calculates a person's age based on their birthdate.

```javascript
function age(date) {
  const d = new Date(date);
  const diff = Date.now() - d.getTime();
  const ageDate = new Date(diff);
  return Math.abs(ageDate.getUTCFullYear() - 1970);
}
```

### dayOfYear

Calculates the day of the year for a given date.

```javascript
function dayOfYear(date) {
  const d = new Date(date);
  const start = new Date(d.getFullYear(), 0, 0);
  const diff = d - start;
  const oneDay = 1000 * 60 * 60 * 24;
  return Math.floor(diff / oneDay);
}
```

## Misc Functions

### debounce

Limits the rate at which a function can be called.

```javascript
function debounce(func, wait) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(this, args);
    }, wait);
  };
}
```

### throttle

Limits the frequency at which a function can be called.

```javascript
function throttle(func, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}
```

### deepClone

Creates a deep clone of an object.

```javascript
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```

### shuffle

Shuffles the elements of an array.

```javascript
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}
```

### flat

Flattens a nested array.

```javascript
function flat(array) {
  return array.reduce(
    (acc, curr) =>
      Array.isArray(curr) ? acc.concat(flat(curr)) : acc.concat(curr),
    []
  );
}
```

## Accessibility functions

### addOutlineOnFocus

Adds an outline to a parent element if the child element has focus

```javascript
function addOutlineOnFocus = (focusElem, parentElem) => {
  focusElem?.addEventListener("focusin", () => {
    if (focusElem?.matches(":focus-visible")) {
      parentElem?.classList?.add("tab-form-outline");
    }
  });

  focusElem?.addEventListener("focusout", () => {
    parentElem?.classList?.remove("tab-form-outline");
  });
};
```

## Cookie Functions

### createCookie

Creates a browser cookie with the inputted key and value

```javascript
function createCookie(key, value) {
  let date = new Date();
  date.setMonth(date.getMonth() + 12);
  document.cookie = `${key}=${value};expires=${date};`;
}
```

### getCookie

Gets the value of an existing cookie based on the name

```javascript
function getCookie(name) {
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) {
    return parts.pop().split(';').shift();
  }
}
```
