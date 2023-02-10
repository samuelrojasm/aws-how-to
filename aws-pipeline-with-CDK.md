# Introducción
Automatizar el despliegue de un pipeline de un proyecto de AWS CDK con AWS Codepipeline integrado en un repositorio de GitHub.

## Step 1: Pre-requisitos
1. Instalar AWS-CLI
1. Configurar credeenciales de manera local
1. Instalar NodeJS
1. Instalar CDK (global)
1. Crear un repositorio vacio (por ejemmplo: aws-codepipeline-project) y clonarlo

## Step 2: Setup del proyecto CDK
### 1. Setup inicial de Code Base el proyecto CDK
- Movernos a la carpeta en donde se clonó el repositorio vacio, por ejemmplo: aws-codepipeline-project
- Abrir la carpeta en VSCode
- Inicializar la aplicación CDK en la carpeta creada
```bash
cdk init app --language typescript
``` 

- The `cdk.json` file tells the CDK Toolkit how to execute your app.

- Useful commands:

* `npm run build`   compile typescript to js
* `npm run watch`   watch for changes and compile
* `npm run test`    perform the jest unit tests
* `cdk deploy`      deploy this stack to your default AWS account/region
* `cdk diff`        compare deployed stack with current state
* `cdk synth`       emits the synthesized CloudFormation template

### 2. Ir al directorio **bin** y abrir el archivo: **your-root-directory-name.ts** y hacer las siguientes modificaciones:
```js
env: { 
    account: '123456789012', // replace with your aws account ID
    region: 'us-east-1'      // replace with your preferred region
 },       
```

### 3. Ir al directorio **lib** y abrir el archio **your-root-directory-name.ts** y hacer las siguientes modificaciones:
Agregar el siguiente código:
```js
import { CodePipeline, CodePipelineSource, ShellStep } from 'aws-cdk-lib/pipelines';
import { ManualApprovalStep } from 'aws-cdk-lib/pipelines';
```

```js
// The code that defines your stack goes here
new CodePipeline(this, 'Pipeline', {
      pipelineName: 'CDKTestPipeline',       // Creating a new code pipeline which is a construct
      synth: new ShellStep('Synth', {        // Add a new synthesis 'shellstep' which will be pointed at our gihub repository 
        input: CodePipelineSource.gitHub('user-name/aws-codepipeline-project', 'main'), // replace the GitHub repository name with 'user-name/repository-name'
        
        // The build steps for the pipeline are defined by these commands
        commands: ['npm ci',
                   'npm run build',
                   'npx cdk synth']
      }),
})
```

- En la clase **AwsCodepipelineProjectStack** se crea un nuevo constructor de **CodePipeline** con el nombre **CDKTestPipeline**
- Se crea una nueva **synthesis** "ShellStep" que apunta al repositorio de Github en donde se localiza el código base de CDK.
- La ejecución del pipeline ocurrirá ante cualquier cambio en main branch de nuestro repositorio.
- Al final se agregan los comandos  paara el paso build en el pipeline:
    * `npm ci` - (npm clean install), which is similar to npm install that is to be used in automated environments.
    * `npm run build` - to allow us to perform any necessary building/prep tasks for the project.
    * `npx cdk synth` - to synthesize whatever we have in the cloud formation stack to generate the self mutating pipeline.

### 4. Hacer el commit y push de los cambios al repositorio remoto de Github:
```console
git remote add origin https://github.com/user-name/aws-codepipeline-project.git
git add .
git commit -m "commit inicial"
git branch -M main
git push -u origin main
```

## Step 3: Crear accesos y configuraciones en AWS

## Step 4: Deployment del Pipeline con un Stages básicos
### 1. Bootstrap del proyecto CDK en respositorio local
- Si está ejecutando un proyecto CDK por primera vez en la máquina local, otro paso importante antes de desplegar el proyecto CDK en codepipeline, es hacer bootstrap. Ejecute los siguientes comandos, en el directorio raíz para arrancar el entorno del proyecto.
```console
cdk bootstrap
```


