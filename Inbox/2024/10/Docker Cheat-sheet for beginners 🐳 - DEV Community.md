---
created: 2024-09-23T11:30:02 (UTC -03:00)
tags: [webdev,docker,beginners,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest
author: 
---

# Docker Cheat-sheet for beginners 🐳 - DEV Community

> ## Excerpt
> 🔧 Common Docker Commands     Start Docker:       systemctl start docker  # Linux   open -a...

---
[![Cover image for Docker Cheat-sheet for beginners 🐳](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxm0lscawycucd500c4t6.png)](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxm0lscawycucd500c4t6.png)

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#common-docker-commands)🔧 **Common Docker Commands**

-   **Start Docker**:

```
  systemctl start docker  <span># Linux</span>
  open <span>-a</span> Docker  <span># macOS</span>
```

-   **Check Docker Version**:

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#working-with-containers)📦 **Working with Containers**

-   **List Running Containers**:

-   **List All Containers (Running + Stopped)**:

-   **Run a Container** (starts and attaches):

-   **Run in Detached Mode**:

```
  docker run <span>-d</span> &lt;image_name&gt;
```

-   **Run with Port Mapping**:

```
  docker run <span>-p</span> &lt;host_port&gt;:&lt;container_port&gt; &lt;image_name&gt;
```

-   **Stop a Running Container**:

```
  docker stop &lt;container_id&gt;
```

-   **Start a Stopped Container**:

```
  docker start &lt;container_id&gt;
```

-   **Remove a Stopped Container**:

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#images)📜 **Images**

-   **List Docker Images**:

-   **Pull an Image from Docker Hub**:

-   **Build an Image from Dockerfile**:

```
  docker build <span>-t</span> &lt;image_name&gt; <span>.</span>
```

-   **Tag an Image**:

```
  docker tag &lt;image_id&gt; &lt;new_image_name&gt;:&lt;tag&gt;
```

-   **Remove an Image**:

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#container-management)🔄 **Container Management**

-   **View Logs of a Container**:

```
  docker logs &lt;container_id&gt;
```

-   **Access a Running Container (Interactive Shell)**:

```
  docker <span>exec</span> <span>-it</span> &lt;container_id&gt; /bin/bash
```

-   **Copy Files from Container to Host**:

```
  docker <span>cp</span> &lt;container_id&gt;:&lt;path_inside_container&gt; &lt;host_path&gt;
```

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#%F0%9F%8F%97-docker-networks)🏗 **Docker Networks**

-   **List Networks**:

-   **Create a Network**:

```
  docker network create &lt;network_name&gt;
```

-   **Connect a Running Container to a Network**:

```
  docker network connect &lt;network_name&gt; &lt;container_id&gt;
```

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#docker-compose)🐳 **Docker Compose**

-   **Start Services in Detached Mode**:

-   **Stop Services**:

-   **Build and Start Containers**:

```
  docker-compose up <span>--build</span>
```

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#inspecting-and-monitoring)📊 **Inspecting and Monitoring**

-   **Inspect Container Details**:

```
  docker inspect &lt;container_id&gt;
```

-   **Display Resource Usage (CPU, Memory)**:

#### [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#%F0%9F%9B%A0-volumes)🛠 **Volumes**

-   **List Volumes**:

-   **Create a Volume**:

```
  docker volume create &lt;volume_name&gt;
```

-   **Mount a Volume** (during `docker run`):

```
  docker run <span>-v</span> &lt;volume_name&gt;:&lt;path_inside_container&gt; &lt;image_name&gt;
```

___

💡 **Pro Tip**: Use `docker system prune` to remove unused containers, networks, and images.

Feel free to save or bookmark this cheat sheet for quick reference!

## [](https://dev.to/keshav___dev/docker-cheat-sheet-for-beginners-18mo?context=digest#docker-cheatsheet-containers-devops)Docker #CheatSheet #Containers #DevOps
