This are some notes and thoughts that I would like to leave to myself in order to have a full understanding on how compose different Docker containers to have my own development env where I'll be able to easily fire up new project in Rails.

The base features that this env should be:
* a full system that let me fire up Rails projects really fast
* the ability to work with Remote Containers of VS Code and set up the entire editor specifically for the project

## Docker Configuration
I believe this should be an easy one since the containers should be:
* *ruby*
* *postgres*

Check out [alpine ruby image](https://github.com/bkuhlmann/docker-alpine-ruby)

# Random notes 
Taken while making experience with Docker for Rails Developers.

#### Building my first 
`docker run -i -t --rm -v ${PWD}:/usr/src/app ruby:latest bash`

This commands opens an interactive bash terminal (`-i -t`, I believe we could also write `-it`) into a Docker Container that uses `ruby:latest` as Image. It will also mount a volume, the current folder `${PWD}`, linked to `/usr/src/app` so inside the container anything you do in this folder will be reflected on local and -more important - viceversa.

`rails new myapp --skip-test --skip-bundle`

Created my app but I have to remember to **install Tailwind and esbuild**, basically we need to add `-j esbuild` and `--css tailwind`.

#### You must create a custom image
Since we would like to also have our Docker Container running our Rails app we want to skip all the *install Rails* when we use it, so it makes a lot of sense to create our image that will setup the container for us.

This is the code I use in the `Dockerfile` that describes the image:
```
FROM ruby:latest

RUN apt-get update -yqq
RUN apt-get install -yqq --no-install-recommends nodejs

COPY . /usr/src/app

WORKDIR /usr/src/app
RUN bundle install
```

We have several `RUN` that execute commands but the important thing to remember here is how `WORKDIR` works because the first `apt-get`s are runned from the current folder but we are able to run `bundle install` only because we moved from `/` to `/usr/src/app` wher ewe previously have installed our Rails application.

#### Run Rails in custom container
Once we build the previous Image, it's time to `run` it and launch our `rails s`, to do so we use the following:
`docker run -p 3000:3000 <container-id> bin/rails s -b 0.0.0.0`

`-p` will forward the container port (the second one listed) to the local specified port (first one) while the `-b` option passed to `bin/rails s` (that will be executed inside our container) serves to change the default *127.0.0.0* that Rails set up to *0.0.0.0* and allow our container to listen for all IPv4 address on the machine because the server is running inside the container and the request comes from outside.

But we can be even more clever and let the container run `rails s` whenever we start it, we just need to edit the `Dockerfile` and add the following line:
`CMD ["bin/rails", "s", "-b", "0.0.0.0"]`

## Managing Containers with Compose
When we use Compose we're able to handle the running status of each single service, aka the containers we're running in it.
![[simplified-container-lifecycle.png]]

* `docker compose up` - let's build and start each containers defined in it, we can also run this as *detached* mode with `-d` and means that once the containers have started we will have back our terminal withouth seeing any feedback from the server
* `docker compose stop` - will stop all the running services listed in the `docker-compose.yml` but you can also stop single service with `docker compose stop <service_name>` 
* `docker compose start` - will start all the services in `docker-compose.yml` but is able also to start single ones if you provide to it the `<service_name>`
* `docker compose restart` - useful if you have applied some changes and want to restart all the services without manually `stop` and `start` them. As the previous, if you provide a `<service_name>` it will restart just that one otherwise it'll restart all of them.

#### Orchestrate different containers
In `docker-compose.yml` we're able to list all the *services* (the components of my app as server web, database...) and describe how they play togheter.

```yaml
version: "3"

services:
  web:
    build: .
    ports:
      - "3000:3000"
```
In this case we list a single service that we named `web`. We tell Compose that the image this container is based on is described in `Dockerfile` with `build: .` and that from this container the port `3000` will be forwarded .
> *Remember:* you can do the same with `docker run`, for example you forward the port with `-p 3000:3000`, but it will be less repeatable because you should do it for each container in the application,

`docker compose up` will run each service in order and will `build` a container only if does not find one, but it'll also connect all the containers to the same network giving us helpful commands and convention to simplify the process.
##### Use an existing image to build from
We saw how the `web` service gets `build` from the `Dockerfile` sitting in the folder but most of the time we need to use custom images mantained by the community (or ourself).

To do so we'll not use the `build` command for the service, we simply add an `image` and pull it from the Hub. For example here's how we could add a Redis server for our app:
```yaml
version: "3"

services:
  # Other services
  redis:
   image: redis
```
##### Mounting volumes
In order to share the files in your folder you must mount a volume, with `docker-compose.yml` this is as simple as to add a new configuration option `volumes`.
```yaml
version: "3"

services:
  web:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
```

#### Differences between `exec` and `run`
With `run` you will run a totally separated Container, even if one it is already running; while `exec` is able to run the command that we provide inside the Container that's running.
#### Share ENV variables
Even if it's not the most secure approach we can share ENV variables right inside our `docker-compose.yml` file.

For example, when we build the PostgreSQL image we can set up the username, password and other right passing the `environment`:
```yaml
version: "3"

services:
  # Other services
  database:
    image: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: some-long-psw
      POSTGRES_DB: dockerized_r7_development
```
At the same time we can pass the same variables to a different service that needs them in order to access. For example our Rails app, the container running it, must access to the database that is defined as a separate service (aka container) that it's connected to the same network.
```yaml
version: "3"

services:
  web:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
    environment:
      DATABASE_HOST: database
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: some-long-psw
      POSTGRES_DB: dockerized_r7_development
  # Other services
```
Now all we need is to use them right inside the `defaul` connection settings stored in `config/database.yml`:
```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  host: <%= ENV.fetch("DATABASE_HOST") %>
  username: <%= ENV.fetch("POSTGRES_USER") %>
  password: <%= ENV.fetch("POSTGRES_PASSWORD") %>
  database: <%= ENV.fetch("POSTGRES_DB") %>
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  variables:
    statement_timeout: 5000d

development:
  <<: *default
```
## Steps for Dummies
This is the collection of steps I took to generate my dev env:
### 1. Install Rails
If you start from an empty folder you need to get the *closest image* that will help you in building your container. Since we talk about Rails we need a [Ruby image](https://hub.docker.com/_/ruby).

`docker run -i -t --rm -v ${PWD}:/usr/src/app ruby:latest bash`

Now you have a terminal inside the container that will allow you to run the Rails installation
