# Basic mass assignment attack

Mass assignment attacks happens when user are allowed to input values they where not intended to input, 
and therefore can control values in the application.

The mitigation for this attack is proper input validation, and not directly using dto's in the code. 
A DTO have a single purpose, which is to represent the data that is being transferred. Once the data is received,
the values of the dto should be used to initialize an object that uphold invariance. This acts as a decouple, and 
ensure that data is not accidentally received or dispatched(creating excessive data exposure) from the application.

## prerequisites
Prior to starting these exercise, you should have completed the exercises in [basic injection](8_Basic_Injection_Attacks.md)

## Changing the status of an order.
1. In the crAPI website create an order (E.g. buy a wheel)
2. Go to the past order menu.
3. click `order details` on a order and capture the request.
4. Send the captured request and inspect the `Allow` header in the response, which http methods are allowed?
5. In the response body notice the status value
6. Change the request method to `PUT`, and set the content body of the request to:
```
{
    "status":"returned"
}
``` 
7. Send the request, and notice the status of the order.

Besides being a mass assigment vulnerability. This is also BFLA and generally a broken access control vulnerability,
seing as a customer can change the state of an order.

