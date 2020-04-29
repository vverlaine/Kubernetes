# Kubernetes
## Proyecto de Sistemas Operativo II - Universidad Mariano Galvez

### Arquitectura
Inventario de servidores: 

![Arquitectura1](Arquitectura1.png)

![Arquitectura2](Arquitectura2.png)

### Requisitos

Configuración de máquina virtual para nuestro Balancer 
- Le asignamos 10G de disco duro
- Le asignamos 2G de RAM
- Le asignamos 3 Procesador
- Ubuntu 18.4

![Balancer](Balancer.png)

Configuración de máquinas virtuales para los Masters 
- Le asignamos 10G de disco duro
- Le asignamos 3G de RAM
- Le asignamos 3 Procesadores
- Ubuntu 18.4

![Master](Master.png)

Configuración de máquina virtual para los  esclavos
- Le asignamos 10G de disco duro
- Le asignamos 2G de RAM
- Le asignamos 2 Procesador
- Ubuntu 18.4

![Esclavo](Slave.png)

### Configuración Principal

- Actualizar el sistema operativo:
``` 
sudo apt update && sudo apt -y upgrade
```

- Entrar  como usuario root:
```
sudo su
```

- Instalar los paquetes para permitir que apt use el repositorio HTTPS:
```
apt-get update && apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg2 
```

- Agregar la llave GPG oficial de Docker :
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
```

- Agregar el repositorio de docker :
```
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

- Instalar Docker CE:
```
apt-get update && apt-get install -y containerd.io=1.2.13-1 \
docker-ce=5:19.03.8~3-0~ubuntu-$(lsb_release -cs) \
docker-ce-cli=5:19.03.8~3-0~ubuntu-$(lsb_release -cs)
```

- Configurar Demonio:
```
cat > /etc/docker/daemon.json <<EOF
{
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": {
"max-size": "100m"
},
"storage-driver": "overlay2"
}
EOF
```

- Crear la carptera del servicio de Docker:
```
mkdir -p /etc/systemd/system/docker.service.d
```

- Reiniciar el demonio:
```
systemctl daemon-reload
```

- Reiniciar Docker:
```
systemctl restart Docker
```

- Verificamos el estatus de nuestro Docker
```
systemctl status docker
```

### Instalación Kubernetes

