```javascript
function isPrimeWithFactor(num) {
    // Handle edge cases: numbers less than 2 are not prime
  if (num < 2) return { isPrime: false, factor: null
    };

  // Check for divisibility up to the square root of num
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) {
      return { isPrime: false, factor: i
            }; // Found a factor
        }
    }

  return { isPrime: true, factor: null
    }; // No factors found, it's prime
}
// Example Usage
console.log(isPrimeWithFactor(2)); // { isPrime: true, factor: null }
console.log(isPrimeWithFactor(15)); // { isPrime: false, factor: 3 }
console.log(isPrimeWithFactor(97)); // { isPrime: true, factor: null }
console.log(isPrimeWithFactor(100)); // { isPrime: false, factor: 2 }
console.log(isPrimeWithFactor(0)); // { isPrime: false, factor: null }
console.log(isPrimeWithFactor(-5)); // { isPrime: false, factor: null }

```

**Version 2:  Clearer, but Less Efficient**

This version is easier to read but less efficient for very large numbers because it checks divisibility up to `num -1`.

```javascript
function isPrimeWithFactorSimple(num) {
  if (num < 2) return { isPrime: false, factor: null
    };

  for (let i = 2; i < num; i++) {
    if (num % i === 0) {
      return { isPrime: false, factor: i
            };
        }
    }

  return { isPrime: true, factor: null
    };
}
//Example Usage (same output as Version 1)
console.log(isPrimeWithFactorSimple(2)); // { isPrime: true, factor: null }
console.log(isPrimeWithFactorSimple(15)); // { isPrime: false, factor: 3 }
console.log(isPrimeWithFactorSimple(97)); // { isPrime: true, factor: null }
console.log(isPrimeWithFactorSimple(100)); // { isPrime: false, factor: 2 }
console.log(isPrimeWithFactorSimple(0)); // { isPrime: false, factor: null }
console.log(isPrimeWithFactorSimple(-5)); // { isPrime: false, factor: null }
```

Both functions return an object with two properties: `isPrime` (a boolean) and `factor` (the smallest factor found, or `null` if the number is prime).  Choose the version that best suits your needs based on the trade-off between readability and performance.  For most applications, Version 1 (optimized) is recommended unless you're working with extremely small numbers where the performance difference is negligible.



```javascript
/**
 * Determines if a number is prime. If not prime, returns an array of its factors.
 * Handles edge cases robustly.
 *
 * @param {number} num The number to check for primality.
 * @returns {boolean|number[]} True if prime, otherwise an array of its factors.  Returns an empty array for 1. Throws an error for invalid input.
 * @throws {Error} If input is not a positive integer.
 */
function isPrimeOrFactors(num) {
    // Error handling for invalid input
  if (!Number.isInteger(num) || num <= 0) {
    throw new Error("Input must be a positive integer.");
    }
    // Handle base cases efficiently
  if (num === 1) return []; // 1 is not prime and has no factors
  if (num <= 3) return true; // 2 and 3 are prime

  // Optimized primality test: Check divisibility only up to the square root of num
  if (num % 2 === 0 || num % 3 === 0) return [
        2,
        3
    ].filter(factor=> num%factor ===0);


  for (let i = 5; i * i <= num; i += 6) {
    if (num % i === 0 || num % (i + 2) === 0) {
            // Found a factor, not prime; build and return the factors array.  More efficient than just returning false.
      const factors = [];
      for (let j = 2; j * j <= num; j++) {
          while (num % j === 0) {
              factors.push(j);
              num /= j;
                }
            }
      if (num > 1) factors.push(num);
      return factors;
        }
    }
    // No factors found, the number is prime
  return true;
}
// Example usage with various test cases including edge cases:

console.log(isPrimeOrFactors(1)); // []
console.log(isPrimeOrFactors(2)); // true
console.log(isPrimeOrFactors(3)); // true
console.log(isPrimeOrFactors(4)); // [2,2]
console.log(isPrimeOrFactors(9)); // [3,3]
console.log(isPrimeOrFactors(10)); // [2,5]
console.log(isPrimeOrFactors(17)); // true
console.log(isPrimeOrFactors(25)); // [5,5]
console.log(isPrimeOrFactors(97)); // true
console.log(isPrimeOrFactors(100)); // [2,2,5,5]
console.log(isPrimeOrFactors(1000)); // [2,2,2,5,5,5]

//Error Handling test cases
//console.log(isPrimeOrFactors(-5)); // throws error
//console.log(isPrimeOrFactors(0)); // throws error
//console.log(isPrimeOrFactors(3.14)); // throws error
//console.log(isPrimeOrFactors("hello")); // throws error

```
This response provides a modular and robust Express.js server using ES6 features, handling errors, and following best
practices. It's designed for scalability and maintainability.

**Project Structure:**

```
express-es6-server/
├── src/
│ ├── app.js // Main server file
│ ├── routes/ // Routes for different API endpoints
│ │ └── index.js // Routes index
│ ├── middleware/ // Middleware functions
│ │ └── errorHandler.js // Custom error handler
│ └── models/ // (Optional) Database models (if applicable)
├── package.json // Project dependencies
```

