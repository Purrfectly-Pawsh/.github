<details><summary>
  <h2>Architecture</h2>
</summary>

### Overview
<a href="https://www.figma.com/design/ygpxw9N4RthlImTPQV1o1w/Wireframes%2FArchitecture?node-id=0-1&amp;t=D919mb47Zm3Rcqf0-0">Architecture diagram, Wireframes, Use Case diagram</a>

### Interservice communication
- One of the goals when designing applications with the microservice architecture is to make the whole system responsive instead of declarative. This means that each service should receive the needed information for a certain action and act uppon it instead of being requested and returning the needed result. <br><br>
- We have implemented this with the help of RabbitMQ utilizing exchanges and queues in different ways.<br><br>
  - For example when the customer adds a product to the basket we deal with the call in the product service and send all product details to the basket service via the queue instead of letting the basket service request all the information for a certain product. <br><br>
  - Further we used the message queue to trigger the same event in multiple subscribers. In our case when the checkout has been successfully completed the checkout service sends an event with the order details and the <code>basketId</code>. Thus the order service receives all the information needed to be stored and the basket can remove the purchased basket from its database. We have achieved this by using a fanout exchange and attaching different queues to it.

### External API/Service
- We incorporated Stripe as an external service into our application and used it for the checkout process and also for retrieving an invoice for the customer.

### IAM
- For identity and access management we chose Keycloak and have created a customer and an admin user which have different privileges.

### Gateway
- In order to prevent each service from having to talk to keycloak for verifying tokens and securing its endpoints, we have included the Spring Cloud Gateway which is a central API for our frontend client and thus routes the requests to the coresponding service and also takes care of security in a single service. By using the gateway services can scale and easily and have multiple instances running on different ports without having to adjust the frontend client.


### Infrastructure
- The whole setup is meant to be run with docker compose. For development purposes each microservice has its own docker compose file which makes working on the service a bit easier. 
- There is also a deployment docker compose file which pulls all services from <a href="https://hub.docker.com/orgs/purrfectlypawsh24/repositories">dockerhub</a> and hooks them up each to its own database.
</details>

 

<details>
  <summary>
  <h2>Project Management</h2>
</summary>

### Jira
- We have decided on using Jira as a ticketing system. We mostly used it as a kanban board where we pushed tickets through different stages and introduced new tickets in a flexible manner according to our current weekly workload. <br></br>
- 
</details>

 

<details><summary>
  <h2>
    Contribution 
    </h2>
</summary>

### Branches should be named by the following convention:

- feature/{Jira ticket nr}
- fix/{Jira ticket nr}
- this gives us a quick hint what the change does (fix/feature) and where to find more information about it (Jira Ticket Nr.)

### Commit messages should look like this:

- {Jira ticket nr} - {short description of the user story}
- helps us figure out what kind of change has been implemented when looking at the git history
</details>
