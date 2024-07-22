Passport is *authentication* middleware for **Node.js**. Extremely flexible and modular, Passport can be unobtrusively dropped in to any Express-based web application.

---
# Passport-jwt
```js
const { ExtractJwt, JwtStrategy } = require('passport-jwt')

// jwt_payload : {identifier: userId}
let opts = {}
opts.jwtFromRequest = ExtractJwt.fromAuthHeaderAsBearerToken()
opts.secretOrKey = 'secret'
passport.use(new JwtStrategy(opts, function(jwt_payload, done){
    User.findOne({id: jwt_payload.sub}, function(err, user){
        if(err)
            return done(err, false)
        if(user)
            return done(null, user)
        else
            return done(null, false)
    })
}))
```

1) `opts` Object: An empty JavaScript object ({}) called opts is created to hold options for configuring Passport's JWT strategy.

2) `opts.jwtFromRequest`: This line sets the jwtFromRequest property of the opts object. It configures how Passport should extract the JWT token from incoming requests. In this case, it uses the ExtractJwt.fromAuthHeaderAsBearerToken() method to *extract the token from the "Authorization" header of the request*. This is a common way to send JWT tokens.

3) `opts.secretOrKey`: This line sets the secretOrKey property of the opts object. It specifies the secret key used to verify the JWT token's signature. In this example, the secret key is set to the string 'secret'.
4) `passport.use`: Passport's use method is used to configure a new authentication strategy. In this case, it's setting up the JWT strategy.

5) `JwtStrategy`: This strategy is provided by the passport-jwt package and is used to authenticate users based on JWT tokens.

6) `function(jwt_payload, done)`: This is the callback function that gets executed when a request is made with a JWT token. It takes two parameters:

7) `jwt_payload`: This parameter contains the data extracted from the JWT token.
8) `done`: This is a callback function that Passport uses to indicate whether authentication succeeded (done(null, user)) or failed (done(err)).
9) `User.findOne`: Inside the callback function, it attempts to find a user in the database based on the id (jwt_payload.sub) extracted from the JWT token. It uses the User model (presumably defined elsewhere in your code).

**Callbacks** for `User.findOne`: Depending on the outcome of the User.findOne query:
- If there's an error (err), it calls done(err, false) to indicate that authentication failed with an error.
- If a user is found (user), it calls done(null, user) to indicate that authentication succeeded with the found user.
- If no user is found, it calls done(null, false) to indicate that authentication failed because no user matching the token was found.

In summary, this code **sets up Passport.js to use JWT-based authentication with a secret key**. When a request with a JWT token is received, it tries to find a user based on the token's content and signals whether authentication succeeded or failed using the done callback.





