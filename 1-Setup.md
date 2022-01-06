
# Table of contents

1.  [Setting up database](#database)
2.  [Setting up "ABAC Management System"](#react)
3.  [Setting up "ABAC Server"](#express)

## Setting up database <a name="database"></a>

### 1. Exporting database from development environment
Running command line below in terminal will generate a SQL file that will restore the database.

```shell
pg_dump --dbname <database_name> -f <file_name>.sql -h <hostname> -p <port> --username <username>
```

### 2. Import database to production environment
Running command line below in production terminal will restore the database. Newly created database is needed for first-time setup to prevent mistakes.
```shell
psql -d <database_name> -f <filename>.sql
```
## Setting up "ABAC Management System"<a name="react"></a>
#### Requirements
1. apache2
2. Node.js
3. npm
4. git

### 1. Clone the web application
```shell
git clone <github_link_of_react_app>
```
Use npm to install all the required modules

```shell
npm install
```

### 2. Setting up the API configuration

  

Use nano (or any other text editor) to change the API link of the web application.

  

```shell
sudo nano src/ApiConfig.js
```

  

Change the value to the desired value

```js
const  api = 'http://localhost:3001' <== //change here
export {
	api
}
```
>  **_NOTE:_**
>  **"ApiConfig.js"** is added to the .gitignore file so that the development and production environment are separated. Changes made would not be reflected on the git repos.

### 3. Building the web application

Running the below command line will generate a **build** folder.

```shell
npm run build
```

### 4.  Move the build folder

Move the build folder to the following path.
```shell
cp /path/build /var/www/
```

### 5. Configuring the apache

Use nano to edit the configuration file.
```shell
nano /etc/apache2/sites-available/<file-name>.conf
```
>  **_NOTE:_**
> Naming convention of the file should be **00\*-name.conf**, where **\*** is the number of sites. Example: **001-abac.conf**

Inside the config file, insert the following (change entries accordingly)
```
<VirtualHost *:80>
	#ip
	ServerName 192.168.0.124
	#path	
	DocumentRoot /var/www/build
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
	<Directory  "/var/www/build">
		RewriteEngine on
		RewriteCond %{REQUEST_FILENAME} -f [OR]
		RewriteCond %{REQUEST_FILENAME} -d
		RewriteRule ^ - [L]
		RewriteRule ^ index.html [L]
	</Directory>
</VirtualHost>
```

### 6. Start/Reload apache
```shell
sudo a2ensite <file_name>.conf 
sudo a2enmod rewrite
sudo service apache2 restart
```

## Setting up "ABAC Server"<a name="express"></a>
#### Requirements
1. pm2
2. Node.js
3. npm
4. git
### 1. Clone the server
```shell
git clone <github_link_of_express_app>
```
Use npm to install all the required modules

```shell
npm install
```
### 2. Setting up the database connection
Use nano (or any other text editor) to change the database information.
```shell
sudo nano routes/database.js
```
Change the value to the desired value
```js
const  Client = require("pg").Pool;
const  client = new  Client({
	host:  "localhost",
	user:  "postgres",
	port:  5432,
	password:  "password",
	database:  "abac_db",
});

const  table_name = "client_side_db";
module.exports = {
	client,
	table_name,
};
```
>  **_NOTE:_**
>  **"ApiConfig.js"** is added to the .gitignore file so that the development and production environment are separated. Changes made would not be reflected on the git repos.

### 3. Start the server
Start the server by running the following line in the directory of the express app:
```shell
pm2 start app.js
```
