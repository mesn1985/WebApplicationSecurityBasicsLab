#Basic BFLAS 

# Explorer the API.
During the api enumeration, the URL http://localhost:3000/api/Products
was discovered. You should explorer this URL from POSTMAN.
How does it react to the methods GET, POST and PUT?

Can you perform any action you where not intended to, such as adding or
editing resources?

One method call might give you some information about the structure of the requests.

# Add product
From postman you should be able to add a product, if you use the right 
HTTP Method. You might also need some authorization. But remember BFLA's is
about user being able to execute functions on the api, which they where never intended to.

Remember to use the right Content-Type header for the data format.

# Update a product
Using the right method, you might be able to update a product.
If you are able to change it, can you test the applications lexical input validation?
And how about the semantics, can you change price to something that does not make sense,
like negativ values?

_Hint: Resource locators are often placed in the URL_