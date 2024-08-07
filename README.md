# nod
const mongoose = require("mongoose");
const express = require("express");
const app = express();
const uri = "mongodb+srv://sunainahegnekar:3FufCnQl1SdWT5jR@nodecluster.7ah8d.mongodb.net/?retryWrites=true&w=majority&appName=Nodecluster";

// Connect to MongoDB
mongoose.connect(uri)
    .then(() => console.log("Connected to Mongo Atlas"))
    .catch(err => console.log("Error connecting to MongoDB:", err));

// Define User Schema
const UserSchema = new mongoose.Schema({
    name: String,
    age: Number,
});

const User = mongoose.model("User", UserSchema);

// Route to get a user
app.get('/', async (req, res) => {
    try {
        // Create a user if none exist
        await create();
        // Retrieve the first user from the database
        const user = await User.findOne();
        res.send(user);
    } catch (err) {
        res.status(500).send("Error retrieving user");
    }
});

// Function to create a user
async function create() {
    try {
        const user = new User({ name: "Nobita", age: 21 });
        const result = await user.save();
        console.log("Created user successfully", result);
    } catch (err) {
        console.error("Error creating user:", err);
    }
}

// Start the server
app.listen(3000, () => {
    console.log("Server is running on port 3000");
});