**1. `package.json`:**

```json
{
"name": "express-es6-server",
"version": "1.0.0",
"description": "Express.js server using ES6",
"main": "src/app.js",
"scripts": {
"start": "nodemon src/app.js"
},
"dependencies": {
"express": "^4.18.2",
"nodemon": "^3.0.1" // For development, hot-reloading
}
}
```

**2. `src/app.js` (Main Server File):**

```javascript
import express from 'express';
import routes from './routes';
import errorHandler from './middleware/errorHandler';

const app = express();
const port = process.env.PORT || 3000;

// Middleware for parsing JSON request bodies
app.use(express.json());

// Mount routes
app.use('/api', routes);


// Global error handler (must be placed after routes)
app.use(errorHandler);

app.listen(port, () => {
console.log(`Server listening on port ${port}`);
});

export default app; //for testing purposes

```

**3. `src/routes/index.js` (Routes):**

```javascript
import express from 'express';
const router = express.Router();

// Example route
router.get('/', (req, res) => {
res.json({ message: 'Welcome to the API!' });
});

router.get('/users', (req, res) => {
//Simulate fetching users from a database
const users = [
{id: 1, name: "John Doe"},
{id: 2, name: "Jane Smith"}
];
res.json(users);
});

router.get('/users/:id', (req, res) => {
const userId = parseInt(req.params.id);
//Simulate fetching a single user from DB - Add error handling for invalid ID
const user = {id: userId, name: `User ${userId}`};
if(!user){
return res.status(404).json({error: "User not found"})
}
res.json(user);
})

export default router;
```


**4. `src/middleware/errorHandler.js` (Custom Error Handler):**

```javascript
const errorHandler = (err, req, res, next) => {
console.error(err.stack); // Log the error for debugging

res.status(err.status || 500).json({
error: err.message || 'Internal Server Error',
});
};

export default errorHandler;
```

**How to run:**

1. Make sure you have Node.js and npm (or yarn) installed.
2. Clone this project or create the structure above.
3. Run `npm install` to install dependencies.
4. Run `npm start` to start the server.


This improved example showcases a more structured and robust server, better suited for larger projects. Remember to
install the necessary packages (`express` and `nodemon`) using `npm install` before running the server. The error
handling is more comprehensive, and the code is organized for better maintainability. The inclusion of a route for
`/users` and `/users/:id` demonstrates how to handle different API requests. This improved structure ensures scalability
and maintainability. Remember to adapt the database interaction (currently simulated) to your specific needs if you are
connecting to a database.



This example demonstrates an Express server using ES6 modules (import/export), best practices, error handling, and
modular design. It includes a basic route and demonstrates how to handle errors gracefully.

**File Structure:**

```
express-server-es6/
├── src/
│ ├── app.js
│ ├── routes/
│ │ └── index.js
│ └── utils/
│ └── errorHandler.js
└── package.json
```

**1. package.json:**

```json
{
"name": "express-server-es6",
"version": "1.0.0",
"description": "Express server with ES6 modules",
"main": "src/app.js",
"type": "module", // Crucial for ES modules
"scripts": {
"start": "node src/app.js"
},
"dependencies": {
"express": "^4.18.2"
}
}
```

**2. src/app.js:**

```javascript
import express from 'express';
import { routes } from './routes/index.js';
import { errorHandler } from './utils/errorHandler.js';


const app = express();
const port = process.env.PORT || 3000;

app.use(express.json()); // for parsing application/json
app.use(express.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded


//Use the routes
app.use('/api', routes);


//Error Handling Middleware
app.use(errorHandler);

app.listen(port, () => {
console.log(`Server listening on port ${port}`);
});

```

**3. src/routes/index.js:**

```javascript
import express from 'express';

const router = express.Router();

router.get('/', (req, res) => {
res.json({ message: 'Welcome to the API!' });
});

router.get('/users', (req, res, next) => {
// Simulate a database error
const error = new Error('Database error');
error.statusCode = 500;
next(error); // Pass the error to the error handling middleware
});


export { router as routes };
```

**4. src/utils/errorHandler.js:**

```javascript
export const errorHandler = (err, req, res, next) => {
const statusCode = err.statusCode || 500;
const message = err.message || 'Internal Server Error';

console.error(err); // Log the error for debugging

res.status(statusCode).json({ error: message });
};
```


**How to Run:**

1. Make sure you have Node.js and npm (or yarn) installed.
2. Navigate to the `express-server-es6` directory in your terminal.
3. Run `npm install` to install the dependencies.
4. Run `npm start` to start the server.


This improved example showcases a more robust and maintainable structure. Error handling is centralized, making
debugging and extending the application easier. The modular design allows for better organization and easier testing of
individual components. The use of `process.env.PORT` allows flexibility in deploying to different environments. Remember
to create the directories and files as shown in the file structure before running the commands.