---
id: instalar_un_server_jenkins_en_ec2
title: Instalar un servidor Jenkins en EC2
description: Cómo instalar Jenkins en una instancia de EC2.
---

# Instalar un servidor Jenkins en EC2

:::important 
Esta instalación se realizará utilizando Terraform, una herramienta de infraestructura como código. Asegúrate de tener un buen conocimiento de Terraform antes de continuar, ya que cualquier cambio en la configuración puede afectar los recursos de AWS y generar costos no deseados.
:::

En esta guía, aprenderás a instalar Jenkins en una instancia de Amazon EC2 usando Terraform. Jenkins es un servidor de automatización de código abierto que ayuda a automatizar partes del desarrollo de software.

## Requisitos previos

Antes de comenzar, asegúrate de tener instalados y configurados los siguientes requisitos:

- **Git**
     - [Documentación de Git](https://git-scm.com/doc)
- **AWS CLI**
     - [Documentación de Git](https://aws.amazon.com/cli/)
- **Cuenta de AWS**: Asegúrate de tener una cuenta activa de Amazon Web Services (AWS).
    - [Documentación de la Consola de AWS](https://docs.aws.amazon.com/console)
- **Terraform v1.9.8**
    - [Documentación de Terraform](https://developer.hashicorp.com/terraform/install)
- **Visual Studio Code (VSCode)**
    - [Documentación de VSCode](https://code.visualstudio.com/docs)


## Paso 1: Clonar repositorio

Para clonar el repositorio `git@github.com:soed8604/Proyecto_Mono.git`, sigue estos pasos:

1. **Abre tu terminal** en tu máquina local.

2. **Navega al directorio** donde deseas clonar el repositorio. Por ejemplo, si deseas clonarlo en un directorio llamado `proyectos`, puedes hacerlo así:

   ```bash
   cd ~/proyectos
3. **Clona el repositorio utilizando el siguiente comando:**
   ```bash
   git clone git@github.com:soed8604/Proyecto_Mono.git
4. **Ingresa al directorio del repositorio clonado:**
   ```bash
   cd Proyecto_Mono
:::important
Asegúrate de que tienes configurada una clave SSH en tu máquina y que está añadida a tu cuenta de GitHub, ya que estás utilizando la URL SSH (`git@github.com:...`). Si no tienes configurada la clave SSH, puedes optar por usar la URL HTTPS:
:::

## Paso 2: Ingresar a AWS Console

- **Crear un usuario de servicio en IAM** 

    ![Image 1](https://github.com/user-attachments/assets/af590aa0-8a8f-4670-b0b3-d44ea52d3512)

    ![Image 2](https://github.com/user-attachments/assets/0bdbc617-c68b-4107-ba9c-d5d83e1a1743)

    ![Image 3](https://github.com/user-attachments/assets/cd039293-fa18-4d7d-8715-bb668675ba6b)

    ![Image 4](https://github.com/user-attachments/assets/02c5c440-b28f-4ba2-add2-f3808570ebe6)

    ![Image 5](https://github.com/user-attachments/assets/7e2c52ba-0d2f-41b1-bdba-cb457d68a895)

    ![Image 6](https://github.com/user-attachments/assets/b1fd493b-bc30-40df-bc5c-fcf821bfce56)

    ![Image 7](https://github.com/user-attachments/assets/894bac80-4474-4e00-b0b6-ae186720c482)

    ![Image 8](https://github.com/user-attachments/assets/b00e9303-8bf6-46ea-a02b-896117edc42d)

- **Crear un bucket de S3 para almacenar el archivo tfstate de Terraform de forma segura** 

    ![image](https://github.com/user-attachments/assets/747ab7bd-6c6f-4be3-aa24-9ecbe9c6b214)

    ![image](https://github.com/user-attachments/assets/193381f5-9c80-446a-8a56-0fe8837ad21c)

    ![image](https://github.com/user-attachments/assets/b6f4c171-cb52-46b3-be37-907fb8d7daf0)

- **Crear un key pair**
    :::important
    Este par de claves será utilizado por el usuario previamente creado para interactuar con la instancia EC2 y realizar la instalación de los paquetes necesarios para asegurar el correcto funcionamiento de Jenkins.
    :::

    ![image](https://github.com/user-attachments/assets/9e9a7be4-00f7-4c06-9511-503b7beb3498)

    ![image](https://github.com/user-attachments/assets/555b0c6a-6b37-448a-9bb3-efac12376f8d)

    ![image](https://github.com/user-attachments/assets/a94d36df-69f4-4e17-b7e1-8b4f2455d078)

## Paso 3: Terraform

:::important
    Para el despliegue del cluster EKS utilizamos la documentación oficial de terraform [Enlace de documentación](https://developer.hashicorp.com/terraform/tutorials/kubernetes/eks).
:::

En este paso nos devolvemos a la consola en donde clonamos nuestro proyecto. El proyecto esta desarrollado en tres pasos.

    - **01-jenkins-server**
    - **02-mono-eks-cluster**
    - **03-k8s-teslo**

Ingresamos al directorio `01-jenkins-server` y debemos realizar los siguientes ajustes al código:

En el archivo `backend.tf`, es necesario especificar el nombre del bucket de S3 que creamos previamente para almacenar de forma segura el estado `tfstate.tf` de Terraform.

![image](https://github.com/user-attachments/assets/9b17d0bd-62e1-4364-8e27-b7d5da119040)

En el archivo `main.tf`, dentro del módulo de la instancia EC2, es necesario especificar el nombre del `key pair` que hemos creado previamente para permitir el acceso seguro a la instancia.

![image](https://github.com/user-attachments/assets/283cd23b-8f37-4367-b562-eaee85a25239)

Después de completar la configuración, ejecutaremos los siguientes comandos para aplicar los cambios:

```bash
# Inicializar Terraform
terraform init
```
```bash
# Validate la configuración de terraform
terraform validate
```
```bash
# formatear el código de terraform
terraform fmt
```
```bash
# Revisar el plan de despliegue de la infraestructura 
terraform plan
```
```bash
# Aplicar el despliegue de la infraestructura 
terraform apply
```
Para destruir la infraestructura creada se ejecuta el comando:
```bash
# Aplicar el despliegue de la infraestructura 
terraform destroy
```
## Paso 3: Jenkins

    - Ingresamos a la consola de AWS
    - Nos ubicamos en el servicio EC2 y verificamos que se haya desplegado nuestra instancia de `Jenkins-Server`
    - Seleccionamos la instancia y buscamos la información de la IP pública que nos permitira el acceso al dashboard de jenkins

        ![image](https://github.com/user-attachments/assets/3950f27d-7f52-45fc-bb3a-58ef075562da)

    - Abrimos un navegador y colocamos la `IP Pública:8080`

        ![image](https://github.com/user-attachments/assets/05fec8f8-861b-44cd-8265-bd416e12f749)
    - Copiamos la ruta `/var/lib/jenkins/secrets/initialAdminPassword` y nos devolvemos a la consola de aws para ingresar a la instancia EC2.
    - Seleccionamos la instancia y presionamos la opcion conectar y elegimos la primera opción `EC2 instance Connect` y presionamos conectar, esto abrira una terminal donde podremos interactuar con el servidor.
    - Ejecutar el comando `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` esto nos indicará el password para ingresar
    - Copiamos y pegamos el password en el dashboard, luego instalamos los plugins sugeridos.

        ![image](https://github.com/user-attachments/assets/1206dc5a-eed2-41be-b248-171db906ba94)
    - Crear un nuevo usuario y password

        ![image](https://github.com/user-attachments/assets/a941b2f5-9966-4987-9d80-4d07acedec12)
    - Presionamos siguientes a los demas items hasta llevarnos a la pantalla principal de Jenkins

        ![image](https://github.com/user-attachments/assets/454b0f22-aa13-4649-8343-02a2de8c59da)
    - Esto garantiza la creación adecuada del servidor de Jenkins.





    
    
    

    









