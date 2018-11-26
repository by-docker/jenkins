## INSTALLATION GUIDE

### AWS Lightsail

Minimum requirement 
* 2 GB RAM
* 1 vCPU
* 60 GB SSD

### Install Docker
> Refer to [article on medium.com](https://medium.com/@JoshuaTheMiller/creating-a-simple-website-with-a-custom-domain-on-amazon-lightsail-docker-86600f19273
)

```bash
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
sudo apt-get -y update
sudo apt-get -y install docker-ce
docker --version
```

* add to docker group
```bash
sudo usermod -aG docker $USER
```

* log out and log back in

### Install Jenkins
* configure Lightsail Instance
> add another _Custom TCP_ port 8080 in addition to SSH and HTTP

* install make
```bash
sudo apt install make
```
* build jenkins
```bash
git clone https://github.com/by-docker/by-docker.github.io
cd by-docker.github.io/jenkins
make container 
```

* run jenkins
```bash
cd by-docker.github.io/jenkins
make run
```

* clean up
```bash
docker rmi `docker images | grep "^<none>" | awk '{print $3}' | xargs` 2>/dev/null || echo 'cleaning unused images'
docker rm `docker ps -aq -f status=exited | xargs` 2>/dev/null || echo 'cleaning exited docker processes'
docker rm `docker ps -aq -f status=dead | xargs` 2>/dev/null || echo 'cleaning dead docker processes'
```

### Prepare for Jupyter Notebooks

* create notebooks directory
```bash
mkdir notebooks
```

* configure Lightsail Instance
> add another _Custom TCP_ port 8888