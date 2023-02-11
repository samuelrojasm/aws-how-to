# Introducción
- El objetivo es crear un pipeline de CI/CD y para nuestra infraestructura usando los servicios de AWS, siguiendo el principio de IaC, por lo que el pipeline creado será probado con el propio código de infraestructura que crea el pipeline para que cualquier proyecto que despliegue infraestructura con AWS IaC pueda usar este mismo pipeline.
- Los beneficios que se obtienen es mejorar la gestión de nuestra infraestructura en un sistema de control de versiones y se espera que la infraestructura creada sea replicable y fiable.
- Se usará AWS Cloud Development Kit (AWS CDK) que es un marco de desarrollo de software de código abierto para definir los recursos de una aplicación en la nube utilizando lenguajes de programación conocidos.

## Step 1: Pre-requisitos
El concepto de stack es heredado de AWS CloudFormation y lo primero que vamos a hacer es crear el proyecto de AWS CDK.

1. Instalar AWS-CLI
1. Configurar credeenciales de manera local
1. Instalar NodeJS
1. Instalar CDK (global)
1. Crear un repositorio vacio en Git Hub(por ejemmplo: aws-codepipeline-project) y clonarlo

## Step 2: Setup del proyecto CDK
### 1. Setup inicial de Code Base del proyecto CDK
- Movernos a la carpeta en donde se clonó el repositorio vacio, por ejemmplo: aws-codepipeline-project
- Abrir la carpeta en VSCode
- Inicializar la aplicación CDK en la carpeta creada
```bash
cdk init app --language python
``` 
- Useful commands:
    * `cdk ls`          list all stacks in the app
    * `cdk synth`       emits the synthesized CloudFormation template
    * `cdk deploy`      deploy this stack to your default AWS account/region
    * `cdk diff`        compare deployed stack with current state
    * `cdk docs`        open CDK documentation

- Activar el ambiente virtual de python:
```console
source .venv/bin/activate
```
- Actualizar el archico `.gitignore` incluir las recomendaciones de la siguiente liga:
[Algunas configuraciones comunes de .gitignore](https://gist.github.com/octocat/9257657)

- Hacer el commit y push inicial
```console
git add .
git commit -m "commit inicial"
git branch -M main
git push -u origin main
```

### 2. Crear los stacks con AWS CDK
- El concepto de stack es heredado de AWS CloudFormation. Lo primero es crear el proyecto de AWS CDK.
- En el archivo `app.py` se crean nuestros stacks. En este caso es el stack que creará el pipeline usando los servicios de AWS de Developer Tools.
- AWS CodePipeline es un servicio de CI/CD que se usa para **modelar, visualizar y automatizar** los pasos necesarios para lanzar su software. Puede diseñar y configurar rápidamente las diferentes etapas de un proceso de lanzamiento de software. CodePipeline automatiza los pasos necesarios para publicar los cambios en el software de manera continua. Se puede integrar fácilmente con servicios de terceros, como GitHub.
- Configurar el stack pipeline (AWS CodePipeline) que se requiere para este proyecto, en el archivo `pipeline_stack.py`

### 3. Definición de configuración de pipeline (AWS CodePipeline)
- Definir el trigger que activará el pipeline.Cuando se hace un push en el repositorio se ejecutará el pipeline automáticamente. Hay que seleccionar el branch que ejecutará el trigger.
- Es necesario también definir el respositorio que se va usa puede ser Git Hub ó AWS CodeCommit.
- Cuando añadimos el repositorio de código a AWS CodePipeline a través de CDK, se crea una regla de eventos de AWS CloudWatch entre el repositorio y el pipeline:
[Source actions and change detection methods](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-about-starting.html#change-detection-methods)
- Definir un rol de AWS IAM que de permisos al pipeline para poder crear los recursos de infraestructura que hemos definido. Cuando una pipeline es creado, se crea un rol de servicio o se utiliza un rol de servicio existente.

### 4. Definir los pasos del pipeline (AWS CodePipeline)
- Crearemos tres pasos con AWS CodeBuild. AWS CodeBuild es un servicio de integración continua totalmente administrado que compila el código fuente, ejecuta pruebas y produce paquetes de software listos para su implementación. 

    - **Paso1** - Build: Descargamos las dependencias necesarias para utilizar AWS CDK y su lenguaje de programación asociado, en este caso Python.
        - Creamos y configuramos el paso de CodeBuild
        - Se adiciona al pipeline en el orden requerido. Esta acción se repite para todos los pasos adicionales que se quieran añadir en el pipeline.
        - Cuando creamos un paso de codebuild, necesitamos añadir la especificación del build, que al final será lo que se ejecute en este paso. Se crean los **build_specs**
        - La configuración de **build_specs** se localiza en el archivo **build.yml** dentro de la carpeta **build_specs**
    
    - **Paso2** - Validar la infraestructura: cdk diff. Paso para comprobar que los nuevos cambios son correctos.
        - El comando cdk diff compara la versión actual de un stack definido en al aplicación con la versión ya desplegada, o con una plantilla de AWS CloudFormation guardada, y muestra una lista de cambios.
        -  Este paso se definió con el nombre **test**
        - La configuración de **build_specs** se localiza en el archivo **test_cicd.yml** dentro de la carpeta **build_specs**

    - **Paso3** - Desplegar la infraestructura: cdk deploy. 
        - Paso para desplegar nuestro pipeline en la cuenta aws seleccionada.
        - El comando cdk deploy despliega los stacks especificados en tu cuenta de AWS. Si hay un error se ejecuta un rollback automático (característica heredada de AWS CloudFormation).
        - La configuración de **build_specs** se localiza en el archivo **deploy_cicd.yml** dentro de la carpeta **build_specs**

### 5. Hacer el commit y push de los cambios al repositorio remoto de Github:
```console
git remote add origin https://github.com/user-name/aws-codepipeline-project.git
git add .
git commit -m "commit inicial"
git branch -M main
git push -u origin main
```

## Step 3: Deployment del Pipeline con un Stages básicos
### 1. Bootstrap del proyecto CDK en respositorio local
- Si está ejecutando un proyecto CDK por primera vez en la máquina local, otro paso importante antes de desplegar el proyecto CDK en codepipeline, es hacer bootstrap. Ejecute los siguientes comandos, en el directorio raíz para arrancar el entorno del proyecto.
```console
cdk bootstrap
```
- Si el proceso de bootstrap tienen exito se debe de observar en el stack de CloudFormation en la cuenta de AWS.
### 2. Deploy del proyecto CDK
- Crea la versión inicial de CodePipeline y la infraestructura requerida para hacer el deploy del proyecto.
```console
cdk deploy
```

