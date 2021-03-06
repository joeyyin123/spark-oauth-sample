# Spark OAuth Sample

Function Description: If you want to do something (like create a room, or send a message) on Spark on behalf of someone else, you need to request permissions to the person. This application provides you with a user interface (web) where the person can accept your request and send the generated token to you. It allows you to define the access scope, client ID, client secret, log level, DB info, and super admin info in a configuration file.

There is a token management center which allows the requester to manage (refresh/remove) token from database (mongodb), and manage(add/update/remove) an admin account.

Installation
============

To get started you will first need to have Node.js installed. Going to [node.js](https://nodejs.org/), it will automatically detect your OS and offer the download link. After you get the installer, just double click it and follow the "next step" to complete the installation. For Linux OS, it provides [Linux Binaries (.tar.gz)](https://nodejs.org/en/download/) in both 32-bit and 64-bit. Download the .gz file to your machine and uncompress it, configure the PATH, then you can use it.

You also want a database (here we use mongodb) to store your tokens. Go to https://www.mongodb.org/ to download and install it in your local. Please do not forget setting environment variable (export PATH=my_mongodb_server_path/bin:$PATH in your Users/your_username/.bash_profile) so that your machine knows where to run "mongod" command to start mongodb server.


To install this sample to your local machine, you can either use NPM (Node Package Manager): 

	~$ npm install --save spark-oauth-sample

or download from [here](https://github.com/tropo/tropo-webapi-node/archive/master.zip), then uncompress and place it into your project folder. 


If you run into the below exception when installing (specifically for bcrypt) by NPM on mac, please install higher version of GCC. This is due to that the version of GCC Apple ships are 4.2 which is ancient, and App don't ship a newer version. You can either install a modern GCC from somewhere like Mac Ports (which is probably simpler and quicker) or build it by yourself following the instructions at http://gcc.gnu.org/wiki/InstallingGCC. In this case, I used Macports to install GCC5 here.

	cc1plus: error: unrecognized command line option "-std=gnu++0x"
	make: *** [Release/obj.target/bcrypt_lib/src/blowfish.o] Error 1
	gyp ERR! build error 
	gyp ERR! stack Error: `make` failed with exit code: 2


Configuration
=============

1. You will first need to create an application on "Spark For Developer" platform following the instructions. 

2. After an application is created, you will see a unique client ID and a unique client Secret ID in the top-right of the page.

3. Go to the "conf" folder of this sample, open the config.json.

4. Copy the client ID to the "clientID" field, and copy the client Secret to the "clientSecret" field.

5. Choose the redirect URL you want to use, and copy it to "redirectURIUnencoded" field. The redirect URL must be one of the redirect URLs in your application.

6. Encode the redirect URL in step 5 (from a tool, i.e. http://meyerweb.com/eric/tools/dencoder/), then fill the encoded URL into "redirectURI" field.

7. Copy out the values of the scope from your application, use whitespace character to seperate them if there are multiple values.

8. Encode the values (from a tool, i.e. http://meyerweb.com/eric/tools/dencoder/), then fill the encoded values into "scope" field. (For 7 and 8, please follow the existing value in config.json as sample if you have problem/question)

9. Change the "contactEmail" field to your email address (as the requester).

10. You can change the 'state' field to a more secure string (i.e. a random UUID string), or keep it as it is just for testing (do not recommand using special characters).

11. If you want to put the sample into a public address, then you will need to change the hostname 127.0.0.0 and port 3000 to the real values. If you test it in local, make sure you use 127.0.0.1 instead of localhost as host name (You also need to use 127.0.0.1 instead of localhost if you want to set a local redirect url in Spark app).

12. The logLevel is for defining the level (debug/warn/error/fatal) of logs, which are used for troubleshooting.

13. The "dbConfig" section is for configuring the ip, port, the name of the DB storing admin users and the name of DB storing tokens.

14. The "superAdminAccount" section is for configureing the profile of the super admin account. It has the highest level permission, like create/update/remove a normal admin account. Normal admin account can only refresh/remove token and update his/her own profile (except username).

15. Save it, and the configurtion is compelte.


Running
-------

If you download the project folder into your local, you just need to CD to the folder and run the below command.

	node app.js

If you use NPM to install, the simplest way would be CD to /node_modules/spark-oauth-sample/ then run the above command.

Home page of starting requesting token：

	http://host:port

Before you get into the admin center, you need to initialize the admin system (specifically initialize super admin account) by running the below link in browser (You will be automatically logged in as super admin user after initialization!)

	http://host:port/admin/initialize
	
Admin login page：

	http://host:port/admin/