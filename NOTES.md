### Installation notes
Let us use Dockerfile.dev for developement and Dockerfile for production

### Dev Mode
#### Installing create-react-app
`$ npm install -g create-react-app`<br>
`$ create-react-app frontend`

Now a project called frontend is created.

Commands to start, test and build the project: <br>
`$ npm run start`<br>
`$ npm run test`<br>
`$ npm run build`<br>

Let us build and run the docker image:<br>
`$ docker build -f Dockerfile.dev -t flow/frontend-dev .`

`$ docker run -p 3000:3000 flow/frontend-dev`

#### To share volume
The below command mounts the pwd of the host to /app inside the container.
`$ docker run -p 3000:3000 -v $(pwd):/app flow/frontend-dev`

Tell docker do not map a folder against node_modules. (since we mount $pwd as the app folder, it needs to be overwritten).
<br>Now we can edit the files in src/ and app gets reloaded automatically (done by react)
`$ docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app flow/frontend-dev`

#### Using docker-compose to solve the above lengthy commands
`$ docker-compose up`

### Production Mode
This could be achieved with multi step builds.<br>
Install npm deps and run build, copy only the build directory(which is required to run the app, node_modules are used only during dev/build)
to nginx image and start nginx.<br>

The Dockerfile does the following

* copy src and template.json
* npm install
* npm run build

Now add nginx steps to host the site.

* copy build from the above image
* nginx start


To Build the image:<br>
`$ docker build -t flow/frontend-prod .`
<br>To Run the container:<br>
`$ docker run -p 8080:80  flow/frontend-prod`
