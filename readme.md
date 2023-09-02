1. git clone https://github.com/vishswati1011/docker-network-communication.git
2. npm i
3. npm start
4. Check the application on localhost:3000 on Thunder or postman because it in a backend application
5. localhost:3000/movies 
 you will get all the movies from third-party rest API
6. Now post movies in your own db 
        req type:post 
        loclahost:3000/favorites
        payload :{
            "name":"A New Hope",
            "type":"movie",
            "url":"some url"
        } 

7. Now let do it dockerize

FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["node", "app.js"]

8. build image
    $ docker build -t fav-node .
9. Run the container
    $ docker run --name fav-node-c -d --rm -p 3000:3000 fav-node

For now we dont need volume 

but when we see localhost 3000 no container is running and localhost refuses to connect

To check what the error run cmd
$ docker ps
but we don't find any container in running mode because when the error comes container stops and it is in detached mode so automatically deleted because we used --rm 

Now again run the container with -d -rm detached mode 
The error coming because of mongo connect
MongoNetworkError: failed to connect to server [localhost:27017] on first connect [Error: connect ECONNREFUSED 127.0.0.1:27017
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1595:16) {
  name: 'MongoNetworkError'
}]

connecting container to localhost mongodb giving error 

but now we will comment the mongo connect code and check API request of the web is working or not
 
  and we will directly add app.listen(3000);
  and again run the container in detached mode
  Create again new image and run container
  docker run --name fav-node-c -d  --rm -p 3000:3000 fav-node

  Now check on Postman
  http://localhost:3000/people 

  work fine...

step 10: checkout on the second-step branch to communicate with Mongodb database
