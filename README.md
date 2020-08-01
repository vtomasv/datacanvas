# Git workflow and branches

Main branches are develop and master. Main difference is
develop has debug settings on and is intended to run locally.
Master has production settings applied and is intended 
as the branch to push to heroku remote for live builds
(demos or final product).

If you are forking this repo, consider that other
branches have incomplete or unsatisfactory solutions.
This repo no longer has any support and remaining
branches are kept for documentation purposes. We
recommend you do not directly use them, but instead
only explore possible strategies or excerpts.

Note: This repo was originally forked from https://bitbucket.org/is2dcc/tablero-digital.

# Run locally

### Previous Requirements
- Install NodeJS (you can reject the prompt asking you to install stuff with Chocolatey)
- Install Python 3.8

### Back-end
It's recommended you always work in a virtual environment (venv) to avoid dependency issues. You can manage this in your favorite IDE or text editor, or manually:
- cd into project base directory.
- Create a virtual environment `py -m venv venv`.
- Activate the environment `.\venv\Scripts\activate` in Windows.

Next:
- Install requirements with pip `pip install -r requirements.txt`.
- Prepare changes made to model `py manage.py makemigrations`.
- Migrate model changes `py manage.py migrate`.
- Run back-end server `py manage.py runserver`.

### Front end

- Go to front-end directory `cd frontend`.
- Install dependencies `npm install`.
- Run server `npm run serve`.

# Getting started with Heroku deployment

