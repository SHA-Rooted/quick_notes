# Docker 

Notes

	images is like iso file 
	container is like the virtual machine
		Container can be classified by two ways
			1. Container_ID
			2. Container_name

Commands :-  

- To add user in Docker group 
```
sudo usermod -aG docker $USER
```

- shows the existing containers
```bash
docker ps -a 
```
- shows existing images from which container can be made
```bash
docker images
```

- To download an image
```bash
docker pull <image_name>
```

- To create image manually 
```bash
docker bulid . -t <images_name>
```

- create container in the background with the given image
```bash
docker run -it -d --name <container_name> <image_name> bash

# Options meaning 
    -it   
			i - interactive
			t - terminal/tty

    -d  - background
    --name - to create container name 
```

- To start a existing container 
```bash
docker exec -it <existing_container_ID_or_name> /bin/bash
```

- To start a container if you have stoped it
```bash
docker start <existing_container_ID_or_name>
```

- To run a ngnix server on port 
```bash
docker container run -it -d -p 8000:80 nginx
```


