# App Dev: Storing Application Data in Cloud Datastore v1.1

## Objectives

In this lab, you will learn how to perform the following tasks:

- Harness Cloud Shell as your development environment.

- Integrate Cloud Datastore into a NodeJS application.

## Steps

### 1. Clone source code in Cloud Shell

To clone the repository for the class, execute the following command:

git clone https://github.com/GoogleCloudPlatform/training-data-analyst

### 2. Configure and run the case study application

- Change the working directory, execute the following command:

```shell
cd ~/training-data-analyst/courses/developingapps/nodejs/datastore/start
```

- Export an environment variable, GCLOUD_PROJECT that references the GCP Project ID, execute the following command:

```shell
export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID
```

```comment
GCP Project ID in Cloud Shell

While working in Cloud Shell, you will have access to the 
Project ID in the $DEVSHELL_PROJECT_ID environment variable.
```

- Install the application dependencies, execute the following command:

```shell
npm install
```

```The installation may take a couple of minutes.```

- To run the application, execute the following command: ```npm start```

- In Cloud Shell, click Web preview > Preview on port 8080 to preview the quiz application.

### 3. Create an App Engine application to provision Cloud Datastore

- Stop the application by pressing `Ctrl+C`
- Create an App Engine application in your project, execute the following command in Cloud Shell:

```shell
gcloud app create --region "us-central"
```

```comment
Note: You aren't using App Engine for your web application yet.

However, Cloud Datastore requires you to create an App Engine application in your project.
```

### 4. Import and use the NodeJS Datastore module

- Open the ...gcp/datastore.js file in the Cloud Shell editor.

- Load the config module from the parent folder.

- Load the @google-cloud/datastore module.

- Declare a Datastore client object named ds.

datastore.js

```javascript

// TODO: Load the ../config module

const config = require('../config');

// END TODO

// TODO: Load the @google-cloud/datastore module

const Datastore = require('@google-cloud/datastore');

// END TODO

// TODO: Create a Datastore client object, ds
// The Datastore(...) factory function accepts an options // object which is used to specify which project's
// Datastore should be used via the projectId property.
// The projectId is retrieved from the config module. This // module retrieves the project ID from the GCLOUD_PROJECT // environment variable.

const ds = Datastore({
 projectId: config.get('GCLOUD_PROJECT')
});

// END TODO

Write code to create a Cloud Datastore entity
Declare a constant named kind, initialized with the value 'Question'.

datastore.js
// TODO: Declare a constant named kind
//The Datastore key is the equivalent of a primary key in a // relational database.
// There are two main ways of writing a key:
// 1. Specify the kind, and let Datastore generate a unique //    numeric id
// 2. Specify the kind and a unique string id

const kind = 'Question';

// END TODO

```

- In the create(...) function, remove the existing Promise.resolve({}) placeholder statement from the create(...) function.

- Declare a constant called key to store the key for this entity.

- Declare a constant named entity and initialize it with the key and the quiz question properties extracted from the form data.

- Use the Datastore client object (ds) to save the entity by calling the save(entity) method.

datastore.js

```javascript
// The create({quiz, author, title, answer1, answer2,
// answer3, answer4, correctAnswer}) function uses an
// ECMAScript 2015 destructuring assignment to extract
// properties from the form data passed to the function

function create({ quiz, author, title, answer1, answer2,
                  answer3, answer4, correctAnswer }) {
 // TODO: Declare the entity key,
 // with a Datastore generated id

 const key = ds.key(kind);

 // END TODO

 // TODO: Declare the entity object, with the key and data

 const entity = {
   key,
// The entity's members are represented in a data property.
// This is an array where each element represents one
// member in the entity. Each element is an object with a // name and a value
   data: [
     { name: 'quiz', value: quiz },
     { name: 'author', value: author },
     { name: 'title', value: title },
     { name: 'answer1', value: answer1 },
     { name: 'answer2', value: answer2 },
     { name: 'answer3', value: answer3 },
     { name: 'answer4', value: answer4 },
     { name: 'correctAnswer', value: correctAnswer },
   ]
 };
 // END TODO

// TODO: Save the entity, return a promise
 // The ds.save(...) method returns a Promise to the
 // caller, as it runs asynchronously.

 return ds.save(entity);

 // END TODO
}
```

### 5. Run the application and create a Cloud Datastore entity

- Save the ...gcp/datastore.js file and then return to the Cloud Shell command prompt.

- Start the application, execute the following command:

```shell
npm start
```

- In Cloud Shell, click Web preview > Preview on port 8080 to preview the quiz application.
- Click Create Question.
- Complete the form with the following values, and then click Save.

| Form Field  | Value |
|----------|:-------------:|
|Author| Your Name |
| Quiz | Google Cloud Platform |
|Title| Which company owns GCP? |
|Answer 1| Amazon |
|Answr 2| Google (select the Answer 2 radio button!) |
|Answer 3| IBM |
|Answer 4 | Microsoft |

- You should be returned to the home page for the application.

- To see the list of all operations, run:

```shell
gcloud datastore operations list
```

- You should see your new question!
