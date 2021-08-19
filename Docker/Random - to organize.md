# Volumes
A volume let's you to clone a directory that lives inside your container with one that is available on your system, so you'll be able to apply changes and save them for later use.

> **Remember:** if you do not mount a volume all the edits you make inside a container will be lost when the container will shut down.

Without a volume in place each time we make an edit we have to build the image for the container, this is because while building Docker will copy the changed files into the new container.
> **Reminder:** To build an image you need to run `docker image build <image_name> <container_directory>`

One of the way we can add a volume while running a container is to set the `-v` option to the `run` command.