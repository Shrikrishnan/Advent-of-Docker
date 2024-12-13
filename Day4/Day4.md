
# Day 4 - Building Your First Container  
**Published:**  
Dec 4, 2024 | 12:00 AM  

It is finally time to build your first container! Today we will go through the process of dockerizing a simple application.  

We will use a simple Go application that prints “Hello, World!” to the console. I purposefully chose Go because it is a very simple language, and you probably don’t have it installed yet. This will show you one of the benefits of dockerizing your applications—you can use any language you want without actually needing to install it on your machine!  

## Step 1: Create a Go Application  

To get started, create a new directory for your project and navigate into it. Then create a new file called `main.go` and add the following code:  

```go
package main  

import "fmt"  

func main() {  
	fmt.Println("Hello, World!")  
}
```  

As you can probably see, this is only going to print “Hello, World!” to the console and then exit. Baby steps!  

If you have Go installed, you can execute the code with:  

```bash
$ go run main.go  
Hello, World!  
```  

I don’t expect you to actually run this on your machine; we will use Docker to build and run it instead!  

## Step 2: Create a Dockerfile  

Now that we have our application, we can start to dockerize it. To do this, create a file called `Dockerfile` and add the following instructions:  

```dockerfile
FROM golang  

COPY . .  

RUN go build -o main main.go  

CMD ["./main"]  
```  

### Explanation of the Dockerfile  

A Dockerfile is a simple text file that contains a set of instructions for Docker on how to build and run your application. It is executed in order from top to bottom.  

1. **`FROM golang`**  
   Tells Docker to use the official Golang image as a base image. This image already contains the Go compiler and other dependencies needed to build our application.  

   ```dockerfile
   FROM golang  
   ```  

2. **`COPY . .`**  
   Copies all files from the current directory into the image.  

   ```dockerfile
   COPY . .  
   ```  

3. **`RUN go build -o main main.go`**  
   Builds our application and names the output file `main`.  

   ```dockerfile
   RUN go build -o main main.go  
   ```  

4. **`CMD ["./main"]`**  
   Specifies that Docker should run the `main` executable when the container starts.  

   ```dockerfile
   CMD ["./main"]  
   ```  

## Step 3: Build the Docker Image  

To create an image from the Dockerfile, run the following command:  

```bash
$ docker build -t hello-world-go .  
```  

This will create a new image named `hello-world-go`. The dot (`.`) at the end tells Docker to use the current directory as the build context.  

The first time you run this command, Docker will download the Golang base image from Docker Hub, which might take a while. Subsequent builds will be faster because Docker caches steps that don’t change.  

You can verify the image was created by listing all images:  

```bash
$ docker images  
REPOSITORY          TAG                 IMAGE ID            CREATED  
hello-world-go      latest              <image-id>          <timestamp>  
```  

## Step 4: Run the Docker Container  

To run your container, use the `docker run` command:  

```bash
$ docker run hello-world-go  
Hello, World!  
```  

This starts a new container from the image and executes the `main` executable, as defined in the `CMD` instruction of the Dockerfile.  

Since the application simply prints "Hello, World!" and exits, the container stops immediately.  

You can check the container’s status using:  

```bash
$ docker ps -a  
CONTAINER ID     IMAGE            COMMAND     CREATED          STATUS  
<container-id>   hello-world-go   "/main"     <timestamp>      Exited (0)  
```  

The `--all` flag shows all containers, including those that have exited.  

## Experiment with Your Dockerfile  

Now it’s your turn to play around with this setup! Here are some ideas:  

- Change the `CMD` instruction to `RUN go run main.go` and see what happens. Does it still work? Why?  
  - getting below error message 
    - docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "./main": stat ./main: no such file or directory: unknown.

- Modify the `RUN` instruction. What happens if you remove the `-o main` part?  
  - Getting Hello, World!
- Add a new `COPY` instruction to include other files in the image. Does it work? 
  - Yes, it is working
- Explore the `WORKDIR` instruction and try to use it in your Dockerfile.
  - Yes, it is working


## What’s Next?  

Tomorrow, we’ll take a deeper look at what happened when we built our image and learn more ways to interact with containers. If you want a head start, check out the `docker exec` command and its capabilities!  

Until then, happy hacking! 🚀  

