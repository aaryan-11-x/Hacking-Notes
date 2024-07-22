```javascript
// Imports express js library
const express = require('express')

// Create express application with app object
const app = express()
app.use(express.json())

app.get('/', (req, res) => {
    res.send('This is Aaryan')
})
app.get('/hello', (req, res) => {
    res.send('This is another route')
})
app.listen(8000, () => console.log('Server running on Port 8000'))
```

`app.get('/', (req, res) => { res.send('This is Aaryan') })`: This code defines a route for the root URL '/'. When a user accesses the root URL of the server (e.g., http://localhost:8000/), the server responds with the text 'This is Aaryan'. This route uses the HTTP GET method.

`app.listen(8000, () => console.log('Server running on Port 8000'))`: This line starts the server and makes it listen on port 8000. When the server starts, it prints a message to the console indicating that the server is running on port 8000.