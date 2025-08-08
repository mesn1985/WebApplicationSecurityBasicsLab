# Basic BFLA (Broken Function Level Authorization)

## 🧠 Learning Objectives
- Understand the concept of Broken Function Level Authorization (BFLA).
- Explore API endpoints using Postman.
- Identify unauthorized access to protected functions.
- Test input validation for API methods.

## ⚙️ Prerequisites
- Completion of Juice Shop Active Reconnaissance exercises.
- Familiarity with Postman or similar API testing tools.

## Explorer the API
During API enumeration, the URL `http://localhost:3000/api/Products` was discovered. Explore this URL using Postman. How does it react to the HTTP methods GET, POST, and PUT?

Can you perform any actions you were not intended to, such as adding or editing resources?

One method call might give you some information about the structure of the requests.

## Add product
From Postman, you should be able to add a product if you use the right HTTP method. You might also need some authorization. Remember, BFLA vulnerabilities allow users to execute functions on the API which they were never intended to.

Remember to use the right `Content-Type` header for the data format.

## Update a product
Using the right method, you might be able to update a product.

If you are able to change it, test the application's lexical input validation. How about the semantics? Can you change the price to something that does not make sense, like negative values?

_Hint: Resource locators are often placed in the URL._

## 🧠 Reflection Questions
- Were you able to add or update resources without proper authorization?
- What input validation does the API enforce, and where does it fail?
- How could these issues be exploited by an attacker?
- What mitigation strategies would prevent BFLA vulnerabilities?
