# docker-node-quickstart
following the freecodecamp git guide

## resources
* https://www.freecodecamp.org/news/what-is-git-and-how-to-use-it-c341b049ae61/  
* https://towardsdatascience.com/getting-started-with-git-and-github-6fcd0f2d4ac6
* https://webkul.com/blog/syncing-local-repository-with-remote-repository-on-github/ *!! for synching local repository with remote*  

## using command line (cli)
-> `cd c:\git\docker-node-quickstart` *!!navigate to local dir*  
-> `git init`   *!!adds a local git repository*  
-> `git add .`  *!!adds all files and folders in local folder to the staging area*  
-> `git commit -m "initial commit for docker quickstart guide for nodejs project"`  *!!committing all files and folders added*  

### optional commands
-> `git status`   *!!shows status regarding what files are modified and what files are in th staging area*  
-> `git log`      *!!show informaiton on commit*  
-> `git branch test`    *!!creates branch named test*  
-> `git checkout test`  *!!navigate to test branch*  
-> `git branch`         *!!list all branches*  
*!!add and commit instructions when in test branch will add and commit to test branch*  
*!!to merge, go back to master branch*  
-> `git checkout master`  
-> `git merge test`     *!!merges test branch to master branch*  

## adding to remote repository GitHub
create new repository in the github webpage  
copy url or github repository  
-> `git remote add origin https://github.com/cheenkuan/docker-node-quickstart.git`    *!!repository url*  
-> `git push -u origin main`   *!!pushes code from the master branch in the local repository to the master branch in the remote repository*  
*!!git default uses main, not master; should push to main*  
-> `git log`    *!!shows the commits*  
-> `git push -u origin --all`    *!!use this push command for first time push*  
-> `git remote -v`   *!!show remote repositories*  

### optional commands
-> `git pull origin master`       *!!pulls the latest changes in the master branch in the remote repository to the local repository*  
-> `git clone [repository url]`   *!!clones an existing remote repository into local*  
-> `git branch -m <oldname> <newname>`  	*!! change branch name*  
-> `git branch -m <newname>`			*!! if change name of current branch, then there is no need to specify old name*  
-> `git branch -M <newname>`			*!! if using case-sensitive file system and mane change is only for change in case, use -M*  

# following docker quickstart guide for building and running images; and sharing images on Docker Hub

## resources
* https://github.com/cheenkuan/docker-node-quickstart.git  
* https://docs.docker.com/get-started/nodejs/build-images/  

## using command line
-> `cd docker-node-quickstart`    *!!navigate to the project folder*  
-> `npm init -y`                  *!!set up a npm package*  
-> `npm install ronin-server ronin-mocks`  

## using visual studio
- create new server.js file with following content  
```
    const ronin     = require( 'ronin-server' )  
    const mocks     = require( 'ronin-mocks' )  
    const server = ronin.server()  
    server.use( '/', mocks.server( server.Router(), false, true ) )  
    server.start()  
```  
- open terminal to run server.js  
    -> `node server.js`  
  
## using Postman
- test server.js using Postman
```
    curl --location --request POST 'http://localhost:8000/test' \  
    --header 'Content-Type: application/json' \  
    --data-raw '[  
    {  
	    "msg": "testing"  
    },  
    {  
      "code":"success","payload":[{"msg":"testing","id":"31f23305-f5d0-4b4f-a16f-6f4c8ec93cf1","createDate":"2020-08-28T21:53:07.157Z"}]  
    }  
    ]'  
```
```
    curl --location --request GET 'http://localhost:8000/test' \  
    --header 'Content-Type: application/json' \  
    --data-raw '{  
      "code":"success","meta":{"total":1,"count":1},  
      "payload":[{"msg":"testing","id":"31f23305-f5d0-4b4f-a16f-6f4c8ec93cf1","createDate":"2020-08-28T21:53:07.157Z"}]  
    }'  
```      
- terminal will show server logs:  
    ```
      2020-XX-31T16:35:08:4260  INFO: POST /test  
      2020-XX-31T16:35:21:3560  INFO: GET /test  
    ```

## using visual studio
- create Dockerfile using visual studio with following content  
```
  FROM node:12.19.0                 *!!creating the docker image from the official node image ver 12.19.0*  
  ENV NODE_ENV=production           *!!specifies the environment in which application will be running; setting it to production environment improves performance*  

  WORKDIR /app                      *!!sets the working directory as /app, which will be the default directory for all subsequent commands*  
  
  COPY ["package.json", "package-lock.json*", "./"]       *!!copies the packages into /app*  

  RUN npm install                                         *!!install package.json, package-lock.json*  

  COPY . .                                                *!!takes all files in the current directory and copies them to image*  

  CMD [ "node", "server.js" ]                             *!!tells docker the command to run when the image is run inside a container*  
```

## using command line
-> `docker build --tag docker-node-quickstart` .                *!!builds the docker image*  
-> `docker run --publish 8000:8080 docker-node-quickstart`      *!!starts the container and expose port 8000 to port 8000 on the host*  
-> `docker run -d -p 8000:8000 docker-node-quickstart`          *!!alternative to above to run the container in detached mode*  
  
### optional commands
-> `docker --v`         *!!shows docker version*  
-> `docker images`      *!!shows images*  
-> `docker tag docker-node-quickstart:latest docker-node-quickstart:1.0.0`      *!!create new tag for image*  
-> `docker rmi docker-node-quickstart:1.0.0`                                    *!!untag an image if there are more than 1 tag for 1 image*  
-> `docker rmi docker-node-quickstart:latest`                                   *!!delete an image if the image has only 1 tag*  
-> `docker rmi -f docker-node-quickstart:latest`                                *!!sometimes it is necessary to force the deletion with -f*  
-> `docker stop <name_of_container>`                                            *!!stops a container*  
-> `docker restart <name_of_container>`                                         *!!restarts a stopped container*  
-> `docker rm <name_of_container>`                                              *!!removes a container; removed container will not remain resident in stopped state*  



