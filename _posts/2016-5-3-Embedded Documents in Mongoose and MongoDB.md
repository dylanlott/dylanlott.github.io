---
layout: post
title: Embedded Models in Mongoose
---

### Quick tip: Notes on using embedded models in Mongoose 

Recently I was working on a project where I needed to use embedded documents in Mongoose.
When I first tried this, this is what my code looked like. 

````
var Part = require('./Part'); 
var Build = require('./Build'); 

var Build = new Schema({
	parts: [Part]; 
})
````
This didn't work, and I didn't know why. I kept getting a `Cast Error` on the parts property. 

### Accessing Schemas vs Accessing Models 
After messing around for a bit, I stumbled across [this article on Stack Overflow][1] and I realized that in my code I 
was accessing the model, _not_ the schema. 

[1]:http://stackoverflow.com/questions/32752897/adding-child-document-to-existing-mongodb-document

If you want to embed a document in another document of a mongoose model, then you need to access the _schema_. 

To do this, you need to make the change as follows: 

````
var Part = required('./Part').schema; 
var Build = require('./Build'); 

var Build = new Schema({
	parts: [Part]
})
````

And that should work for you!


# But what if I want to use Populate? 

`populate` is the method that Mongoose gives you to create relational data. It's powerful if you use it right, and it can give your data great depth.



Sources: 
[2]:http://stackoverflow.com/questions/14199529/mongoose-find-modify-save

