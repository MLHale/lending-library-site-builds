# Lending Library App Build Repository.
Summary to go here

## Requirements
This app is fully containerized. There is only 1 requirement:
* Docker (https://docs.docker.com/install/)

## API (Backend) Installation
You need to build the docker image from the provided DockerFile using Docker Compose. To do this:

### Acquiring the files
```bash
git clone --recursive https://github.com/MLHale/lending-library-site-builds.git
cd lending-library-site-builds
git submodule sync
git submodule update --init --recursive --remote
```

### Building and Running the API
```bash
docker-compose build
docker-compose up -d
```

This creates a few docker containers with all of the requisite installed dependencies to run the dev environment. It also initializes the database and starts the containers.

### Setup an Admin user and compile C libraries
To create a new admin user for use in the admin portal do the following once the containers are up and running (i.e. `docker-compose up -d` has been run).

```bash
docker-compose exec django bash
python manage.py createsuperuser
# provide admin credentials
exit
```

Now visit `localhost` in your host browser to view the API.

## Development Environment
### Running the backend API in development
In addition to the production deployment, you can also run the server in development mode. In development mode, the server will reload automatically whenever you make any sort of code changes in python.

To run the server in dev mode, simply execute the following from within the `dev` folder:

```bash
docker-compose build
docker-compose up -d
```
Now visit `localhost` in your host browser to view the api.
> Note you may need to adjust the django settings file to serve up the static files in dev (this can usually be accomplished by setting debug to true and uncommenting the the static file directory option)

### Setup an Admin user and compile C libraries
If you run the server in dev mode, you will still need to set it up first.

```bash
docker-compose exec django bash
python manage.py createsuperuser
# provide admin credentials
exit
```

### Running the client-side ember app in development
While running in dev mode any changes to the javascript code will automatically recompile and push to the backend. The following commands runs the client-side app in dev mode:

#### Installing
To setup the client-side dev environment do the following:

* ensure node/npm are installed
* ensure ember-cli is installed

from `frontend`, run:
```bash
npm install
```
Now to build the code and push it to the backend server:
from `frontend`, run:
```bash
ember build -w -o ../lending-library-backend/static/ember
```
> thats it, your code will auto-rebuild as you make changes


## Collaborating on this project
To prevent merge conflicts, always make sure you are working on a git branch. Read more about git branching here: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
For an overview of useful git commands, visit: https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf

To create a new branch in each repo, navigate to the parent directory:
```bash
cd <path to this project directory>
git pull
git branch <yourname>-<feature-name>
cd backend/
git pull
git branch <yourname>-<feature-name>
cd ../frontend/
git pull
git branch <yourname>-<feature-name>
```
It is good practice to name the branch using a combination of your name and the feature you are working on. For example ```git branch mlhale-userinterface``` might be an acceptable branch name.

### When to make a pull request
Once you have finished developing whatever feature you are working on, make a pull request. To make a pull request, use the github web or desktop interfaces to select where the pull request targets (usually the ```master``` branch).

Visit https://help.github.com/articles/about-pull-requests/ for more information.

## Monitoring containers
If desired, you can view the performance of containers using the google cAdvisor project (see https://github.com/google/cadvisor).

To run do the following:
```bash
docker pull google/cadvisor:latest
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

Now visit ```localhost:8080``` or ```<ip>:8080``` to view the current performance.

# License
The Lending Library App is an search-oriented award management app for maintaining the uno lending library.
Copyright (C) 2017  Matt Hale and CYBR8470 Class.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
