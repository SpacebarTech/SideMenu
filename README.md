
To start your new project, follow these steps

#### 1. Clone this repository

``` git clone https://github.com/SpacebarTech/node-module-template.git ```

#### 2. Install all the packages

``` npm install ```

#### 3. Create a new repository for your project

Do this on github, or send Tim a request to have it done.

#### 4. Remove template.git from your project

``` git remote remove origin ```

#### 5. Add your repository to your project

``` git remote add origin https://github.com/SpacebarTech/SideMenu.git ```

#### 6. Update your package.json so that all fields are filled out correctly

In your package.json, make sure that "name," "description," "repository," "bugs," and "homepage" are all the correct values for your repository, and not for template.git


#### 7. Update webpack.config.js

Change [MODULE_NAME] to the name you want

#### 8. Push up changes
``` git add . ```\
``` git commit -m “Update from template” ```\
``` git push origin master```


# Set up semantic release

``` semantic-release-cli setup ```

Follow prompt.\
Pick Travis CI\
Do not make a new travis.yml file\

# Publish module
``` git add . ```\
``` git-cz ``` Follow the prompts\
``` git push origin master ```


# Update README.md
  1. Delete instructions
  2. use template below

SideMenu

## Installation

  `npm install @spacebartech/sidemenu`

## Usage

## Tests

`npm test`
