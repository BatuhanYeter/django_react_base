# django_react_base

> cd Development

`python -m pip install pipenv --upgrade`

> mkdir app_name
> cd app_name

"app_name klasörünün içinde" 
`pipenv shell`
// Successfully created virtual environment!

`pipenv install django`

`django-admin startproject app_name .`


> code .

python manage.py startapp api
python manage.py startapp frontend

pip install djangorestframework #if needed for api

pip list

// Now first, ensure you are in the frontend directory
npm install

//next, create an empty npm project
npm init -y

//still in frontend dir
npm install @babel/core @babel/preset-env @babel/preset-react babel-loader react react-dom react-router-dom webpack webpack-cli

mkdir templates
cd templates
mkdir frontend

// frontend/templates/frontend -> create index.html file
// and then paste this into the index.html:
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Starter template</title>
    {% load static %}
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
    />
    <link rel="stylesheet" type="text/css" href="{% static "css/index.css" %}"
    />
  </head>
  <body>
    <div id="main">
      <div id="app"></div>
    </div>

    <script src="{% static "frontend/main.js" %}"></script>
  </body>
</html>
```


cd ..
cd ..
// frontend/static -> create static folder
mkdir static

cd static
// frontend/static/frontend -> create frontend's static folder
mkdir frontend
mkdir css

// create index.css -> frontend/static/css/index.css

// cd back to main frontend and then 
// frontend/src
mkdir src
//after that, create index.js inside of src
// frontend/src/index.js
// then paste this into index.js:

`import App from "./components/App";`

// next, create components inside of src then add App.js and HomePage.js
cd src
mkdir components

// after creating App.js, paste this into that:
```
import React, { Component } from "react";
import { render } from "react-dom";
import HomePage from "./HomePage";

export default class App extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
        <HomePage />
      </div>
    );
  }
}

const appDiv = document.getElementById("app");
render(<App />, appDiv);
```

// and paste this to Home.js:
```
import React, { Component } from "react";

import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  Redirect,
} from "react-router-dom";

export default class HomePage extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <Router>
        <Switch>
          <Route exact path="/">
            <p>This is the home page</p>
          </Route>
         
        </Switch>
      </Router>
    );
  }
}
```



// Now let's add some configuration files

// frontend/babel.config.json
```
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "node": "10"
        }
      }
    ],
    "@babel/preset-react"
  ],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

// frontend/webpack.config.js
```
const path = require("path");
const webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./static/frontend"),
    filename: "[name].js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  optimization: {
    minimize: true,
  },
  plugins: [
    new webpack.DefinePlugin({
      "process.env": {
        // This has effect on the react lib size
        NODE_ENV: JSON.stringify("development"),
      },
    }),
  ],
};
```



// and finally, edit scripts in package.json file
```
"scripts": {
    
  "dev": "webpack --mode development --watch",
   
  "build": "webpack --mode production"
   
 
  },
```

// All set up react, time to add views and urls for django to interact with react
// frontend/views.py
```

from django.shortcuts import render

def index(request, *args, **kwargs):
    return render(request, 'frontend/index.html')

// frontend/urls.py
from django.urls import path
from .views import index

urlpatterns = [
    path('', index),
  
]

```

// now back to the, django_react_starter/urls.py
```
urlpatterns = [
    path('admin/', admin.site.urls),
    # path('api/', include('api.urls')),
    path('', include('frontend.urls'))
  ]

```

// and remember to add all the apps you installed to add to 
// django_react_starter/settings.py
```

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # add this

    'rest_framework',
    'api',
    'frontend',
    # .....

]
```


// To test the app,
django_react_starter/python manage.py runserver
django_react_starter/frontend/npm run dev

// add .git into your project
// then .gitignore
ignore node_modules
```

*.pyc
/backend/db.sqlite3
.DS_Store
.env
*/settings/local.py
/staticfiles/*
/mediafiles/*
__pycache__/
/.vscode/

# coverage result
.coverage
/coverage/

# pycharm
.idea/

# data
*.dump

# npm
node_modules/
npm-debug.log

# Webpack
/frontend/bundles/*
/frontend/webpack_bundles/*
/webpack-stats.json

# Sass
.sass-cache
*.map

# General
/{{project_name}}-venv/
/venv/
/env/
/output/
/cache/
boilerplate.zip

# Spritesmith
spritesmith-generated/
spritesmith.scss

# templated email
tmp_email/

```







