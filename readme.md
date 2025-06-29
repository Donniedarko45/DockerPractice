# DOCKER PRACTICE

### commands to build with docker is

- docker built -t nameofProject .
- docker tag docker nameofProject donniedarko45/nameofProject:latest
- docker push donniedarko45/nameofProject

## Dockerfile

```dockerfile
FROM node:20
```

This line specifies the base image for the new image you are building. It tells Docker to use the official `node` image from Docker Hub with the tag `20`. This means your container will be based on an environment that has Node.js version 20 pre-installed.

```dockerfile
WORKDIR /app
```

This sets the working directory inside the container to `/app`. All subsequent commands in the Dockerfile (`RUN`, `CMD`, `COPY`, etc.) will be executed from this directory. If the `/app` directory doesn't exist, it will be created.

```dockerfile
COPY package* .
```

This command copies files from your local machine (the build context) into the container's working directory (`/app`). The `package*` is a wildcard that will copy any file in the root of your project whose name starts with "package". This typically includes `package.json` and `package-lock.json`. These files are copied before the rest of the source code to take advantage of Docker's layer caching. If these files haven't changed between builds, Docker can reuse the layer from the previous build, which speeds up the image creation process.

```dockerfile
RUN npm install
```

This command executes `npm install` inside the container. This will download and install all the dependencies defined in your `package.json` file.

```dockerfile
COPY . .
```

This copies all the files and folders from the current directory on your local machine (the build context) into the container's working directory (`/app`). This is done after `npm install` so that if you only change your source code, Docker doesn't need to re-run `npm install`.

```dockerfile
RUN npm run build
```

This command executes the `build` script defined in your `package.json` file. This is often used to compile code (like TypeScript to JavaScript) or to bundle assets for production.

```dockerfile
EXPOSE 3000
```

This instruction informs Docker that the container will listen on port 3000 at runtime. This is primarily for documentation purposes and does not actually publish the port. To make the port accessible from outside the container, you need to use the `-p` or `-P` flag when you run the container.

```dockerfile
CMD ["node", "dist/index.js"]
```

This specifies the default command to run when the container starts. In this case, it will execute `node dist/index.js`, which starts your Node.js application. There can only be one `CMD` instruction in a Dockerfile. If you have multiple, only the last one will be executed.

