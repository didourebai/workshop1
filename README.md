# Getting Started 

This repository is a sample application to start with a Docker container.

The application is based on the application using NodeJs.
Before you can run the application, you need to get the application source code onto your machine.
1- Clone the repository using the following command:

   ```console
git clone https://github.com/didourebai/workshop1.git
   ```
2- View the contents of the cloned repository. You should see the following files and sub-directories.

 ```text
   ├── getting-started-app/
   │ ├── package.json
   │ ├── README.md
   │ ├── spec/
   │ ├── src/
   │ └── yarn.lock
   ```

## Build the app's image

To build the image, you'll need to use a Dockerfile. A Dockerfile is simply a text-based file with no file extension that contains a script of instructions. Docker uses this script to build a container image.

1. In the `getting-started-app` directory, the same location as the `package.json` file, create a file named `Dockerfile`. You can use the following commands to create a Dockerfile based on your operating system.

   In the terminal, run the following commands.

   Make sure you're in the `getting-started-app` directory. Replace `/path/to/getting-started-app` with the path to your `getting-started-app` directory.

   ```console
    cd workshop1
   ```

   Create an empty file named `Dockerfile`.

   ```console
    code Dockerfile
   ```

   In the Windows Command Prompt, run the following commands.

   ```console
    cd workshop1
   ```

   Create an empty file named `Dockerfile`.

   ```console
    type nul > Dockerfile
   ```

   In PowerShell, run the following commands.


   ```console
    cd workshop1
   ```

   Create an empty file named `Dockerfile`.

   ```powershell
   New-Item -Path . -Name Dockerfile -ItemType File
   ```


2. Using a text editor or code editor, add the following contents to the Dockerfile:

   ```dockerfile
   # syntax=docker/dockerfile:1
   
   FROM node:18-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   EXPOSE 3000
   ```

3. Build the image using the following commands:


   Build the image.
   ```console
    docker build -t getting-started .
   ```

   The `docker build` command uses the Dockerfile to build a new image. You might have noticed that Docker downloaded a lot of "layers". This is because you instructed the builder that you wanted to start from the `node:18-alpine` image. But, since you didn't have that on your machine, Docker needed to download the image.

   After Docker downloaded the image, the instructions from the Dockerfile are copied into your application and used `yarn` to install your application's dependencies. The `CMD` directive specifies the default command to run when starting a container from this image.

   Finally, the `-t` flag tags your image. Think of this as a human-readable name for the final image. Since you named the image `getting-started`, you can refer to that image when you run a container.

   The `.` at the end of the `docker build` command tells Docker that it should look for the `Dockerfile` in the current directory.

## Start an app container

Now that you have an image, you can run the application in a container using the `docker run` command.

1. Run your container using the `docker run` command and specify the name of the image you just created:

   ```console
    docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

   The `-d` flag (short for `--detach`) runs the container in the background.
   This means that Docker starts your container and returns you to the terminal
   prompt. You can verify that a container is running by viewing it in Docker
   Dashboard under **Containers**, or by running `docker ps` in the terminal.

   The `-p` flag (short for `--publish`) creates a port mapping between the host
   and the container. The `-p` flag takes a string value in the format of
   `HOST: CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is
   the port on the container. The command publishes the container's port 3000 to
   `127.0.0.1:3000` (`localhost:3000`) on the host. Without the port mapping,
   you wouldn't be able to access the application from the host.

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000).
   You should see your app.
   

3. Add an item or two and see that it works as expected. You can mark items as complete and remove them. Your frontend is successfully storing items in the backend.


At this point, you have a running to-do list manager with a few items.

If you take a quick look at your containers, you should see at least one container running using the `getting-started` image and on port `3000`. You can use the CLI or Docker Desktop's graphical interface to see your containers.


Run the following `docker ps` command in a terminal to list your containers.

```console
 docker ps
```
Output similar to the following should appear.
```console
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
df784548666d        getting-started     "docker-entry point.s…"   2 minutes ago       Up 2 minutes        127.0.0.1:3000->3000/tcp   priceless_mcclintock
```

In Docker Desktop, select the **Containers** tab to see a list of your containers.

