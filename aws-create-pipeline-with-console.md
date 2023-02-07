
# Introducción
El objetivo es la creación de un CI/CD pipeline para los proyectos de **AWS CDK**. Esta guía es para crear el pipeline desde la consola con el objetivo de entender de manera general como trabajan los servicios de AWS Developer tools y verificar los detalles para después crear el proceso de manera automatizada e integralo con el ecosistema de AWS.

## Conceptos básicos involucrados
- **CI/CD** hace referencia a Continuous Integration y Continuous Delivery e introduce automatización y monitoreo del proceso completo de SDLC(Software Development Lifecycle).

- **Continuous Integration (CI)** es una práctica de desarrollo de software en la que los desarrolladores combinan regularmente sus cambios de código en un repositorio central, después de lo cual se ejecutan builds y pruebas automatizadas.
    - Encontrar y corregir errores más rápidamente
    - Mejorar la calidad del software
    - Reducir el tiempo que lleva validar y lanzar nuevas actualizaciones de software
    - La integración continua se enfoca en la integración de commits y cambios de código pequeños

- **Continuous delivery (CD)** es una práctica de desarrollo de software en la que los cambios de código se crean, prueban y preparan automáticamente para su lanzamiento en producción.

    - Automatizar el proceso de release de software
    - Mejorar la productividad del desarrollador
    - Mejorar la calidad del código
    - Entregar actualizaciones más rápido
    - El objetivo de la entrega continua no es aplicar todos los cambios a la producción inmediatamente, sino asegurarse de que todos los cambios estén listos para pasar a producción.

## AWS CodePipeline para AWS CDK
El objetivo es crear la solución con la consola de AWS porque ayuda a comprender cómo funcionan los servicios involucrados.