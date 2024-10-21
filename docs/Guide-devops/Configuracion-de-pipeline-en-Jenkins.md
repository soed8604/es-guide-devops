---
id: Configuracion-de-pipeline-en-Jenkins
title: Configuracion de pipeline en Jenkins
description: Pasos para configurar un pipeline en Jenkins
---

# Crear un pipeline en jenkins para desplegar un cluster de EKS

:::important 
En esta fase, crearemos un pipeline que automatizará el despliegue y la destrucción de un clúster de Kubernetes (EKS) utilizando Jenkins para CI/CD y Terraform para Infraestructura como Código (IaC).
:::

## Paso 1: Configuración de pipeline en Jenkins e Instalación de Plugins 

Para recibir notificaciones utilizaremos como ejemplo `Slack`

    - Accedemos a `Administrar Jenkins`.
    - Seleccionamos la opción `Plugins`.
    - Luego, elegimos `Available plugins`
    - En la barra de búsqueda, ingresamos Slack y seleccionamos el plugin `Slack Notification`.
    - Se recomienda reiniciar Jenkins después de instalar el plugin.
    - Regresamos al panel de control para crear nuestro primer Job.
    - Asignamos un nombre a nuestro Job; en este ejemplo, utilizaremos `cicd-deploy-eks-terraform`
    - Seleccionamos la opción `pipeline`

        ![image](https://github.com/user-attachments/assets/a3c297e6-ff09-47b9-9ed2-21847ce3d078)
    - En el Job, aparecerán varias opciones. Debemos elegir `Esta ejecución debe parametrizarse`
    - En esta opción, añadimos un parámetro de tipo `Elección`
    - Lo llamaremos `action`
    - En el campo de opciones, ingresamos las palabras clave `apply` y `destroy`.

        ![image](https://github.com/user-attachments/assets/1acccb7c-465f-4519-8d2e-0459090f33be)
    - Seguidamente  no ubicamos en la opción `Elección` en definition seleccionamos `Pipeline script from SCM`.
    - En el campo SCM seleccionamos `Git` y colocaremos el repositorio de nuestro proyecto.
    - En le campo `Branch Specifier (blank for 'any')` colocamos la rama `main`.
    - Indicamos la ruta donde se encuentra nuestro archivo `Jenkinsfile` que para este caso se encuentra en la raíz del proyecto.

        ![image](https://github.com/user-attachments/assets/5c857355-3be9-44ff-aa36-3705b3676b9e)
    - Guardamos los cambios.
    - Seguidamente debemos crear los secretos que permitirá la conexión con `AWS`.
    - Nos ubicamos en el panel de control principal, seleccionamos la opción credentials.

        ![image](https://github.com/user-attachments/assets/607f777a-43d8-4bb4-b828-eefa136bd4a1)
    - Presionamos en la opción `(global)`.
    - Luego presionamos `add Credentials`.
    - El tipo de secreto que debemos seleccionar es `Secret text`.
    - En el campo secret debemos colocar el valor `AWS_ACCESS_KEY_ID` y en ID `AWS_ACCESS_KEY_ID`.
    - Salvamos los cambios.
    - Seguimos el mismo paso para la variable `AWS_SECRET_ACCESS_KEY`.
    - Salvamos los cambios.

        ![image](https://github.com/user-attachments/assets/6b868023-d67d-4bae-a36e-831a6e057d18)
    - La configuración debe quedar de la siguiente manera:

        ![image](https://github.com/user-attachments/assets/73597633-e5c1-40ad-9ba1-f1cb37f09ee0)

## Paso 2: Configuración de notificaciones en Jenkins

Como se mencionó anteriormente, para este escenario utilizaremos `Slack`. Sin embargo, es posible realizar configuraciones similares para otras herramientas de notificación, como `MS Teams`.
    - Nos dirigimos al panel de control principal y seleccionamos `Administrar Jenkins`.
    - Elegimos la opción `System`.
    - Ubicamos el apartado `Slack`.

        ![image](https://github.com/user-attachments/assets/2495117a-89de-4948-a960-5272796323dd)
    - Llenar los campos `Workspace` `credential` `Default channel` 
    - Esta información la podemos obtener de Slack
    - Nos ubicamos en la aplicación Slack y creamos un `Workspace` en este ejemplo utilizaré `soed8604`.
    - Creamos un canal para este ejemplo `mono-notificacions`.

        ![image](https://github.com/user-attachments/assets/58e2404b-b186-43c1-843e-47b853f45a12)
    - Luego nos ubicamos en el icono del perfil, presionamos y nos aparece un recuadro con una pestañas y seleccionamos `integrations` y `add an App`.

        ![image](https://github.com/user-attachments/assets/e7e010f5-3060-428b-b83f-b81d64b419f2)
    - Elegimos `Jenkins CI` y luego `configuration`
    - Esto nos llevará a un portal web donde elegimos el canal que creamos previamente para recibir las notificaciones.
    - A continuación, se nos redirige a una guía que proporciona la información necesaria para configurar Jenkins.
    - Procedemos a ingresar esos valores en Jenkins.
    - Si los datos ingresados son correctos, puedes realizar una prueba de conexión `Test Connection`, la cual debería resultar en `Succes`.

        ![image](https://github.com/user-attachments/assets/db364d6c-bb74-41d3-be23-5219502d5885)
    - Salvamos los cambios.
    - Luego de realizar estas configuraciones, estaremos listos para probar nuestro pipeline.
    - Ubicamos nuestro job `cicd-deploy-eks-terraform`, lo seleccionamos y luego elegimos la opción `Build with Parameters`.

        ![image](https://github.com/user-attachments/assets/e81a3ff5-4142-4c81-a824-9ec7c4bde326)
:::important
    En la prueba técnica se solicita que se realice el build del clúster EKS mediante un push. Sin embargo, para facilitar la creación y destrucción del clúster, se ha implementado un parámetro que permite seleccionar la acción a realizar.
:::

    - Cuando presionemos `Build with Parameters`, nos aparecera dos opciones `apply` y `destroy`

        ![image](https://github.com/user-attachments/assets/ca3d1ec7-bece-4629-9a6a-aefd6bfd7ddf)
    - En este caso elegimos `apply` para la creación automatizada del cluster.
    - Si toda la configuración se realizo correctamente el proceso de creación puede demorar aproximadamente 15 minutos.

        ![image](https://github.com/user-attachments/assets/935329dd-6a81-4185-9e0b-0c5805ed133e)

        ![image](https://github.com/user-attachments/assets/0a4d6f6a-8486-4c2b-b4a2-8d5bb7793d36)

        ![image](https://github.com/user-attachments/assets/f06331bb-b91b-41f4-9be4-a19d3e8864eb)
