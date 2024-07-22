```js
const express = require('express')
const User = require('./models/User')
const router = express.Router()
const bcrypt = require('bcrypt')

router.post('/register', async (req, res) => {
    // This function will handle the register user logic

    // Step 1: Get details from req.body
    const {firstName, lastName, email, password} = req.body
    if (!firstName || !email || !password) {
        res.status(400).json({err: 'Invalid request body'})
    }

    // Step 2: Check if user with that email already exists or not
    const exisitingUser = await User.findOne({email: email})
    if (exisitingUser) {
        res.status(402).json({err: 'User with this email already exists'})
    }
    
    // Step 3: Create the user if above condition satisfies
    const hash_pass = await bcrypt.hash(password, 10)
    const newUserDetails = {firstName, lastName, email, password: hash_pass}
    const newUser = await User.create(newUserDetails)

	// Step 4: Create JWT and return the token to the user
    const token = await getToken(email, newUser)
    const userReturn = {...newUser.toJSON(), token}
    delete userReturn.password
    return res.status(200)
})
```

`router.post('/register', async (req, res) => { ... })`: This code defines a POST request handler for the /register route. It listens for incoming POST requests to register new users.
Request Body Parsing:

`const { firstName, lastName, email, password } = req.body`: This line extracts the firstName, lastName, email, and password fields from the request's body. These fields should be sent as part of the POST request.

`if (!firstName || !email || !password) { ... }`: This condition checks if any of the required fields (firstName, email, password) are missing in the request body. If any field is missing, it responds with a 400 Bad Request status and an error message.

`const existingUser = await User.findOne({ email: email })`: It attempts to find a user in the database with the same email address provided in the request. If a user with the same email exists, it sets existingUser to that user's data.
User Existence Check:

`if (existingUser) { ... }`: If existingUser is not null, it means a user with the same email already exists in the database. In this case, it responds with a 402 status (Payment Required - you might want to use a different status code) and an error message.

`const hash_pass = await bcrypt.hash(password, 10)`: This line uses bcrypt to hash the user's password. Hashing is a security measure to store passwords securely in the database. The 10 indicates the number of rounds of hashing.

`const newUserDetails = { firstName, lastName, email, password: hash_pass }`: It creates an object newUserDetails with the hashed password.

`const newUser = await User.create(newUserDetails)`: It attempts to create a new user in the database using the User.create method. If successful, the newUser object contains the newly created user's data.

`const token = await getToken(email, newUser)`: This line calls the getToken function from the "helper.js" file to generate a JWT. It passes the email and newUser object as arguments. The getToken function will return a JWT based on the user's email and identifier (usually the user's unique ID).

`const userReturn = {...newUser.toJSON(), token}`: It creates a new object userReturn by spreading the properties of the newUser object (presumably containing user data). It also adds a property token to this object, which contains the JWT generated in the previous step.

`delete userReturn.password`: This line removes the password property from the userReturn object for security reasons. You typically don't want to send the user's password back to the client.

`return res.status(200).json(userReturn)`: Finally, it sends a JSON response to the client with a status code of 200 (OK).

`bcrypt.compare(pass1, pass2)`: Compares two passwords.