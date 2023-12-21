Uses endpoints given in the [ST 2023-24 Sample Project](https://git.ecdf.ed.ac.uk/s2119967/stsample-2023-2024). Adapted from lecture on the 26th of September.

For payloads, Body > raw Json 

## table of contents
- [table of contents](#table-of-contents)
- [authorisation](#authorisation)
- [user - all](#user---all)
  - [register](#register)
  - [login](#login)
  - [get user details](#get-user-details)
  - [update user details](#update-user-details)
- [user - admin](#user---admin)
  - [get all users](#get-all-users)
  - [get user by userID](#get-user-by-userid)
  - [delete user by userID](#delete-user-by-userid)
- [order - all](#order---all)
  - [place order](#place-order)
  - [get order by orderID](#get-order-by-orderid)
  - [get all orders](#get-all-orders)
  - [update order by orderID](#update-order-by-orderid)
  - [delete order by orderID](#delete-order-by-orderid)
- [order - admin](#order---admin)
  - [get orders by userID](#get-orders-by-userid)
- [models](#models)
  - [user](#user)
  - [order](#order)

## authorisation
1. [log user in](#post-localhost3000userslogin), get accessToken from their response
2. in Authorization, 
   1. add Bearer Token 
   2. add user's token

## user - all
### register

```
    [POST] localhost:3000/users/register
    {
        "name": "Test User",
        "address": "1234 Street Edinburgh",
        "email": "test@test.com",
        "password": "12345678",
        // "role": "Admin"/"User" by default
    }
```

### login

```
    [POST] localhost:3000/users/login
    {
        "email": "test@test.com",
        "password": "12345678"
    }

    -> {
        "user": {
            "id": "658465d4fbd054c275c88a2",
            "email": "test@test.com",
            "role": "User",
            "name": "Test User"
        },
        "message": "Login successfull",
        "accessToken": "[some token]"
    }
```

### get user details
requires [authorisation](#authorisation)

```
    [GET] localhost:3000/user

    -> {
        "_id": "658465d4fbd054c275c88a2b",
        "name": "Test User",
        "email": "test@test.com",
        "password": "$2b$08$Lwo6LhVtxIuU7qqjDNTQvuC810fq2hd6D3DOkyo627Da.Rs5NO0a6",
        "address": "1234 Street Edinburgh",
        "role": "User",
        "__v": 0
    }
```

### update user details
requires [authorisation](#authorisation)

```
    [PUT] localhost:3000/user
    {
        "password": "new password"
    }


    -> {
        "_id": "658465d4fbd054c275c88a2b",
        "name": "Test User",
        "email": "test@test.com",
        "password": "new password",
        "address": "1234 Street Edinburgh",
        "role": "User",
        "__v": 0
    }
```

## user - admin
all require [authorisation](#authorisation) as admin
### get all users
```
    [GET] localhost:3000/users 

    -> [ 
        {
            "_id": "658465d4fbd054c275c88a2b",
            "name": "Test User",
            "email": "test@test.com",
            "password": "$2b$08$Lwo6LhVtxIuU7qqjDNTQvuC810fq2hd6D3DOkyo627Da.Rs5NO0a6",
            "address": "1234 Street Edinburgh",
            "role": "User",
            "__v": 0
        },
        {
            "_id": "6584666efbd054c275c88a31",
            "name": "Test Admin",
            "email": "admin@test.com",
            "password": "$2b$08$n2O5h/9NBHYBy7Sd6YJH7ugdEsKYvqsHmOWM5ZdRGSJeh/Q4NmegC",
            "address": "1234 Street Edinburgh",
            "role": "Admin",
            "__v": 0
        }
    ]
```


### get user by userID

```
    [GET] localhost:3000/users/658465d4fbd054c275c88a2b

    -> {
        "_id": "658465d4fbd054c275c88a2b",
        "name": "Test User",
        "email": "test@test.com",
        "password": "$2b$08$Lwo6LhVtxIuU7qqjDNTQvuC810fq2hd6D3DOkyo627Da.Rs5NO0a6",
        "address": "1234 Street Edinburgh",
        "role": "User",
        "__v": 0
    }
```

### delete user by userID

```
    [DELETE] localhost:3000/user/6584665efbd054c275c88a2e
    
    -> {
        "_id": "6584665efbd054c275c88a2e",
        "name": "Test User1",
        "email": "test1@test.com",
        "password": "$2b$08$.yrNBnmlVwniExMjtCbB/uA8WJQECJJPUAgsCHw7kJh8X4jl1g2IK",
        "address": "1234 Street Edinburgh",
        "role": "User",
        "__v": 0
    }
```

## order - all
### place order
requires [authorisation](#authorisation)

admins cannot make orders for other users, only themselves

```
    [POST] localhost:3000/order
    {
        "type": "Box2",
        "description": "Secret"
    }  
    // if type isn't specified, Box1 is default


    -> {
        "type": "Box2",
        "description": "Secret",
        "_id": "65847e8eece3f2440b7d0a9e",
        "user": "65847dceece3f2440b7d0a9a",
        "__v": 0
    }
```

### get order by orderID
requires [authorisation](#authorisation) 

admins can see every order, users can only see an order by ID if they made it

```
    [GET] localhost:3000/order/65847e8eece3f2440b7d0a9e

    -> {
        "_id": "65847e8eece3f2440b7d0a9e",
        "type": "Box2",
        "description": "Secret",
        "user": "65847dceece3f2440b7d0a9a",
        "__v": 0
    }
```

### get all orders
requires [authorisation](#authorisation)


```
    [GET] localhost:3000/orders

    -> [
        {
            "_id": "65847e8eece3f2440b7d0a9e",
            "type": "Box2",
            "description": "Secret",
            "user": "65847dceece3f2440b7d0a9a",
            "__v": 0
        }
    ]
```

### update order by orderID
requires [authorisation](#authorisation)

updates existing order, admins can update other users' orders

```
    [PUT] localhost:3000/order
    {
        "_id": "65847e8eece3f2440b7d0a9e",
        "description": "More secretive"
    }

    -> {
        "_id": "65847e8eece3f2440b7d0a9e",
        "type": "Box2",
        "description": "More secretive",
        "user": "65847dceece3f2440b7d0a9a",
        "__v": 0
    }

```

### delete order by orderID
requires [authorisation](#authorisation) 

admins can delete every order, users can only delete an order if they made it

```
    [DELETE] localhost:3000/order/65847e8eece3f2440b7d0a9e

    -> {
        "_id": "65847e8eece3f2440b7d0a9e",
        "type": "Box2",
        "description": "More secretive",
        "user": "65847dceece3f2440b7d0a9a",
        "__v": 0
    }
```


## order - admin
### get orders by userID
requires [authorisation](#authorisation) as admin

```
    [GET] localhost:3000/orders/user/65847dceece3f2440b7d0a9a

    -> [
        {
            "_id": "65847e8eece3f2440b7d0a9e",
            "type": "Box2",
            "description": "More secretive",
            "user": "65847dceece3f2440b7d0a9a",
            "__v": 0
        }
    ]

```

## models
### user
    name:
        string 
        required

    email:
        string
        required
        unique
        lowercase
        valid email

    password:
        string
        required
    
    address:
        string
        required

    role:
        string
        "User" default or "Admin"
        required

### order
    type:
        string
        "Box1" default or "Box2"
        not required
    
    description:
        string
        not required
    
intentionally let not required for DB testing purposes