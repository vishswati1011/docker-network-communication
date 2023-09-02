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

10. 
