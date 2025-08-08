# 🔍 Endpoint Analysis with Postman

## 🧠 Learning Objectives
- Understand the role of Swagger/OpenAPI in documenting APIs.
- Use Swagger documentation to discover API behavior and validate backend constraints.
- Reiterate skills in building Postman collections through exploration or proxying.
- Recognize potential risks in exposing internal API documentation.

---

## ⚙️ Prerequisites
You should have completed the [Postman exercise with crAPI](../crAPI/4_Endpoint_Analysis_with_postman.md) and exercises in [3 Active Reconnaissance](3_Active_reconnaissance.md), especially Exercise 4.

---

## 1 - Exploring Swagger Documentation for Juice Shop

In [3 Active Reconnaissance - Exercise 4](3_Active_reconnaissance.md), you discovered the endpoint `/api-docs/swagger.json`. This is a Swagger endpoint, built around the [OpenAPI Specification](https://swagger.io/docs/specification/about/), used to describe and document APIs.

Visit the Swagger JSON endpoint to understand how Juice Shop’s API is structured.

### 🧪 Task
- Explore the endpoint `/api-docs/swagger.json`. What endpoints are documented?
- What parameters are required? Are there default values?
- Does the `quantity` field have backend validation?
- Try to use this documentation to manually construct a valid request using Postman.
- Does the documentation mention authentication requirements? Is it reflected in actual API behavior?

> 🧠 *Reflection*: Could this Swagger documentation itself be considered an information disclosure risk? How might it help an attacker understand the internal structure of the application?

---

## 2 - Build a Juice Shop Postman Collection

Just like in [crAPI - Endpoint Analysis with Postman](../crAPI/4_Endpoint_Analysis_with_postman.md), you will now construct your own request collection for Juice Shop.

You can build the collection:
- Manually (based on the Swagger documentation), or
- By proxying browser traffic through Postman.

Experiment to see which method gives you the best insight or coverage.

> 🛠️ *Advanced Tip*: Tools like [Postman](https://www.postman.com/) or [Insomnia](https://insomnia.rest/) support importing Swagger/OpenAPI files directly.

> 🔍 *Bonus Task*: Try sending requests outside what’s specified in the Swagger docs. Does the API still respond? Any surprises?

> 🔁 *Reminder*: This is a repetition task from crAPI. The goal is to deepen your comfort with API Postman and exploratory testing techniques.

---

## 🧠 Reflection Questions

- How does Swagger documentation improve developer experience—and how might it help attackers?
- What API security concerns can arise from public exposure of Swagger docs?
- Was your manual Postman collection consistent with the API spec? Why or why not?
