# tutorial-mean-0001

Angular 6 – MEAN Stack Crash Course – Part 1: Front-end Project Setup And Routing
    Installing Node.js & Angular CLI
        $ npm install -g @angular/cli

    Setting Up The Angular 6 Project With Angular CLI
        $ ng new frontend
        $ cd frontend
        $ ng serve --open

        Adding Angular Material
           $ ng add @angular/material

    Creating Components
        $ ng g c components/list
        $ ng g c components/create
        $ ng g c components/edit
    
    Adding Routing

Angular 6 – MEAN Stack Crash Course – Part 2: Implementing The Back-end
    Setting Up The Back-end Project
        $ mkdir backend
        $ cd backend
        $ npm init -y

        Setting Up Babel
            $ npm install --save-dev babel-cli babel-preset-env

            Once installed you need to add a .babelrc file to the project and add the content:
            {
            "presets": ["env"]
            }
        
        Installing babel-watch
            $ npm install babel-watch --save-dev

            Then use babel-watch in your package.json file in scripts section like this:

            "scripts": {
                "dev": "babel-watch server.js"
            }
        
        Later on, when we’ve implemented the server in file server.js this will enable us to start the server by using the following command:
            $ npm run dev

        Installing Express
            Because we’d like to use the Express framework for implementing the server we’re installing the corresponding package first:
            $ npm install express

            Furthermore we need to make sure that the mongoose library is available, so that we’re able to use that library to access the MongoDB database:
            $ npm install mongoose

            Furthermore we’re installing the cors package:
            $ npm install cors            

    Implementing The Server
        Implementing A Basics Node.js/Express Server
            To be able to implement the server we first need to add a new file server.js to our project:
                $ touch server.js

            Start the server process by executing
                $ npm run dev

            If you’re now accessing http://localhost:4000 in the brower you should be able to see the output:
                "Hello World!"
        
        Extending The Server Implementation
            Now that we have a first basic Node.js/Express server implemented and running we can continue to extend the server implementation further. The Node.js server should act as the back-end of our MEAN stack application and therefore needs to offer various end points. The following use cases must be covered:

            Process an HTTP GET request to retrieve a list of all issues from the MongoDB database
            Process an HTTP GET request to retrieve a single issue by ID from the MongoDB database
            Process an HTTP POST request to insert a new issue in the MongoDB database
            Process an HTTP POST request to update an existing issue entry in the MongoDB database
            Process an HTTP GET request to delete an existing issue entry from the database

            In the first step let’s extend the server implementation in server.js with the following code:

            Because we’ll be dealing with Issue objects, let’s first add a data model for that entity to our application by using the Mongoose library. Create a new folder inside the back-end project named models and create a new file Issue.js to insert the following code:

            By using this code, we’re creating a new Mongoose model for the issue entity with the following properties included:
                title
                responsible
                description
                severity
                status

            The model is created by defining a new Schema first and then creating the model by using the mongoose.model method. The model is exported, so that we’re able to import it again in server.js by adding the following import statement:

        Retrieving All Issues
            We’ve already created the Express Router instance which is available via router. To configure and implement the various endpoints of the server we can now make use of Router’s route method. First let’s add the /issues route by inserting the following piece of code into server.js:

            By calling the get method on the returned Route object we’re registering an event handler function which is called each time a HTTP GET request is send to this route. Inside that function we’re making use of the Mongoose model Issue to call the find method to retrieve all issues available in the database. We’re passing in another function which is invoked once the result from the database is available.

            If an error has occurred (the err parameter is available) the error is printed on the console. If no error has occurred the list of issues is returned in JSON format.        

        Retrieving An Issue By ID
            The next route which is added is /issues/:id. This route is used to send a HTTP GET request to retrieve a single issue from the database in JSON format. Part of that route is the id parameter. This parameter is used to specify which issue entry should be returned. Add the following piece of code to the implementation in server.js:

            To retrieve a single entry via the Mongoose model we’re using the method findById. The value from the id route parameter can be accessed via req.params.id and is passed as the first parameter to findById.

        Adding New Issues
            The route which is used to add new issues via an HTTP post request is /issues/add. With the data submitted in the request body (available via req.body) we’re creating a new Issue object. The save method from the model class is then used to store this new Issue object in the database.

        Updating Issues
            The code which needs to added to update existing issues by sending an HTTP Post request to route /issues/update/:id can be seen in the following listing:

            First the findById method is used to retrieve the issue which should be updated from the database. Once the issue object is available the data fields are updated with the values included in the request body. The update issue object is then stored in the database by calling method save.

        Deleting Issues
            Next, let’s add the route /issues/delete/:id to delete an existing issue entry from the database:

            To delete an entry by it’s identifier the method findByIdAndRemove is used. This method is expecting to get two parameters:

            As the first parameter an object is passed in containing the property _id. As the value we’re assigning the identifier of the issue which should be deleted. This value is available via the route parameter id, so that we’re able to access it via req.params.id.
            The second parameter is a call-back method which is invoked once the request to delete the entry from the database has been processed.

        Finally, let’s take a look at the complete code in server.js:

    Setting Up MongoDB
        Install MongoDB
            Separate process.

        Using Robo 3T
            Robo 3T (formerly Robomongo) is the free lightweight GUI for MongoDB enthusiasts.
    
    Starting The Node.js Server
        Finally we’re ready to start up the Node.js server by entering the following command within the backend directory:
            $ npm run dev

    Testing The Server With Postman
        Postman is a powerful HTTP client for testing web services.

