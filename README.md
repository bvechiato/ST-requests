# st-requests
Uses endpoints given in the [ST 2023-24 Sample Project](https://git.ecdf.ed.ac.uk/s2119967/stsample-2023-2024). Adapted from lecture on the 26th of September.

For payloads, Body > raw Json 

## table of contents
- [st-requests](#st-requests)
  - [table of contents](#table-of-contents)
  - [authorisation](#authorisation)
  - [user - all](#user---all)
    - [(POST) localhost:3000/users/register](#post-localhost3000usersregister)
    - [(POST) localhost:3000/users/login](#post-localhost3000userslogin)
    - [(GET) localhost:3000/user](#get-localhost3000user)
    - [(PUT) localhost:3000/user](#put-localhost3000user)
  - [user - admin](#user---admin)
    - [(GET) localhost:3000/users](#get-localhost3000users)
    - [(GET) localhost:3000/users/:userID](#get-localhost3000usersuserid)
    - [(DELETE) localhost:3000/user/:userID](#delete-localhost3000useruserid)

## authorisation
1. [log user in](#post-localhost3000userslogin), get accessToken from their response
2. in Authorization, 
   1. add Bearer Token 
   2. add user's token

## user - all
### (POST) localhost:3000/users/register

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

### (POST) localhost:3000/users/login

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

### (GET) localhost:3000/user
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

### (PUT) localhost:3000/user
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
all require [authorisation](#authorisation)
### (GET) localhost:3000/users 
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


### (GET) localhost:3000/users/:userID

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

### (DELETE) localhost:3000/user/:userID

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
