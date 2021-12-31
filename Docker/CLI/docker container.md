The `docker container` is one of the management commands that allow us to manage our, you guess it, containers. 

As for many other `docker` commands you can check all its options with `docker contaienr --help` and you can investigate even further by yourself.

Those are the general options that you probably use most of the time with `docker container`.

## `ls` List
With `docker container ls` we see all the active containers. As in Linux if you want see also the previously ran containers we can add an additional option `docker container ls -a`
## `run` Run
As for the [[docker image]] this command let us run a container with the image specified as the last option of the command
### `-it` Interactive
This option allow us to make the container interactive so it is able to listen for standard command as *Ctrl+C*, terminal colors and set up the terminal to be responsive among others. Generally is always a good idea to use `-it`.
### `-p` Ports
It allows us to specify how to publish the port of the container to the host. Generally we match them indicating the container port and the host one separated by comma: `-p 5000:5000`
But you can also let Docker **randomly pick the port** that we will use in the host by specify only the port of the container `-p 5000`.
### `-e` Environment
Allow us to pass an environment variable into the container. For example the Flask app we're using as example require the `FLASK_APP` to be set, so while we use our `run` command we can pass it with `-e FLASK_APP=app.py`
### `--rm` Remove
When used Docker will automatically remove the container from the list, that you can see with `ls`, once the container has been shut down. 
### `--name` Rename
With this we can set a custom name for our container, if you want to call you container `my_cool_flask_app` all you have to do is `--name my_cool_flask_app`. If not provided Docker will randomly assign a name.
### `-d` Detach
If you do not want that Docker will take the control of your terminal with the `-d` option you can detach it and have your terminal at your disposal.
### `--restart` Restart
Docker is able to restart the container in case something happen, a common option is to use it with `on-failure` so if something bad happen that forces the container to shut down we're Docker is able to bring the container up automatically.
This option **does not work** if `--rm` is declared, that's because if Docker removes the container once stopped it will not able to restart it.
### `-v` Volumes
We can link a folder in the container with one that is present in the host. When we use the option `-v` with `docker container run` it expect two values separated by a column. The first value should be the local folder we want to mount and the second one is the path of the container where the folder will live. For example if I want to mount the current directory to the `/app` folder in the container we can write `docker container run -v $PWD:/app`, this is because in our *Dockerfile* we set the `/app` folder as our working directory with `WORKDIR /app`.
We can even set multiple volumes but there are better approaches than using the command line.

If we will use `docker container inspect <container_name>` we should see inside the *Mounts* portion that our volumes are correctly bind:
```
"Mounts": [
	{
		"Type": "bind",
		"Source": "/Volumes/Dati/Coding/diveintodocker/src/06-docker-in-the-real-world/03-creating-a-dockerfile-part-1",
		"Destination": "/app",
		"Mode": "",
		"RW": true,
		"Propagation": "rprivate"
	}
],
```
### `--net` Network
Allow to connect a container to a custom network, this is really useful since we can create multiple containers that talks to each other. For example a Rails container that talks with a PostgreSQL as we do in our work. While using `docker container run` you can add `--net <network_name>` and you'll connect that container into the network.
## `logs` Logs 
You can check the logs of your container any time you want, you just need to use `docker logs <container_name>` and it will show to you all the logs that your application has produced. This command will exit automatically once it has listed all the logs.
### `-f`
if you want to keep it open and see each message as they come all you need to do is to add `-f` to the command and it will behave like the `tail` unix command.
## `stats` Stats
Return usage metrics all the containers that are running.
## `stop` Stop
You can manually stop the container providing the container name to this `stop` command.
## `inspect` Inspect
This useful command let us know many information about the container we specify by its name as last parameter. 
## `exec` Run command
With `docker container run [options] <container_name> <command>` you're able to run the `<command>` inside the container you specify. Generally this is used in combination of `-it` since you want to have an *interactive* terminal able to run your Linux commands.