Angular 6 – MEAN Stack Crash Course – Part 3: Connecting Front-end To Back-end
    Adding A Service
        $ cd frontend

        $ ng g s Issue

    Implementing IssueService
        Retrieving Issues
        Adding Issues
        Updating Issues
        Deleting Issues

        In the following listing you can find the complete code of file issue.service.ts once again:                    

    Injecting The Service
        Because the three components which have been created in our Angular application already should use the service to access the back-end we need to inject IssueServices in ListComponent, EditComponent and CreateComponent. First of all add the following import statements to files list.component.ts, edit.component.ts and create.component.ts.

        Then we can use dependency injection to make an instance of the service class available within our components:

Angular 6 – MEAN Stack Crash Course – Part 4: Completing The User Interface
    Including Material Design Components
        We’ve already started to use Material Design components from the Angular Material library in the first part of this series (like the MatToolbar component). In this part we’re going to continue using Material Design components to build the user interface of the sample application. In order to make all needed Material Design components available the following imports needs to be added in app.module.ts:

        Having added the needed imports we need to make sure to add the modules to the array which is assigned to the imports property of the @NgModule decorator as well:

    Creating A Data Model
        Because we’re dealing with Issue data in all our components we’re introducing the Issue interface in issue.model.ts:

        export interface Issue {
            id: String;
            title: String;
            responsible: String;
            description: String;
            severity: String;
            status: String;
        }    

    Creating A Data Model

    Implementing ListComponent
        Implementing The Component Class
            In list.component.ts we need to add the following code:

            Here we’re making sure that Issue and IssueService are imported and that IssueService is injected into the component by adding a corresponding constructor parameter. Three methods are added to the class:

            fetchIssue: This method is used to retrieve the complete list of issues to display. The retrieval is done by calling the IssueService method getIssue and subscribing to the Observable which is returned. The array of issues is stored in class member issues.
            editIssue: The editIssue method is used as the event handler method for the click event of the edit link which is included in the output. In this case the navigate method of the Router service is used to navigate to the edit route. Furthermore editIssue is expecting to get the ID of the issue to edit as a parameter in order to hand it over to the edit route.
            deleteIssue: The deleteIssue event handler method is connected to the click event of the delete link. The ID of the current issue is passed into the method as a parameter and used to call the deleteIssue service method to delete the issue from the database. Once the issue has been deleted (we’re subscribing to the Observable which is returned from the call of the service method) we’re calling fetchIssue again to make sure that the list of issue is retrieved again from the back-end.

        Implementing The Component Template
            Next let’s take a look at the corresponding template implementation in list.component.html. The following code needs to be inserted:

    Implementing CreateComponent
        Next let’s take a look at the code which needs to be used for CreateComponent.

        Implementing The Component Class
            In create.component.ts the following code needs to be inserted:

            Here we’re using Angular’s FormBuilder service to create a a form which can then be used to enter issue data. FormBuilder, FormGroup and Validators is imported from the @angular/forms library.

            To make sure that you can use this import statement in create.component.ts you also need to make sure to add ReactiveFormsModule in app.module.ts. Add the following import statement on top of the file:            

            And make sure to add ReactiveFormsModule to the imports array as well.

            We’re using CreateComponent’s constructor to use the FormBuilder service to create a form group by using method group and passing in a form configuration object.

            Furthermore the addIssue event handler method is implemented which is called when the user clicks on the Save button. In this case the addIssue method from IssueService is used to save the new issue in the database.

        Implementing The Component Template
            The template code of CreateComponent needs to be inserted in create.component.html. The code can be seen in the following listing:

            The createForm FormGroup is attached to a <form> element and the corresponding form input controls are added as well. For each input control we need to make sure to

            add the formControlName attribute and assign the name of the FormGroup element as a string
            add a template variable
            The click event handler of the submit button is bound to the addIssue event handler method. The current values of the input controls are passed into the addIssue call by using the previously assigned template variables of those control elements.

            The output of CreateComponent should look like what you can see in the following screenshot:

    Implementing EditComponent
        Finally, let’s complete the implementation of EditComponent.

        Implementing The Component Class
            The code which needs to be added in edit.component.ts to complete the implementation of the EditComponent class can be seen in the following:

            Again a form is needed and Angular’s FormBuilder service is used to create a new FormGroup. The code which is needed to create the FormGroup is encapsulated in method createForm. This method is then called within the class constructor to make sure that the Form is initiated when the component is created.

            Because we want to use this form for editing an existing issue entry we need to set the input field values accordingly. This is done in the ngOnInit lifecycle method. Inside ngOnInit we’re using the IssueService method getIssueById to retrieve the issue which has been passed into the component by using route parameters. One the issue record is available we’re setting updateForm control values to the corresponding issue values.

            Furthermore the updateIssue event handler method is added. Every time the user clicks on the Save button of the form this method is invoked. Inside this method we’re calling the updateIssue method of IssueService. If the issue has been updated successfully we’re using the MatSnackBar service to output a confirmation message.

        Implementing The Component Template
            ast but not least the corresponding template code to be inserted into edit.component.html:

    Conclusion
        In this final part of the Angular 6 – The MEAN Stack Crash Course series we’ve completed the front-end application by fully implementing the previously created components in our Angular 6 project.

        By the end of this series you should now have a practical and profound understanding of the MEAN stack and it’s building blocks. Throughout this four part series you’ve learned how to build an Angular 6 CRUD application from scratch with MongoDB, Express, Node.js, and Material Design UI.

Quellen
Part 1: Front-end Project Setup And Routing
https://codingthesmartway.com/angular-6-mean-stack-crash-course-part-1-front-end-project-setup-and-routing/
https://youtu.be/x2_bcCZg8vQ
https://github.com/seeschweiler/mean-stack-angular-6-part-1

Part 2: Implementing The Back-end
https://codingthesmartway.com/angular-6-mean-stack-crash-course-part-2-implementing-the-back-end/
https://youtu.be/a30flH_q5-A
https://github.com/seeschweiler/mean-stack-angular-6-part-2

Part 3: Connecting Front-end To Back-end
https://codingthesmartway.com/angular-6-mean-stack-crash-course-part-3-connecting-front-end-to-back-end/
https://youtu.be/HTqghYMRrtA
https://github.com/seeschweiler/mean-stack-angular-6-part-3

Part 4: Completing The User Interface
https://codingthesmartway.com/angular-6-mean-stack-crash-course-part-4-completing-the-user-interface/
https://youtu.be/PhIhNU5MLqo
https://github.com/seeschweiler/mean-stack-angular-6-part-4