Follow the same steps described in [Getting Started](https://devcenter.heroku.com/articles/getting-started-with-python), but basically:

#### Create account

[https://signup.heroku.com/dc](https://signup.heroku.com/dc)

#### Install Heroku Command Line Interface

##### Linux:

````bash
sudo snap install heroku --classic
````

##### Mac:

Link: [https://cli-assets.heroku.com/heroku.pkg](https://cli-assets.heroku.com/heroku.pkg)

````bash
brew install heroku/brew/heroku
````

##### Windows:

32 bit: [https://cli-assets.heroku.com/heroku-x86.exe](https://cli-assets.heroku.com/heroku-x86.exe)

64 bit: [https://cli-assets.heroku.com/heroku-x64.exe](https://cli-assets.heroku.com/heroku-x64.exe)

#### Logging in

````bash
heroku login
````
This opens a browser tab where you enter you credentials, then return to CLI.

# Deploying in Heroku
Make sure you have already cloned project repo into your machine.

You can deploy the project to Heroku as a single application or with two separate apps for frontend and backend. In either case you will have to provision a PostgreSql add-on in Heroku for the database of the app, then the django-heroku library used in the project will take care of the configurations for the connections.


## Deploy with single app

### Create y set app

Log into heroku CLI and `cd` into project root. Now:

````bash
heroku create tablero-digital
````
`tablero-digital` is just an example. App name within Heroku platform must be unique, and may be already taken.

Remember you must be on project directory for Heroku CLI to identify de the specific app automatically (you can have multiple apps so this make sense).

Also notice that creating the app like this will automatically create a new heroku remote among your git remotes (this remote is important for deploying). If you checkout the project to a new location or delete the local project, you will most likely need to add the remote manually, unless you delete your existing app from Heroku platform and redeploy from scratch.

### Builpacks

Install buildpacks. For this you can do it from heroku CLI or from browser.

#### Browser

Log into your Heroku account in Heroku website. Go to Dashboard and then to <your-project-name>. The go to Settings tab and then look for buildpacks section:

![buildpacks](https://bitbucket.org/tablerodigital/tablero_digital/raw/443b5649cef732eacb980c1f676713c12b5d01f7/tutorial/buildpacks.png)

Add these two on the previous image **in that order**.

#### Command Line

````bash
heroku buildpacks:set https://github.com/negativetwelve/heroku-buildpack-subdir
heroku buildpacks:add heroku/python
````

### Variables

Again, yo can do it from CLI or browser:

#### Browser

Again, on settings:

![vals_1](https://bitbucket.org/tablerodigital/tablero_digital/raw/443b5649cef732eacb980c1f676713c12b5d01f7/tutorial/vals_1.png)

Click on `Reveal Config vals`:

![vals_2](https://bitbucket.org/tablerodigital/tablero_digital/raw/443b5649cef732eacb980c1f676713c12b5d01f7/tutorial/vals_2.png)

Add the variables on the image. Remember that DJANGO_SECRET_KEY and DJANGO_JWT_SECRET should be secret. Change them!
Also remember to replace "yourAppName" with your actual app name.

Here you have the same variables for copying and pasting:

````bash
SINGLE_HEROKU_APP=true
DJANGO_DEBUG_MODE=false
DJANGO_SECRET_KEY=gp_gsy)lm8=0@jf!ooyzu7ap*y+88^9vl9u^3!mt27=wut4cs$
DJANGO_JWT_SECRET=fTjWnZr4u7x!A%D*G-KaNdRgUkXp2s5v
DJANGO_PRODUCTION_CORS_ORIGIN=https://yourAppName.herokuapp.com
NODE_ENV=production
VUE_PUBLIC_PATH=https://yourAppName.herokuapp.com/
REST_API_URL=https://yourAppName.herokuapp.com/api/v1.0/
````

#### Terminal

Commands are:

````bash
heroku config:set SINGLE_HEROKU_APP=true
heroku config:set DJANGO_DEBUG_MODE=false
heroku config:set DJANGO_SECRET_KEY=gp_gsy)lm8=0@jf!ooyzu7ap*y+88^9vl9u^3!mt27=wut4cs$
heroku config:set DJANGO_JWT_SECRET=fTjWnZr4u7x!A%D*G-KaNdRgUkXp2s5v
heroku config:set DJANGO_PRODUCTION_CORS_ORIGIN=https://yourAppName.herokuapp.com
heroku config:set NODE_ENV=production
heroku config:set VUE_PUBLIC_PATH=https://yourAppName.herokuapp.com/
heroku config:set REST_API_URL=https://yourAppName.herokuapp.com/api/v1.0/
````
Notice that secret key may give you trouble because of weird characters. You can set those on browser.

### Pushing branch and running:

Commands:

````bash
git push heroku master
# Takes time
heroku ps:scale web=1
heroku open
````
You can push another branch using `git push heroku branchName:master`, but it is not recommeded.
Remeber that `heroku` remote is still a git remote, and you cannot push different branches into a remote without merging them first.

### Execute migration
Remeber to run migrations, same as you would locally.
To execute commands on the app workspace, you generally prefix them with "heroku run":

````bash
heroku run python manage.py makemigrations
heroku run python manage.py migrate
````

# Deployar con dos app

Here we will create an app for the backend (eg. api-tabler),
and one for the front-end (eg. front-tablero).

Remember that app names still must be unique and might be taken. Replace the name used here with your actual app name.

````bash
heroku create api-tablero
heroku buildpacks:set heroku/python -a api-tablero
````

Notice that since we have two apps we nee to specify which one we we refer to by use of the `-a` argument.

Setting variables:

````bash
DJANGO_DEBUG_MODE=false
DJANGO_SECRET_KEY=gp_gsy)lm8=0@jf!ooyzu7ap*y+88^9vl9u^3!mt27=wut4cs$
DJANGO_JWT_SECRET=fTjWnZr4u7x!A%D*G-KaNdRgUkXp2s5v
DJANGO_PRODUCTION_CORS_ORIGIN=https://front-tablero.herokuapp.com
````
Remeber to keep actual secret keys secret. Do not use these in production!

Now we deploy backend app:

````bash
git push https://git.heroku.com/api-tablero.git HEAD:master
heroku ps:scale web=1 -a api-tablero
# we don't call heroku open on this one
````

Now vue app:

````bash
heroku create front-tablero
heroku buildpacks:set https://github.com/heroku/heroku-buildpack-multi-procfile -a front-tablero
heroku buildpacks:add https://github.com/negativetwelve/heroku-buildpack-subdir -a front-tablero
````

And variables:

````bash
heroku config:set NODE_ENV=production -a front-tablero
heroku config:set PORT=443 -a front-tablero
heroku config:set PROCFILE=frontend/Procfile -a front-tablero
heroku config:set VUE_PUBLIC_PATH=https://front-tablero.herokuapp.com/ -a front-tablero
heroku config:set REST_API_URL=https://api-tablero.herokuapp.com/api/v1.0/ -a front-tablero
````

To run:

````bash
git push https://git.heroku.com/front-tablero.git HEAD:master
heroku ps:scale web=1 -a front-tablero
heroku open -a front-tablero
````



