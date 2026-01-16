# ALM with Microsoft Power Platform
Application lifecycle management

<img width="474" height="468" alt="image" src="https://github.com/user-attachments/assets/fdcf17d2-c237-45bf-ab01-e9084352508e" />

## Entornos 

Para pode aplicar los principios de ALM, se necesitan como mínimo dos entornos, **Desarrollo** y **Producción**, pero lo ideal sería contar como mínimo con los entornos de **Desarrollo**, **Test** y **Producción**, de esta forma, se podrían hacer pruebas de tanto de funcionalidad como de despliegue. Otros entornos que se podrían añadir serían los entornos de **Test para cliente** e **Integración**.

## Soluciones 

Cualquier desarrollo que se quiera desplegar en un entorno de producción debería estar incluido dentro de una solución. Esta es la base que nos permitirá mover los desarrollos entre los distintos entornos como una unidad de cambio. 

<img width="800" height="274" alt="image" src="https://github.com/user-attachments/assets/bcee19d4-77fe-4731-b508-4422a4f05d10" />

Se deberán usar las soluciones administradas en los entornos de **Test** y **Producción**, de esta forma cualquier cambio necesario, debe realizarse en el entorno de Desarrollo y lo que se valide en el entorno de Test se puede pasar a Producción, con la confianza de ser lo probado en Test. 

> 💡 **Tip**
> 
> El proceso de creación de la solución administrada para pasar de un entorno a otro, debería ser un proceso automático que genere el artefacto para la instalación.


### Control de versiones 

Se debería utilizar un sistema de control de versiones por las siguientes razones: 

- Fuente de verdad de los desarrollos 

- Trazabilidad de los cambios 

- Organización en ramas (bugs, pequeñas modificaciones, grandes desarrollos, ...) 

- Uso de procesos de aprobación de cambios 


## Control de versión con una solución 

- Integración con [Power Platform Integration](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/overview) 

- Pipeline en DevOps 

  - Tareas [Build tools ](https://learn.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks#build-and-release-pipelines)
    
    [Ver caso Pipeline en DevOps con Build Tools](#Pipeline-en-DevOps-con-Build-Tools)

  - [YAML pipeline ](https://learn.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks#solution-tasks)

<img width="1053" height="436" alt="image" src="https://github.com/user-attachments/assets/7602a1c5-8ade-4e44-8fbb-30827119193a" />

## Pipeline en DevOps con Build Tools

### Exportar una solución desde un entorno de desarrollo

Con este pipeline, obtendremos todos los objetos de una solución en el entorno de desarrollo y los subiremos al repositorio mediante un commit.

Necesitamos disponer de los siguientes elementos:
- Autorizacion de aplicación en Azure Entra ID para acceder al entorno
- Cuenta en DevOps
- Conexión de servicio hacia el entorno en el repositorio de DevOps
- PAT (Personal Access Token) con los permisos necesarios

