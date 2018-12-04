# Deploying two heroku applications from one github repo
In this example i'll deploy a node.js backend with an angular frontend.

## You'll need
1. The heroku CLI
2. Two created heroku applications

### Setting up the Node.js server
1. Create a dir called server (or whatever you want)
2. Go to your server folder; use the command ``` heroku git:remote -a applicationName ```
 		- Make sure you are within the Server folder when using this command
3. Go back to your main folder; use the command ``` git remote -v```
 		- It should list 2 remotes
4. When two remotes are listed within the main directory use the following command: ```git remote rename heroku heroku-server```
	- note that heroku-server can be whatever you want to be
5. After succesfully renaming the remote; you can upload your server by using the command ```git subtree push --prefix server heroku-server master```
	- heroku-server is your remote name --prefix server is used to push everything inside the /server directory

### Setting up the Angular frontend
1. use the command ng new your-application name (client for me)
2. 