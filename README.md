
## Basic Workflow

* signup as a new user
* login by setting authorization header
* create a new order
* add a couple of items

### Authentication

Signup as a new user and copy both the `id` and `token` for later use.

```graphql
mutation signup {
  signupUser(email: "testuser@163.com", 
    password: "password") {
    id
    token
  }
}
```

Login system with the user which you created just now

```graphql
mutation login {
  authenticateUser(email: "testuser@163.com", password: "password") {
    token
  }
}

}
```
Setting the `Authorization` to `Bearer <token>` which you got it from login response, your requests are authenticated. You can use this query to obtain the authenticated user id:

```graphql
query loggedInUser {
  loggedInUser {
    id
  }
}
```

### Checkout Process

First, Check a order for the Login user. Use the id which you got from 'loggedInUser' to replace the <userId>

```graphql
mutation {
  createOrder(userId: "<userId>",
  description:"description sample") {
    id
  }
}
```
View all of the products in system

```graphql
{
  allProducts{
    id
    description
    price
  }
}
```

Add products in to the order, user order id to replace `<orderID>` and specify an amount:

```graphql
mutation {
  setOrderItem(orderId:"<orderID>",
  productId:"cjhhttmcwjs8r016011018o5v",
  amount:2) {
    itemId
    amount
  }
}
```

To get all orders of a specific user, you can use this query, use the login user id to replace '<userId>'

```graphql
{
  allOrders(filter: {user: {id: "<userId>"}}) {
    description
    orderStatus
    items {
      amount
      product {
        name
      }
    }
  }
}

```
