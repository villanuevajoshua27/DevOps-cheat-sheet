# Pushing and Pulling Docker Images with Docker Hub

This documentation outlines the steps to effectively manage your Docker images by pushing them to and pulling them from Docker Hub, Docker's cloud-based registry service. 

---

## Step 1: Create a Repository on Docker Hub

First, you'll need a place to store your images on Docker Hub.

1.  **Sign in** to your Docker Hub account. If you don't have one, you can [sign up here](https://hub.docker.com/).
2.  Once logged in, navigate to the "Repositories" section and **create a new repository**. Choose a descriptive name that reflects the image it will contain. 

---

## Step 2: Push Your Docker Image to Docker Hub

After creating a repository, you can push your local Docker images to it.

1.  **Open your terminal or command prompt** and sign in to Docker Hub using your credentials. When prompted, enter your Docker Hub password.
    ```bash
    docker login -u <YOUR_DOCKERHUB_USERNAME>
    ```
    Replace `<YOUR_DOCKERHUB_USERNAME>` with your actual Docker Hub username.

2.  **Tag your local Docker image.** Before pushing, you must tag your image with the correct repository name and a new tag (version). This associates your local image with the remote repository on Docker Hub.
    ```bash
    docker tag <LOCAL_IMAGE_NAME>:<LOCAL_IMAGE_TAG> <YOUR_DOCKERHUB_USERNAME>/<REPOSITORY_NAME>:<NEW_IMAGE_TAG>

    # Example: Tagging a local nginx:alpine image for a repository named 'my-app'
    docker tag nginx:alpine javillanueva/my-app:nginx-alpine
    ```
    * `<LOCAL_IMAGE_NAME>`: The name of your local Docker image (e.g., `nginx`).
    * `<LOCAL_IMAGE_TAG>`: The current tag of your local image (e.g., `alpine`).
    * `<YOUR_DOCKERHUB_USERNAME>`: Your Docker Hub username.
    * `<REPOSITORY_NAME>`: The name of the repository you created on Docker Hub (e.g., `my-app`).
    * `<NEW_IMAGE_TAG>`: A new tag you want to assign for the image in Docker Hub (e.g., `nginx-alpine`). This is often used for versioning.

3.  **Push the tagged image to Docker Hub.** This command uploads your local image to the specified repository.
    ```bash
    docker push <YOUR_DOCKERHUB_USERNAME>/<REPOSITORY_NAME>:<NEW_IMAGE_TAG>

    # Sample: Pushing the previously tagged nginx image
    docker push javillanueva/my-app:nginx-alpine
    ```
    You'll see progress indicators as the image layers are uploaded.

---

## Step 3: Run Your Docker Image on a New Instance (Pulling)

Once your image is on Docker Hub, you can pull and run it on any machine with Docker installed.

1.  **Pull the Docker image** from Docker Hub to your new instance.
    ```bash
    docker pull <YOUR_DOCKERHUB_USERNAME>/<REPOSITORY_NAME>:<IMAGE_TAG>

    # Sample: Pulling the nginx-alpine image
    docker pull javillanueva/my-app:nginx-alpine
    ```
    This command downloads the specified image from your Docker Hub repository to your local machine.

2.  **Run the pulled image.** After pulling, you can run the image as a container.
    ```bash
    docker run -dp 0.0.0.0:80:80 <YOUR_DOCKERHUB_USERNAME>/<REPOSITORY_NAME>:<IMAGE_TAG>

    # Example: Running the nginx-alpine container, mapping port 80
    docker run -dp 0.0.0.0:80:80 javillanueva/my-app:nginx-alpine
    ```
    * `-d`: Runs the container in **detached mode** (in the background).
    * `-p 0.0.0.0:80:80`: **Maps port 80** of the host machine to port 80 of the container. This allows external access to the application running inside the container. You can adjust the host port as needed (e.g., `-p 8080:80`).
    * `<YOUR_DOCKERHUB_USERNAME>/<REPOSITORY_NAME>:<IMAGE_TAG>`: The full name and tag of the image you want to run.

---

For further details and a hands-on workshop, refer to the official Docker documentation: [Sharing your app](https://docs.docker.com/get-started/workshop/04_sharing_app/)
