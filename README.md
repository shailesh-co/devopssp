 deploy.yml
- name: Deploy Docker Containers on Red Hat AWS
  hosts: aws_servers
  become: true
  tasks:
    - name: Check if Docker is installed
      command: docker --version
      register: docker_installed
      ignore_errors: true

    - name: Install Docker if not installed
      yum:
        name: docker
        state: present
      when: docker_installed.rc != 0

    - name: Start Docker service
      service:
        name: docker
        state: started
        enabled: yes

    - name: Log in to Docker Hub
      command: docker login -u shailesh01 -p ""
      register: docker_login
      changed_when: false

    - name: Pull Frontend Image from Docker Hub
      command: docker pull shailesh01/frontend-app:latest

    - name: Pull Backend Image from Docker Hub
      command: docker pull shailesh01/backend-app:latest

    - name: Stop and Remove Existing Backend Container (if exists)
      command: docker rm -f backend_container
      ignore_errors: yes

    - name: Stop and Remove Existing Frontend Container (if exists)
      command: docker rm -f frontend_container
      ignore_errors: yes

    - name: Run Backend Container
      command: docker run -d --name backend_container -p 5000:5000 shailesh01/backend-app:latest

    - name: Run Frontend Container
      command: docker run -d --name frontend_container -p 80:80 --link backend_container shailesh01/frontend-app:latest
########################################################################################


*** checkpassword elect vorrct

???????????????????????????????????????
inventory.ini     //file
[aws_servers]
localhost ansible_connection=local
docker need to Installation   Redghat system   


# 1. Remove Podman and all conflicts
sudo yum remove -y podman podman-docker containers-common

# 2. Clean yum cache
sudo yum clean all
sudo rm -rf /var/cache/yum

# 3. Install Docker
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io

# 4. Start and Enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# 5. Verify Docker Installation
docker --version
sudo systemctl status docker

# 6. Run Ansible Playbook
ansible-playbook -i inventory.ini deploy.yml



check the docker images

docker compose down 
docker-compose up -d
docker tag  <image-id> shailesh01/imagename
docker push  <image-id> shailesh01/iomageid


and pull this image friom in our ansible deploy file











