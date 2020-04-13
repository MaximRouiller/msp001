# Demos

## Demo 1 - Hello World

* `mkdir myapp`
* `cd myapp`
* `npm init`
  - everything by default except the entrypoint which will be `app.js`
* `code .` to open VS Code on that folder
* Explore what `package.json` is. Mainly mention the entrypoint which is `main: app.js`
* Show how to open the terminal in VSCode (`View > Terminal`)
* To handle the webserver, we're going to install `express`
* In the terminal window, run the command `npm install express --save` while explaining what it is doing.
* Notice that `package.json` now has a dependency section on the latest version of `express`
* Create the file `app.js`
* Type in/snippet/copy/paste the following content. Always make sure to explain what each lines are doing as you are typing them.

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => res.send('Hello world'));
app.listen(port, () => console.log('Application started'));
```

* In the terminal window, run the command `node app.js` to run the application .
* Open up the browser and navigate to http://localhost:3000 to view our hello world show up on screen.

## Demo 2 NodeJS API

We start with the code from the previous demo.

* `npm install -g express-generator`
* `express --no-view`
* Go over the `package.json` and show the different packages
* `npm install`, explain why since we scaffolded and new dependencies are present
* Go through the `app.js` file and explain what's going on
  * Line 1-4: Module import
  * Line 6-7: Importing our routers
  * Line 9: Similar as before
  * Line 11-15: configuring middleware (JSON support, logger, cookie parsing, static file)
  * Line 17-18: mapping our router to a url
  * Line 20: module export that is required 
* Run the code again with F5/`node app.js` and go to localhost:3000
* Navigate to `/users`. Open up the Chromium Debugger tools using F12.
  * Notice how the content mime type of "/users/" is returned (text/plain within the headers).
* Let's go back to the code. Look at how the different routes are mapped together (userRouter and app.use('…', router) usage specifically).
* Let's edit /routes/users.js as it will be our new API. At line 6, replace `res.send(….)` content with an actual object (eg: {name:'Hello world'}     )
* Restart the application and look again at the Content-Type header for `application/json`. We now have an API.
* Whatever we do in that "get" controller to gather data, etc. will be accessible from /users/. It's up to us to customize and add more routes to handle things.

## Demo 3 - Pushing to Azure

### Moving our code to GitHub

* Create a Node `.gitignore` file from https://github.com/github/gitignore within our project
* Create a new repository on GitHub to host our project (any name) using https://github.com/new
* Follow instructions to add our repository to this project
* Ensure that all the code has been pushed to the repository

### Creating the Azure Resources

From the Azure Portal (https://portal.azure.com)

* Click on "Create a resource"
* Click WebApps in the list
* Create a new Resource Group named `myapp`
* Select a Web App name (`msp001` in the video)
* Publish mode is `Code`
* Runtime stack is Node 12 LTS
* Operating System is Linux
* Region is `East US`
* Linux Plan:
  * Create a new one named myapp
  * Change size to F1 (under Dev/Test)
* Click on Review + Create
* Click on Create

While this is deploying, talk about the current Deployment window that is being seen. Go through the Overview, Inputs, and Template.

Deployment should take no more than 1-2 minutes. Once everything is deployed, click on the button "Go to resource".

Explore the App Service Overview page.
* URL where things are deployed.
* Current App Service Plan
* Application stats
* Configuration -> this is where you set environment variable for your deployed app only. If an app require specific configurations, they should be located there. Within Configuration, under the "General settings", you can change your tech stack, platform settings, etc.

This should give you all the necessary basic tools to manage your first cloud application.

Once we've gone over everything, click the URL on the overview page. This takes us to a default page.

Let's fix that.

### Deploying with GitHub Actions

* Go to our newly created repository.
* Click on the "Actions" tab
* Select "Deploy Node.js to Azure Web App" option by clicking "Set up this workflow"
* Ensure that `AZURE_WEBAPP_NAME` has the name of the AppService we chose earlier
* Ensure that `NODE_VERSION` is `12.x`
* Take note of `AZURE_WEBAPP_PUBLISH_PROFILE` in the comments of that file.
* Make sure to change the trigger code to the below one:
```yml
on:
  push:
    branches:
      - master
```
* Go back to our resource in Azure and download the publishing profile by clicking "Get publish profile"
  * Open it in Code
* Right click on Settings > Open in new tab
  * Click on secrets
  * Click add a new secret
    * Name: `AZURE_WEBAPP_PUBLISH_PROFILE`
    * Value: Copy the whole content of the publishing profile we opened earlier
  * Click the button "Add secret"
* Click on "Start commit" then "Commit new file"
* Wait until the Action has finished executing (<1 minute)
* Refresh the default page of our application.

You should see "Express / Welcome to Express" message that is the default for newly created Express application.

To prove this works, let's change the title on the `index.html` page and publish that change again to GitHub.
