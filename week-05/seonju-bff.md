# BFF(Backend For Frontend)

## What is BFF(Backend For Frontend)?

For example, imagine a bank that offers both banking and insurance to its customers.

If we were tasked with developing a customer self-service portal for the bank, the portal would need to call three different systems (CRM/Bank/Insurance), with three different protocols, and three different security schemes, just to get a basic overview of a customer’s engagement with the bank.

A common way to overcome this imperfect setup is to create a new backend in front of the real backend(s) and then design the perfect API for the frontend.

- one of several types of microservice architecture patterns
- consist of multiple backends developed to address the needs of respective frontend frameworks, like desktop, browser, and native-mobile apps
- provide an additional layer between microservices and each client type separately

### Public API Implementation
![image](https://github.com/dev-presenter/English-presentations/assets/52817735/135dec67-83cc-4118-b4c7-653161bd53cd)

- General purpose API backend takes multiple responsibilities.
- It is usually complex and not easy to maintain solution.

### BFF Implementation
![image](https://github.com/dev-presenter/English-presentations/assets/52817735/31855b70-6217-47a7-916b-b0f7aeabebf2)

- BFF facilitates each frontend application interface to directly fetch the data from the backend system’s respective microservice.
- It provides dedicated and optimized solutions per each frontend client.

→ By Implementing BFF we try to keep the frontend decoupled from the backend.

## When to use a BFF?

- When the application depends on microservices and consumes many external APIs and other services.
- When the application needs to develop an optimized backend for a specific frontend interface.
- When clients need to consume data that require a significant amount of aggregation on the backend.

→ It is better to use a BFF to streamline the data flow and introduce a lot of efficiency.

## Benefits of BFF

- **Separation of concerns** — Frontend requirements will be separated from the backend concerns. This is easier for maintenance.
- **Easier to maintain and modify APIs** — The client application will know less about your APIs’ structure, which will make it more resilient to changes in those APIs.
- **Better error handling in the frontend** — Server errors are meaningless to the frontend user most of the time. Instead of directly returning the error server sends, the BFF can map out errors that need to be shown to the user. This will improve the user experience.
- **Multiple device types can call the backend in parallel** — While the browser is making a request to the browser BFF, the mobile devices can do the same. It will help obtain responses from the services faster.
- **Better security** — Certain sensitive information can be hidden, and unnecessary data to the frontend can be omitted when sending back a response to the frontend. The abstraction will make it harder for attackers to target the application.
- **Shared team ownership of components** — Different parts of the application can be handled by different teams very easily. Frontend teams get to enjoy ownership of both their client application and its underlying resource consumption layer; leading to high development velocities.

## References
- [Backend for frontend (BFF) pattern— why do you need to know it?](https://medium.com/mobilepeople/backend-for-frontend-pattern-why-you-need-to-know-it-46f94ce420b0)

- [Why “Backend For Frontend” Application Architecture? - mobileLIVE](https://www.mobilelive.ca/blog/why-backend-for-frontend-application-architecture)

- [BFF - Backend for Frontend Design Pattern with Next.js](https://dev.to/adelhamad/bff-backend-for-frontend-design-pattern-with-nextjs-3od0)

- [The Backends for Frontends (BFF) Pattern](https://kennethlange.com/backends-for-frontends-pattern/)

- [The BFF Pattern (Backend for Frontend): An Introduction](https://blog.bitsrc.io/bff-pattern-backend-for-frontend-an-introduction-e4fa965128bf)
