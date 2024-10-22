---
id: Configuracion-de-pipeline-en-jenkins-deploy-app
title: Configuracion de pipeline en Jenkins para deployar un app en un cluster de EKS
description: Pasos para configurar un pipeline en Jenkins para el despliegue de la applicación
---

# Crear un pipeline en jenkins para desplegar una aplicación en un cluster de EKS

:::important 
En esta fase, crearemos un pipeline que automatizará el despliegue de una aplicación en un cluster de EKS y los archivos de nuestra aplicación se encuentran en el directorio `03-k8s-teslo`.
:::

## Paso 1: Crear un job

1. **Acceder a la consola de Jenkins:**
    - Ingresa a la consola de Jenkins con tus credenciales.
2. **Crear un nuevo Job:**
    - Desde el panel de control principal, selecciona la opción `Nueva Tarea`.
    - Asigna un nombre a tu nuevo job, por ejemplo: `cicd-deploy-application-api`.
    - Selecciona el tipo de proyecto `pipeline`.
    - Haz clic en `Guardar` para guardar los cambios.
3. **Configurar el Pipeline:**
    - Una vez dentro de la configuración del job, ve a la sección `pipeline`.
    - En el campo Definition, selecciona la opción `Pipeline script from SCM`.
    - En el campo SCM, elige `Git` como sistema de control de versiones.
4. **Configurar el repositorio Git:**
    - En el campo `Repositories`, introduce la URL del repositorio de tu proyecto. Ejemplo: `https://github.com/soed8604/Proyecto_Mono.git`.
    - En el campo `Branch Specifier (Blank for any)`, indica la rama de Git que deseas utilizar (por ejemplo, main o develop).
    - Selecciona el tipo de proyecto `pipeline`, indica la rama de Git que deseas utilizar (por ejemplo, `main`).
    - Haz clic en `Guardar` para guardar los cambios.
5. **Configurar el Jenkinsfile:**
    - En el campo `Script Path`, especifica la ruta del archivo `Jenkinsfile` en tu repositorio. Ejemplo: Jenkinsfile (o la ruta completa si está en un subdirectorio).
6. **Configurar el Jenkinsfile:**
    - En el campo `Script Path`, especifica la ruta del archivo `Jenkinsfile` en tu repositorio. Ejemplo: Jenkinsfile (o la ruta completa si está en un subdirectorio).

    ![image](https://github.com/user-attachments/assets/e87b2576-59b1-4fba-8dd7-3ce36ecfdb5b)

    ![image](https://github.com/user-attachments/assets/e87b2576-59b1-4fba-8dd7-3ce36ecfdb5b)

    ![image](https://github.com/user-attachments/assets/97ab317b-dbab-418a-85ff-15ab6b977210)

## Paso 2: Ejecución del Job

Al guardar los cambios, serás redirigido al panel de control principal, donde verás tu nuevo job. Selecciónalo y haz clic en la opción `Construir ahora` para iniciar la ejecución del pipeline.

    ![image](https://github.com/user-attachments/assets/97ab317b-dbab-418a-85ff-15ab6b977210)

    ![image](https://github.com/user-attachments/assets/7825d9dc-d7a9-4c1d-a3d6-6662e4d9aee8)

Este job también incluye una configuración de notificaciones, lo que te permitirá recibir alertas sobre el estado del job, ya sea que haya finalizado con éxito `Success` o que haya fallado `Failure`.



