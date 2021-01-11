# Questioner Laminas App

## Introduction

Laminas project have MVC structure, and it will come with the basic configuration and minimum requirements to build an app. Our app will be Questioner App that have questions with their corresponding choices and based on the number of correct answer we will give the points. But first lets install the app. 

## Goals
- To be able to create laminas-mvc application
- How to create Form and render it to the view.
- Create a database and interact with it.
- Add authentication feature to the app.

## Installation using Composer

The easiest way to create a new Laminas MVC project is to use
[Composer](https://getcomposer.org/). If you don't have it already installed,
then please install as per the [documentation](https://getcomposer.org/doc/00-intro.md).

The command below will ask to install different modules, but for our app all we need id laminas-db, laminas-form. And select the default module configuration file (i.e. module.config.php in the config directory) for installation. 

To create your new Laminas MVC project:

```bash
$ composer create-project -s dev laminas/laminas-mvc-skeleton path/to/install
```

Once installed, you can test it out immediately using PHP's built-in web server:

```bash
$ cd path/to/install
$ composer serve 
# OR 
$ php -S 0.0.0.0:8080 -t public
# OR use the composer alias:
$ composer run --timeout 0 serve
```

This will start the cli-server on port 8080, and bind it to all network
interfaces. You can then visit the site at http://localhost:8080/
- which will bring up Laminas MVC Skeleton welcome page.

## Steps To Follow

> You can refer to this [Laminas Tutorial](https://docs.laminas.dev/tutorials/getting-started/skeleton-application/). Every thing is described in there. I will point out some points below regarding our app.

### Step 1 :-
- Create `Questioner` directory in the module, this is where our app will be created. And then create its subdirectories `config, src` and `view`.
- In `src` create `Controller, Form` and `Model` directories and a `Module.php` class and implement `ConfigProviderInterface` to the Module.php class.
- In the `config` will create `module.config.php` to configure the `routing` and `view manager` and `connection b/n routes and its respective Actions and Controller`. see [how config file looks like](https://docs.laminas.dev/tutorials/getting-started/routing-and-controllers/) 
- Don't forget to include the config/module.config.php file into `Module.php` class.
```phpt
public function getConfig()
    {
        return include __DIR__ . '/../config/module.config.php';
    }
```
- Let the Project know your app, so add `"Questioner\\": "module/Questioner/src/"` next to the Application in `composer.json` file of the project. 
```phpt
"autoload": {
        "psr-4": {
            "Application\\": "module/Application/src/",
            "Questioner\\": "module/Questioner/src/"
        }
    };
```
And then run `composer dump-autoload` in your terminal. <br>
Add `Questioner` to the configuration file of the whole project in `config/modules.config.php` not the config in your app, this is the where all the modules in the project configured.

### Step 2 :-
- Create `QuestionController` and extend it to AbstractActionController. Always add `Action` as suffix to the name of the methods in a controller.
```phpt
public function indexAction()
    {
        return new ViewModel([
            'message' => 'Congratulation!!!, it is working',
        ]);
    }
```
- In `view` create a folder and its name has to be namespace of the app which is `questioner` with small letter. Inside the question folder create another folder and give it the controller name which is `question` and also create a file and give it name what ever your action is in the controller without the suffix Action. <br>
  To check if the app configured correctly `var_dump(message);` in `view/questioner/question/index.phtml` and add question route to your url `http://0.0.0.0:8080/question`. <br>
  if it doesn't show the message, look in the `module.config.php` in module/Questioner/config if it is configured correctly. You can also ask me if you want.

- Then create `Question.php` class in Model with `question, first_choice second_choice`, `third_choice` and `correct_answer` parameters.
- Create `QuestionForm.php` class in Form and extend it to `Form` module from Laminas\Form. see more [how to create Form](https://docs.laminas.dev/tutorials/getting-started/forms-and-actions/), but first install it if you don't install it during installation to the project with `composer require laminas/laminas-form` command in the terminal. The form will have different inputs corresponding to Question object parameters.
- Add new method in the QuestionController that return a QuestionForm instance to its corresponding file in the view, this is to create the question from the user side. 

### Step 3 :- 
- When the form is submitted, instantiate the Question class with the value of the form.
- if you don't install laminas-db during project installation, install it by running `composer require laminas/laminas-db` in the terminal.
- #### To can interact with the database in two ways :-
- 1. To configure our database in the `config/autoload/global.php` as described in this tutorial.
  2. To use the `Adapter` module directly in your app to configure the database. see [Adapter doc here](https://docs.laminas.dev/laminas-db/adapter/)
  
- If you choose the first one go as pre the tutorial, but for the second one instantiate `TableGateway` with the first parameter `table_name` and the second one is the `adapter` you instantiate in the previous step. Using the $tableGateway instance, we can insert, delete, select and update our data. So using the tableGateway insert the values you get from the form and save it in the database.

### The final step :-
- Create another controller `CorrectionController`, and configure it in your Question/config/module.config.php file. Then create a method that return all the question that you created from the database and render it to the corresponding view file.
- Also create a form that submit what has been answered and accordingly give point to the one taking the exam.

## extras
- Using [authentication](https://docs.laminas.dev/laminas-authentication/intro/) give access to the teacher to create questions, but the student can only answer and see the result.
- make it look good using bootstrap. 

