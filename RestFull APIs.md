# REST API Interview Questions and Answers

## 1. How are REST APIs Stateless?
**Stateless** means the server does not remember anything about your previous requests. It has no memory. 
* **Common Example:** Imagine calling a big customer service center. Every time you call, a different person answers. Because they don't know you, you have to tell them your account number and your problem from the beginning, *every single time*. 
* **In REST APIs:** Every time your app sends a request to the server, it must include all the information (like a login token or ID) the server needs to understand it. The server will not say, "Oh, I remember you from 5 seconds ago."

## 2. Explain the HTTP Codes: 200, 400, 500
HTTP codes are short messages the server sends back to tell you how the request went.
* **200 (OK):** Everything went perfectly. 
  * *Example:* You asked a waiter for a menu, and they handed it to you. Success!
* **400 (Bad Request):** This is a **Client Error** (your fault). You sent the request wrong, or missed some information.
  * *Example:* You went to a shoe store and ordered a pizza. The store is confused because your request makes no sense.
* **500 (Internal Server Error):** This is a **Server Error** (the server's fault). Your request was fine, but the server crashed or broke while trying to process it.
  * *Example:* You ordered a burger correctly, but the restaurant's oven caught on fire. They can't give you your food because their system broke.

## 3. What is URI, URN, URL?
These are ways to identify things on the internet. Let's use a person as an example.
* **URI (Uniform Resource Identifier):** This is the big umbrella term. It is a way to identify a resource. It can be a name, an address, or both.
* **URN (Uniform Resource Name):** This is the **Name** of the resource. It tells you *what* it is, but not *how to find it*. 
  * *Example:* A book's ISBN number (ISBN-12345), or a person's name ("John Doe").
* **URL (Uniform Resource Locator):** This is the **Address** of the resource. It tells you *how to find it*. 
  * *Example:* `https://www.google.com` or a person's home address ("123 Main Street, New York"). 

## 4. What are some Best Practices for making URIs?
When developers create links for APIs, they should follow rules to keep them clean and easy to read.
* **Use Nouns, not Verbs:** The URI should be the name of the thing, not the action. 
  * *Good:* `/users` 
  * *Bad:* `/getUsers` (The HTTP method handles the action, not the link).
* **Use Plurals:** Always use plural words to keep it consistent.
  * *Good:* `/cars/123`
  * *Bad:* `/car/123`
* **Use lowercase letters:** It is easier to read and prevents errors.
* **Use hyphens (-) instead of underscores (_):** Hyphens are easier to read in web links.

## 5. What are the differences between REST and SOAP?
They are two different ways systems talk to each other over the internet.

| Feature | REST | SOAP |
| :--- | :--- | :--- |
| **What is it?** | An architectural style (a set of guidelines). | A protocol (a strict set of rules). |
| **Data Format** | Uses JSON, plain text, or XML. JSON is very lightweight and fast. | Uses ONLY XML, which is heavy and takes more data. |
| **Complexity** | Simple, flexible, and easy to build. | Very complex, strict, but highly secure. |
| **Example** | Like sending a quick text message to a friend. | Like sending a formal, legally binding letter with a wax seal. |

## 6. What is the difference between REST and AJAX?
They are completely different things that work together to make websites fast.
* **REST:** This is how the **Server** (backend) is designed. It is the kitchen that prepares the data.
* **AJAX:** This is a technique used on the **Web Browser** (frontend). It allows your webpage to fetch data from the REST API *without reloading the whole page*. 
  * *Example:* When you are scrolling on Instagram, new posts keep loading without the screen turning white and refreshing. That is AJAX talking to a REST API!

## 7. What are some tools to develop and test REST APIs?
Developers use these tools every day:
* **For Testing APIs:** **Postman** (very famous), **Insomnia**, or **cURL** (command line). These let you send requests to see if the API works.
* **For Building APIs:** Node.js (Express), Python (Django/Flask), or Java (Spring Boot).
* **For Documenting APIs:** **Swagger** (it creates a nice instruction manual for your API).

## 8. What are some Real-World Examples of REST APIs?
We use them every day without knowing!
* **Weather Apps:** Your phone does not have a thermometer. It uses a REST API to ask a weather server, "What is the temperature like today?" and the server replies with the data.
* **Log in with Google/Facebook:** When you click "Log in with Google" on a new website, that website uses a REST API to ask Google, "Is this person real?"
* **Ride Sharing (Uber/Careem):** They use the Google Maps REST API to calculate the distance and route between you and your driver.

## 9. What are the different HTTP Methods?
These are the action verbs you use to tell the REST API what you want to do. We call this **CRUD** (Create, Read, Update, Delete).
* **GET (Read):** You want to just look at data. *(Example: Show me my bank balance).*
* **POST (Create):** You want to send new data to the server. *(Example: Create a new Facebook post).*
* **PUT (Update):** You want to completely replace/update existing data. *(Example: Updating your entire user profile).*
* **PATCH (Partial Update):** You want to change just one small piece of data. *(Example: Changing only your profile picture).*
* **DELETE (Delete):** You want to remove data. *(Example: Delete a photo from your account).*

## 10. Advantages and Disadvantages of REST APIs
### Advantages:
* **Easy to understand:** It uses standard internet rules (HTTP) that everyone knows.
* **Fast and Lightweight:** Because it mostly uses JSON, it sends data very quickly.
* **Separate Frontend and Backend:** The mobile app team and the server team can work independently without breaking each other's code.

### Disadvantages:
* **Over-fetching or Under-fetching:** Sometimes you ask for a user's name, and the API sends back their name, address, age, phone number, and history (too much data). Or, it sends too little, and you have to make a second request.
* **Statelessness can be heavy:** Because the server forgets you, you have to send your long security token (ID) with every single request, which can take up network bandwidth.
