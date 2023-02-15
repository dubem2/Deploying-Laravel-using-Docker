# Deploying-Laravel-using-Docker

## Objective

This project demonstrates how to deploy a Laravel application using Docker. (Note: a mac was used to carry out this project)

## Introduction
Docker is a platform that allows you to easily build, test, and deploy applications as containers. Containers are lightweight, stand-alone, and portable executable packages that include everything needed to run a piece of software, including the code, runtime, system tools, libraries, and settings.

## Prerequisites
- Docker installed on your machine (Preferably Docker Desktop for mac as it comes with Composer already installed)
- Basic knowledge of Laravel and Docker

## Deployment Steps
### 1. Clone the laravel repo to your local machine.

run the command "git clone https://github.com/laravel/laravel.git laravel-app". 

### 2. Creating Docker Compose.

In this section you will write a docker-compose file that defines your web server, database, and application services by moving into the laravel-app directory and running "nano docker-compose.yml"

  __Check out the docker_compose.yml file in this repo and use it.__

### 3. Let's create a Dockerfile for our laravel-app application.

Dockerfile includes instructions that Docker can use to build custom Docker images. It can also install the software required and configure the necessary settings for your application. They specify the environment inside a container that will host your application code. 
We will create a Dockerfile that will specify the instructions to build the Laravel application image. Move into the laravel-app directory and use nano to create the Dockerfile thus; "nano Dockerfile"
  __Please checkout the dockerfile in this repo and use it__

### 4. Let's make configuration directories inside our laravel-app directory
   - PHP config Directory: run "mkdir php" to create a directory, then run "nano /php/local.ini" and input the following;
   
    upload_max_filesize=40M
    post_max_size=40M
    
   - Nginx Config Directory: To configure Nginx, you will create an app.conf file with the service configuration in the ~/laravel-app/nginx/conf.d/ folder. Add the lines of code inside the */nginx/conf.d/app.conf* file in this project repo
  
   - Mysql configuration directory: To configure MySQL, you will create a mysql directory in the laravel-app dir and create the my.cnf file in the mysql folder. This is the file that you bind-mounted to /etc/mysql/my.cnf inside the container in Step This bind mount allows you to override the my.cnf settings as and when required.
   In the app.conf file, add the following code to enable the query log and set the log file location:
   
    [mysqld]
    general_log = 1
    general_log_file = /var/lib/mysql/general.log
    
### 5. Modifying Environment Settings
We will make a copy of the .env.example file that Laravel includes by default and name the copy .env, which is the file Laravel expects to define its environment:

      cp .env.example .env
You can now modify the .env file on the app container to include specific details about your setup.<br>
Open the .env file using nano or your text editor of choice <br>
Find the block that specifies DB_CONNECTION and update it to reflect the specifics of your setup. You will modify the following fields:<br>
.DB_HOST will be your db database container.<br>
.DB_DATABASE will be the laravel database.<br>
.DB_USERNAME will be the username you will use for your database. In this case, we will use laraveluser.<br>
.DB_PASSWORD will be the secure password you would like to use for this user account.<br>
Save the file and exit your editor.<br>

### 6. Running the Application with Docker Compose
Use the command below to create the app image:<br>
```docker compose build app```<br>
When the build is finished, you can run the environment in background mode with:<br>
```docker compose up -d```<br>
The environment is now operational, but we still need to perform a few tasks to complete the application setup.<br>
To remove the composer.lock if there is any run:<br>
```docker compose exec app rm -rf vendor composer.lock```

We’ll now run composer install to install the application dependencies:<br>
```docker compose exec app composer install```

Before testing the application, the last step is to create a special application key using the artisan Laravel command-line tool:<br>
```docker compose exec app php artisan key:generate```

As a final step, visit http://localhost:80 in your browser to test the app.


## Conclusion:
In this project, you have learned how to deploy a Laravel application using Docker. Docker is a powerful tool that allows you to easily deploy applications in a consistent and repeatable way. By using containers, you can ensure that your application runs in the same environment everywhere, which is essential for developing and testing applications in a production-like environment.

