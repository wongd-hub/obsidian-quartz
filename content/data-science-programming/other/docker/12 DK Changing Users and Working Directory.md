---
title: "12 DK Changing Users and Working Directory"
---
#course_datacamp-docker #docker 

- `FROM`, `COPY`, `RUN` instructions only affect the file system of the image, and do not modify each other.
- Some instructions can influence others directly, the ones we'll discuss in this lesson are `WORKDIR` and `USER` used to change the working directory and executing user respectively.
## Changing the working directory

- Up until now, when we've needed to specify a path in our Docker image, we've used a full path. This can get lengthy and unwieldy. Instead, we can specify a working directory and then use relative paths from that like so.

```Dockerfile
COPY /projects/pipeline/ /my/long/path/app/

# Becomes
WORKDIR /my/long/path/
COPY /projects/pipeline/ app/
```

- This also affects our starting directory in subsequent `RUN` and `CMD` commands.
## Linux permissions

- What you're allowed to do/your permissions are determined by your user settings in Linux. There is a unique user called the `root` user that has all the permissions in the system.
    - Best practice is to use the `root` user to create one or two new users with permissions required for specific tasks, then to use these better scoped users for everything else. i.e. Don't run everything as `root`.

- The user that we start as is determined by the image we're running. For example, the `ubuntu` image starts you as the `root` user.

- The `USER` command allows you to change user, any following command will be run as the user you set with that command.
    - This command can be run multiple times to switch between users.
    - The last `USER` instruction in the `Dockerfile` will control the default user you start as when you start the image

```Dockerfile
FROM ubuntu
RUN useradd -m repl
USER repl
RUN apt-get update
```
