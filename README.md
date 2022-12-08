# tektonPipeline
Proyecto de tesis utilizando los pipelines de Tekton y Jenkins X


- Node.js
- Git
- Docker 
- Kubernetes-Minikube
- Tekton

Instalación de herramientas para la distribución de linux Ubuntu 22.04 LTS. 

Node.js
```
curl -fsSL https://deb.nodesource.com/setup_19.x | sudo -E bash - &&\ ;
sudo apt-get install -y nodejs;
node -v;
node -e "console.log('Hola mundo')";
```

para distribuciones Debian utilizar el siguiente comando como root 

```
curl -fsSL https://deb.nodesource.com/setup_19.x | bash - &&\
apt-get install -y nodejs

```


Git
```
sudo apt-get update;
sudo apt install git-all -y;
git --version;
```

Docker 

Nos aseguramos de desinstalar cualquier versión instalada para realizar una instalación limpia 
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
Actualizamos los paquetes para utilizar el repositorio de instalación
```
sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y
```
Agregamos la llave oficial de docker para tener una aplicación certificada y segura
```
sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
 configuramos el repositorio
 ```
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
  actualizamos nuevamente el repositorio
 ```  
   sudo apt-get update
```
   otorgamos permisos para la llave
```
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update
 ```
Comenzamos la instalación de docker engine
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```
verificamos una instalación correcta 
``` 
 sudo docker run hello-world
```
para poder utilizar docker sin ser usuario root, agregamos un grupo de usuario llamado docker para poder utilizarlo sin acceder al usuario root
```  
  sudo groupadd docker
```  
  luego agregamos nuestro usuario al grupo creado
```  
  sudo usermod -aG docker $USER
```
  si se esta utilizando una máquina virtual hay que reiniciarla para que tengan efecto los cambios o se puede utilizar el siguiente comando
```  
  newgrp docker
``` 
 verificamos que se pueda utilizar docker sin sudo 
```  
  docker run hello-world
```
para inicializar docker utilizamos los siguientes comandos 
``` 
 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
```
para parar o deshabilitar nuestro servicio de docker utilizamos
```
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
Kubernetes, Minikuber y Tekton

Para la instalación de minikube y  tekton dentro de kubernetes utilizamos los siguientes comandos 
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64;
sudo install minikube-linux-amd64 /usr/local/bin/minikube;
```
 iniciamos minikube con 
 ```
 minikube start
 ```
una vez iniciado minikube podemos proceder a instalar las herramientas de tekton
para la instalación de tekton CLI
```
sudo apt update;sudo apt install -y gnupg
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3EFE0E0A2F2F60AA
echo "deb http://ppa.launchpad.net/tektoncd/cli/ubuntu eoan main"|sudo tee /etc/apt/sources.list.d/tektoncd-ubuntu-cli.list
sudo apt update && sudo apt install -y tektoncd-cli
```
para la instalación del dashboard
```
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml
```
para verificar la instalación usamos 
```
kubectl get pods --namespace tekton-pipelines --watch
```
para acceder al dashboard de tekton utilizamos 
```
kubectl proxy
```
o podemos abrir un puerto para su visualización con
```
kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```


Instalación de SCRIPT 

```
