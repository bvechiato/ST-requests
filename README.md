# ST-requests


## User - all
### localhost:3000/users/register

```json
    [POST] localhost:3000/users/register
    {
        "name": "Test User",
        "address": "1234 Street Edinburgh",
        "email": "test@test.com",
        "password": "12345678",
        // "role": "Admin"/"User" by default
    }
```

### localhost:3000/users/login

```json
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

### localhost:3000/user
For it to work:
1. localhost:3000/users/login user, get accessToken from their login response
2. in Authorization, 
   1. add Bearer Token 
   2. add user's token

```json
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

### localhost:3000/user (change info)
For it to work:
1. localhost:3000/users/login user, get accessToken from their login response
2. in Authorization, 
   1. add Bearer Token 
   2. add user's token

```json
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

## User - admin
For all to work:
1. localhost:3000/users/login admin, get accessToken from their login response
2. in Authorization, 
   1. add Bearer Token 
   2. add admin's token

### localhost:3000/users 
```json
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


### localhost:3000/users/:userID

```json
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

### localhost:3000/user/:userID


```json
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
