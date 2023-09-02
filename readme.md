1. git clone
2. npm i
3. npm start
4. check the application on localhost:3000 on thunder or postman becouse it in an backend application
5. localhost:3000/movies 
 you will get all the moovies from third party rest api
6. now post movies in your own db 
        req type :post 
        loclahost:3000/favorities
        payload :{
            "name":"A New Hope",
            "type":"movie",
            "url":"some url"
        } 

7. Now lets to it dockerize

FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["node", "app.js"]

8. build image
    $ docker build -t fav-node .
9. Run container
    $ docker run --name fav-node-c -d --rm -p 3000:3000 fav-node

For now we dont need volume 

but when we see localhost 3000 no container is running and localhost refused to connect

to check what the error run cmd
$ docker ps
but we dont find any container in runing mode because when the error comes container stoped and it in detached mode so automatically deleted becuase we used --rm 

Now again run the container with -d -rm detached mode 
The error coming because of mongo connect
MongoNetworkError: failed to connect to server [localhost:27017] on first connect [Error: connect ECONNREFUSED 127.0.0.1:27017
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1595:16) {
  name: 'MongoNetworkError'
}]

connecting container to localhost mongodb giving error 

but now we will comment the mongo and check api request of web is working or not
 
  and we will directly add app.list(3000);
  and again run the container in detaiched mode
  create again new image and run cantainer
  docker run --name fav-node-c -d  --rm -p 3000:3000 fav-node

  now check on postman
  http://localhost:3000/people 

  work fine...

step 10 : checkout on second-step branch for communicate with mongodb db

to solve mongo dv localhost issue
mongodb://localhost:27017/swfavorites change it to like
mongodb://host.docker.internal:27017/swfavorites

now run the post request and check the localhost db in mongo

That end.

communicating with container to container 
nodejs to mongo db container 

to run the mongodb container we will use docker mongodb image
and cmd to run conatiner
$ docker run -d --name mongodb mongo 
now connect this mongodb container to nodejs conatiner
run inspect command and copu the IPAddress of mongodb container from networksetting and use it in place of "host.docker.internal" in mongodb url
Ex: 
mongodb://host.docker.internal:27017/swfavorites 
mongodb://172.17.0.3:27017/swfavorites


$ docker container inspect mongodb  
 now again the build image and run the container and run the api calls but you will and post api works and get also which will return the data which is just now saved  
 get request localhost:3000/favorites
 because the data is saved in mongodb conatiner not in mongodb localhost

 but there is one issue every time we need add build image if the mongodb container ip will changes
Docker Network : within a docker network, all container can communicate with each other and IPS are automcatically resolved

First stop the running conatiner both node and mongo
docker stop mognodb
docker stop fav-node-c
docker container prune

now run the cmd
now create the network first
run docker network --help
docker network create favorites-net
docker network ls
now run "mongodb" conatiner with mongo and "favorites-net" network image
docker run -d --name mongodb --network favorites-net mongo
do the changes in code 
Ex 
mongodb://172.17.0.3:27017/swfavorites
mongodb://mongodb:27017/swfavorites

create the new image and run node js container with same network
$ docker build -t fav-node .
$ docker run --name fav-node-c --network favorites-net -d --rm -p 3000:3000 fav-node

here we can see we have connect between container with network


