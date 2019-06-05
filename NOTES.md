#### installation process
Install Nodejs
$ npm install -g create-react-app
$ create-react-app frontend

#### Now a project called frontend is created.

cd frontend

$ npm run start
$ npm run test
$ npm run build

Dockerfile.dev for developement
Dockerfile for production

$ docker build -f Dockerfile.dev -t flow/frontend .

$ docker run -p 3000:3000 flow/frontend

#### To share volume
The below command mounts the pwd of the host to /app inside the container.
$ docker run -p 3000:3000 -v $(pwd):/app flow/frontend

Tell docker node_modules is installed in /app/modules. (since we mount $pwd as the app folder, it needs to be overwritten)

Tell docker do not map a folder against node_modules.<br>Now we can edit the files in src/ and app gets reloaded automatically (done by react)
$ docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app flow/frontend


#### Usinf docker-compose
to rebuild <br>
$ docker-compose up --build

#### Production Mode
This could be achieved with multi step builds.<br>
Install npm deps and run build, copy only the build directory(which is required to run the app, node_modules are used only during dev/build)
to nginx image and start nginx.<br>

dockerfile<br>
copy files<br>
npm install<br>
npm run build<br>

#### Host the site in a nginx machine
dockerfile<br>
copy build<br>
nginx start<br>

To Build the image:<br>
$ docker build -t flow/frontend-prod .
<br>To Run the container:<br>
$ docker run -p 8080:80  flow/frontend-prod
