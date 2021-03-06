# Deploying two heroku applications from one github repo
In this example i'll deploy a node.js backend and a angular frontend.

## You'll need
1. The heroku CLI
2. The Angular CLI
3. Two created heroku applications

### Setting up the Node.js server
1. Create a dir called server (or whatever you want)
2. Go to your server folder; use the command ``` heroku git:remote -a applicationName ```
 		- Make sure you are within the Server folder when using this command
3. Go back to your main folder; use the command ``` git remote -v```
 		- It should list 2 remotes
4. When two remotes are listed within the main directory use the following command: ```git remote rename heroku heroku-server```
	- note that heroku-server can be whatever you want to be
5. After succesfully renaming the remote; you can use git add & git commit inside your main directory just like normal. 

	If you want to update your heroku-server node you can do so by using the command ```git subtree push --prefix server heroku-server master```
	- heroku-server is your remote name --prefix server is used to push everything inside the /server directory

6. Go to your heroku node for server; if you've used my app.js it should just display a message with 'Application is running' which means everything worked fine

### Setting up the Angular frontend
1. use the command ng new your-application name (client for me)
	 - You can use whatever settings you like, it does not matter for deploying
2. After the Angular CLI has run, got to your package.json in the client folder and copy ```typescript and all @angular/``` from devDependencies to dependencies.
3. Under the scripts tag (still in package.json) add the following line
```"postinstall": "ng build --aot --prod"``` this tells Heroku to build the application using the AOT compiler, also change your ```"start": "ng serve"``` to ```"start": "node server.js"```.
4. Add Node and NPM engines, you can grab the versions I used in my package.json or run the command ```node-v and npm -v``` to get your locally installed version. You can add the engines underneath your devDependencies by using ```  "engines": {
    "node": "6.11.0",
    "npm": "3.10.10"
  }```
5. You'll need to add a server.js here as well. It'd recommend grabbing mine again.
	- Make sure to change ```/dist/client``` to your respective client name (e.g. ```/dist/amazingApp```)
6. Verify everything works by running ```ng serve```
7.  After everything runs; add the second remote by using ``` heroku git:remote -a applicationName ``` again.
	- make sure to be in the client folder.
8. When going back to the main folder; and using the command ```git remote -v``` all remotes should be visible, you can rename the client remote as well or keep it as heroku. (I renamed my to heroku-client)
9. Using git subtree push again from the main directory ```git subtree push --prefix client heroku-client master``` you should be able to sucessfully update your heroku.

### Updating your git-repo
After setting up both sides you might want to push to your github repo, you can do so like normal by just using ```git push origin branchName```.

I found heroku might say there is not anything to update when you push to github before pushing to heroku. Something to keep in mind.

### After all this setup
When you want to update your heroku client/server just use the git subtree push so
```git subtree push --prefix client heroku-client master``` or
```git subtree push --prefix server heroku-server master```

Updating your github repo should go as normal. you can use a commandline or gitkraken etc. 
Make note that pushing to github before updating heroku could cause heroku to say that there isn't anything to update.

### Notes
I've found using a procfile would cause difficulty, hence i've tried doing it this way. It works pretty well however if you make the fault of pushing client to server or server to client you'll need to clear /delete the heroku end.

Furthermore the client also uses express so that heroku can properly serve the pages.

#### Resources used
 - https://medium.com/@hellotunmbi/how-to-deploy-angular-application-to-heroku-1d56e09c5147
