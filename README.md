# boilerplate-code-notes

# connection.js 
const path = require("path");
const {Sequelize, DataTypes, Model} = require("sequelize");

const db = new Sequelize({
  dialect: "sqlite",
  storage: path.join(__dirname, "db.sqlite"),
});

module.exports = db;

// DO NOT EDIT CODE BELOW

module.exports = {
  db,
  Model,
  DataTypes,
};

# Here is a boilerplate way to create a db instance.  It will create a table called Product.  It will be stored in the db.sqlite we created in connection.js
const { Sequelize, DataTypes } = require("sequelize");
const db = require("../db/connection.js");

const Product = db.define("product", {
  name: DataTypes.STRING,
  description: DataTypes.STRING,
  price: DataTypes.INTEGER,
});

module.exports = Product;

const { db, DataTypes, Model } = require('./db/connection.js');

// const Product = db.define('Product', {});
  
class Product extends Model {};
Product.init({
    name: DataTypes.STRING,
    description: DataTypes.STRING,
    price: DataTypes.INTEGER
}, {
    sequelize: db,
    modelName: 'Product'
});

// DO NOT EDIT CODE BELOW
module.exports = Product;

# This is exporting the methods that you create in the methods folder using a methods.index.js

const Product = require("./Product.js");

module.exports = Product;

# This is potential boilerplate code for how to initiate the database that you created, adding and updating the data before you initialize it.  The database is synced using db.sync, and then after the tables are set up, you can add your data.  Chat gpt wants me to use a try, catch block for this btw.  
#!/bin/node
const Product = require("./model/index.js");
const db = require("./db/connection.js");
const initialize = async () => {
  // Uncomment the line below to run any debugging, but make sure to comment it out before runnning the tests.
//   await db.sync({force: true});

  // Create products
  let allProducts;
  try {
      await Product.create({
          name: 'Wireless Earbuds',
          description: 'True wireless earbuds with noise canceling technology',
          price: 149.99,
      }),
      await Product.create({
          name: 'Smartphone',
          description: 'A high-quality smartphone with advanced features',
          price: 799.99
      }),
      await Product.create({
          name: 'Laptop',
          description: 'A powerful laptop with high-end specifications',
          price: 1499.99
      });

      allProducts = await Product.findAll();
    //   console.log(allProducts)
  // Find item with ID of 2
  let foundItem = await Product.findByPk(2);
//   await foundItem.update({name: 'iPhone'}); 
    // console.log(foundItem)
  // Update item with ID of 2
  let updatedItem;
  updatedItem = await foundItem.update({name: 'iPhone'});
    // console.log(foundItem);
  // Delete item with ID of 2
  let deletedProduct;
  // Write your code here
deletedProduct = await foundItem.destroy();
// DO NOT EDIT
  return {
    allProducts,
    updatedItem,
    deletedProduct,
  };
} catch (error) {
    console.log('Error creating the database:' , error);
}
// DO NOT EDIT  

};

// Uncomment the line below to run any debugging, but make sure to comment it out before you run the tests!
// initialize()

// DO NOT EDIT CODE BELOW
module.exports = { initialize };

Key Points
Most Sequelize CRUD methods return a Promise, so we need to await the values in order to perform the CRUD operations.
The .create() method creates a new instance (row) of our model. It accepts an object with key/value pairs as an argument.

await User.create({"name": "Johnny", "password": "abcd1234"});
await User.create({"name": "Maureen", "password": "password123"});
The findAll() method returns all values in a model (table)

const findAll = await User.findAll();
The findByPk() method returns the instance that matches the corresponding id.

const foundUser = await User.findByPk(2); 
// Returns Maureen instance
The update() method can be called on an instance and can be used to update a value formatted as an object.

const foundUser = await User.findByPk(2); 
const updatedUser = await foundUser.update({
    password: "helloWorld!"
});
Entries can be deleted using the destroy() method. This can be called on the instance or the model.

const foundUser = await User.findByPk(2); 
// Deleting using the instance
await foundUser.destroy();

// Deleting via model
User.destroy({
    where: {
        name: "Maureen"
    }
})

Here we have another example.  hardcoded data came from the json file, so we passed it in.  

#!/bin/node
const Rocket = require("./models/index.js");
const { db } = require("./db/connection.js");
const rocket = require("./rocket.json");

async function initialize() {
//   await db.sync({force: true});
let createdRocket; 
  try {// = YOUR CODE HERE
createdRocket = await Rocket.create(rocket)
  const firstRocketVersion = {
    name: createdRocket && createdRocket.name,
    difficultyLevel: createdRocket && createdRocket.difficultyLevel,
  };

  let foundRockets; // = YOUR CODE HERE
foundRockets = await Rocket.findAll();

  let updatedRocket; // = YOUR CODE HERE
updatedRocket = await createdRocket.update({name: 'High Flyer', difficultyLevel: 5});
  const firstUpdate = {
    name: updatedRocket && updatedRocket.name,
    difficultyLevel: updatedRocket && updatedRocket.difficultyLevel,
  };

  let deletedRocket; // = YOUR CODE HERE
    deletedRocket = await updatedRocket.destroy(); 

// DO NOT EDIT BELOW
// console.log(firstRocketVersion);
  return {
    createdRocket: firstRocketVersion,
    foundRockets,
    updatedRocket: firstUpdate,
    deletedRocket,
  };
  } catch (error) {
      console.log('Error creating the database', error);
  }
}
// Uncomment the line below to run any debugging
// initialize()

// DO NOT EDIT BELOW
module.exports = initialize;