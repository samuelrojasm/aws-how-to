# Introducción


## Configurar QuickSight
* En el caso que sea la primera vez que se usa QuickSight en una cuneta específica es necesario como primer paso crear la cuenta.
1. Buscar el servicio QuickSight
1. En la pagina de QuickSight, dar click en el boton `Sign up for QuickSight`.
    <p align="center">
        <img src="img/quicksight-quickstart01.png" width="450"/>
    </p>

1. Seleccionar la edición Enterprise que es la de default
    <p align="center">
        <img src="img/quicksight-quickstart02.png" width="450"/>
    </p>

1. Ingresar los datos de los campos:
    - QuickSight account name
    - Notification email address
    <p align="center">
        <img src="img/quicksight-quickstart03.png" width="450"/>
    </p>

1. Presionar el botón `Finish`
    <p align="center">
        <img src="img/quicksight-quickstart04.png" width="450"/>
    </p>

1. Click en `Go to Amazon QuickSight`
    <p align="center">
        <img src="img/quicksight-quickstart05.png" width="450"/>
    </p>

1. Inicia la consola de QuickSight
    <p align="center">
        <img src="img/quicksight-quickstart06.png" width="450"/>
    </p>

## Construye tu primer dashboard
### Crear el dataset
1. Seleccionar la opción `Datasets`
1. Seleccionar el botón `New dataset`
1. Seleccionar la primera opción `Upload a file`
1. Crragar el archivo `SaaS-Sales.csv` que se localiza en los recursos
    <p align="center">
        <img src="img/quicksight-quickstart07.png" width="450"/>
    </p>
1. Seleccionar `Next`
1. Seleccionar `Visualize` Aquí es donde se pueden añadir elementos visuales, ajustar el diseño y publicar el dashboard.
    <p align="center">
        <img src="img/quicksight-quickstart08.png" width="450"/>
    </p>
1. Hacer click en el nombre predeterminado del panel superior para acceder al modo de edición.
1. Cambiar el nombre `SaaS-Sales analysis - Build your first dashboard`
1. Hacer click fuera del cuadro de nombre para salir del modo de edición de nombre.

### Visualizar ventas por mes
1. En `Fields list` seleccionar `Sales` y  `Order Date`
1. Haga clic en la flecha situada junto a la etiqueta Order Date del eje X y elija la opción `Aggregate->Month.` 
Como alternativa, puede desplegar la sección Field Wells en la parte superior de la pantalla, utilizar la flecha en el campo `Order Date` y cambiar `Aggregate->Month`
    <p align="center">
        <img src="img/quicksight-quickstart09.png" width="450"/>
    </p>

## Referencias
- [QuickSight Workshops](https://catalog.workshops.aws/quicksight/en-US)