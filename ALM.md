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
- [Autorizacion de aplicación en Azure Entra ID para acceder al entorno](##Autorizacion-de-aplicación-en-Azure-Entra-ID-para-acceder-al-entorno)
- Cuenta en DevOps
- [Conexión de servicio hacia el entorno en el repositorio de DevOps](##Conexión-de-servicio-hacia-el-entorno-en-el-repositorio-de-DevOps)
- [PAT (Personal Access Token) con los permisos necesarios](##Personal-Access-Token)

#### Autorizacion de aplicación en Azure Entra ID para acceder al entorno

Debemos disponer en Azure Entra ID de una autorización de aplicación hacia nuestro entornno de Power Platform.

##### Azure Entra Id

Crearemos un registro de applicación normal

<img width="1442" height="597" alt="image" src="https://github.com/user-attachments/assets/b5918301-a14d-4779-aaae-9829a5def1f4" />

Asignamos los siguientes permisos

<img width="1027" height="392" alt="image" src="https://github.com/user-attachments/assets/61194015-1ae2-4333-8722-d2c60b895250" />

Creamos un secreto y copiamos su valor antes de salir

<img width="1024" height="209" alt="image" src="https://github.com/user-attachments/assets/a323746f-8b26-4bfd-ac65-e8a1f58a8a05" />

#### Añadir aplicación a los entornos de Power Platform

Debemos agregar la aplicación registrada a cada uno de los entornos a los que queramos acceder desde DevOps.

<img width="489" height="295" alt="image" src="https://github.com/user-attachments/assets/1eb066dc-1eb8-4312-8d5b-3b34f2000240" />

Asignar permiso de Administrador de sistema

<img width="489" height="903" alt="image" src="https://github.com/user-attachments/assets/d2962a6b-071b-43f4-9c78-0b51287b279e" />

#### Conexión de servicio hacia el entorno en el repositorio de DevOps

Crearemos tantos Servicios de conexión como entornos distintos nos queramos conectar.

<img width="591" height="916" alt="image" src="https://github.com/user-attachments/assets/3bfdd86f-fcda-4729-8ab5-41493006d2a7" />

Ejemplo de creación del Servicio de conexión **Development Service Connection**

<img width="1234" height="904" alt="image" src="https://github.com/user-attachments/assets/23e5efee-f459-42aa-89d4-33e6baf514a6" />

|Configuración|Valor|
|-|-|
|Server URL|Url del entorno al que nos queremos conectar <img width="691" height="520" alt="image" src="https://github.com/user-attachments/assets/69caad26-6ce2-4c9c-836a-c11a42e2da95" />|
|Tenant Id|Id del tenant <img width="1426" height="342" alt="image" src="https://github.com/user-attachments/assets/bc6a91b1-7943-4dbb-9ca6-63dcd7adeccf" />|
|Application Id| Id de la applicación registrada en Azure Entra ID <img width="1426" height="342" alt="image" src="https://github.com/user-attachments/assets/a3ad2ca8-48eb-4de8-b01b-355cc01ec2e5" />|
|Client secret o Application Id|Valor del secreto de la aplicación registrada en Azure Entra ID <img width="1078" height="126" alt="image" src="https://github.com/user-attachments/assets/80f1fa1a-8ff9-42e4-b8d6-f58b3c6e08c6" />|
|Service Connection Name|Nombre del servicio de conexión|
|Security| Marcar la opción *Grant access permission to all pipelines*|

#### Personal Access Token
