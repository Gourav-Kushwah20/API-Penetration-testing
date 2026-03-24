# 🐳 Docker Commands

## 📘 Basic Docker Information

#### 🔎 Check the installed version of `Docker`

```bash
docker --version
```

#### 📊 Display detailed information about Docker installation and environment

```bash
docker info
```

#### ❓ List all Docker commands or get help for a specific command

```bash
docker help
docker help run
```

---
## 🖼 Working with Docker Images

#### 📂 List all downloaded Docker images

```bash
docker images
```
#### ⬇️ Download (pull) an image from Docker Hub or another registry

```bash
docker pull <image>
docker pull hello-world
docker pull docker.io/kalilinux/kali-rolling
```

---

## 🗑 Remove a Docker Image

#### 🔥 Remove an image by ID or name

```bash
docker rmi <image_id>
docker rmi hello-world
```

#### ⚠️ Tip: Stop containers using the image before removing it

```bash
docker ps -a
docker stop <container_id>
docker rmi <image_name>
```

---

## 📦 Working with Containers

#### ▶️ List running containers

```bash
docker ps
```

#### 📋 List all containers (including stopped)

```bash
docker ps -a
docker container list --all
```

---

## 🚀 Create and start a container from an image

```bash
docker run <image>
docker run hello-world
docker run kalilinux/kali-rolling
```
---
## 🏃 Run Containers

#### 🔄 Run a container in detached (background) mode

```bash
docker run -d <image>
docker run -d hello-world
```

#### 🖥 Run a container interactively (with a terminal session)

```bash
docker run -it <image>
docker run -it kalilinux/kali-rolling
```

---

## ⚙️ Managing Containers

#### 🛑 Stop a running container

```bash
docker stop <container_id>
docker stop 30f1a2176463
```


#### 🔁 Restart a stopped container

```bash
docker start <container_id>
docker start kalilinux/kali-rolling
```



#### 🗑 Remove a stopped container

```bash
docker rm <container_id>
docker rm 3b52b6b21464

docker rm -f 4d951b2c1f55      # Force remove
docker rm -vf 98490d27bb28     # Remove volume + force
```

---

## 📜 View Logs of a Container

```bash id="t4x9pq"
docker logs <container_id>
docker logs a9b1498b2428
```

---

## 🖥 Accessing and Managing Containers

#### 🧠 Execute a command inside a running container

```bash id="m8k2zs"
docker exec -it <container_id> bash
docker exec -it 78e2eec292dd uname -a
```

> 💡 Very useful for API pentesting labs to inspect files, configs, and running services inside vulnerable containers.

---

## 🔗 Attach to a running container

```bash id="v6c1dr"
docker attach <container_id>
docker attach 78e2eec292dd
```

> ⚠️ Tip: Use `Ctrl + P + Q` to detach without stopping the container.

---

## 🧹 Cleaning Up Docker Resources

## 🛑 Stop all running containers

```bash
docker stop $(docker ps -aq)
```

## 🗑 Remove a single container

```bash id="k2j9fd"
docker rm <container_id>
docker rm -f 4d951b2c1f55        # Force remove
docker rm -vf 4d951b2c1f55       # Remove volume + force
```

---

## 🗑 Remove Specific or All Images

```bash id="rmi1x9"
docker rmi -f ec04eecaa36b
docker images -a
```

---

## 🔎 Remove Images Matching a Pattern

```bash id="grep7k2"
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```

> 💡 Useful when cleaning multiple lab images with similar names.

---

## 🧼 Pruning Unused Resources

#### 🧹 Remove unused containers

```bash id="prune1a"
docker container prune
```

#### 🖼 Remove unused images

```bash id="prune2b"
docker image prune -f
```



#### 💾 Remove unused volumes

```bash id="prune3c"
docker volume prune -f
```


#### 🌐 Remove unused networks

```bash id="prune4d"
docker network prune
```

---

## 💥 Completely clean the system

Removes:

* Stopped containers
* Unused networks
* Dangling images
* Build cache

```bash id="prune5z"
docker system prune -a
```

> ⚠️ Warning: This will remove all unused data. Use carefully in lab environments.

---

```bash
docker system prune --volumes
docker system prune --volumes --all
```

---

## 🧠 Quick Reference

| 🐳 Command                  | 📖 Description           |
| --------------------------- | ------------------------ |
| `docker ps`                 | List running containers  |
| `docker images`             | List images              |
| `docker exec -it <id> bash` | Access a container shell |
| `docker stop <id>`          | Stop container           |
| `docker start <id>`         | Start container          |
| `docker rm <id>`            | Remove container         |
| `docker rmi <id>`           | Remove image             |
| `docker system prune -a`    | Clean up everything      |

---

