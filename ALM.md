# ALM with Microsoft Power Platform
Application lifecycle management

<img width="474" height="468" alt="image" src="https://github.com/user-attachments/assets/fdcf17d2-c237-45bf-ab01-e9084352508e" />

## Tabla de contenido
  - [Entornos](#entornos)
  - [Soluciones](#soluciones)
    - [Control de versiones](#control-de-versiones)
    - [Control de versión con una solución](#control-de-versión-con-una-solución)
  - [Pipeline en DevOps con Build Tools](#pipeline-en-devops-con-build-tools)
    - [Autorizacion de aplicación en Azure Entra ID para acceder al entorno](#autorizacion-de-aplicación-en-azure-entra-id-para-acceder-al-entorno)
      - [Azure Entra Id](#azure-entra-id)
      - [Añadir aplicación a los entornos de Power Platform](#añadir-aplicación-a-los-entornos-de-power-platform)
    - [Conexión de servicio hacia el entorno en el repositorio de DevOps](#conexión-de-servicio-hacia-el-entorno-en-el-repositorio-de-devops)
    - [Permisos sobre el repositorio](#permisos-sobre-el-repositorio)
    - [Personal Access Token](#personal-access-token)
    - [Agent pool](#agent-pool)
  - [Exportar una solución desde un entorno de desarrollo](#exportar-una-solución-desde-un-entorno-de-desarrollo)
  - [Creación de una release y despliegue en entorno](#creación-de-una-release-y-despliegue-en-entorno)
  - [Integración Git Power Platform](#Integración-Git-Power-Platform)
  - [Despliegues con pipelines Power Platform](#Despliegues-con-pipelines-Power-Platform)
    - [Requisitos](#requisitos)
    - [Creación de un flujo Desarrollo - Test](#creación-de-un-flujo-desarrollo---test)
    - [Creación de un flujo Desarrollo - Test - Producción](#creación-de-un-flujo-desarrollo---test---producción)
      - [Primer fase, Desarrollo ➡️ Test](#primer-fase-desarrollo--test)
      - [Segunda fase, Test ➡️ Producción](#segunda-fase-test--producción)
    - [Administrar pipelines](#administrar-pipelines)
      - [Histórico](#histórico)
      - [Artefactos](#artefactos)
    - [Despliegue de pipelines Service Principal o Pipeline Stage Owner](#Despliegue-de-pipelines-Service-Principal-o-Pipeline-Stage-Owner)

## Entornos 

Para pode aplicar los principios de ALM, se necesitan como mínimo dos entornos, **Desarrollo** y **Producción**, pero lo ideal sería contar como mínimo con los entornos de **Desarrollo**, **Test** y **Producción**, de esta forma, se podrían hacer pruebas de tanto de funcionalidad como de despliegue. Otros entornos que se podrían añadir serían los entornos de **Test para cliente** e **Integración**.

[Ir a tabla de contenido](#Tabla-de-contenido)

## Soluciones 

Cualquier desarrollo que se quiera desplegar en un entorno de producción debería estar incluido dentro de una solución. Esta es la base que nos permitirá mover los desarrollos entre los distintos entornos como una unidad de cambio. 

<img width="800" height="274" alt="image" src="https://github.com/user-attachments/assets/bcee19d4-77fe-4731-b508-4422a4f05d10" />

Se deberán usar las soluciones administradas en los entornos de **Test** y **Producción**, de esta forma cualquier cambio necesario, debe realizarse en el entorno de Desarrollo y lo que se valide en el entorno de Test se puede pasar a Producción, con la confianza de ser lo probado en Test. 

> 💡 **Tip**
> 
> El proceso de creación de la solución administrada para pasar de un entorno a otro, debería ser un proceso automático que genere el artefacto para la instalación.

[Ir a tabla de contenido](#Tabla-de-contenido)

### Control de versiones 

Se debería utilizar un sistema de control de versiones por las siguientes razones: 

- Fuente de verdad de los desarrollos 

- Trazabilidad de los cambios 

- Organización en ramas (bugs, pequeñas modificaciones, grandes desarrollos, ...) 

- Uso de procesos de aprobación de cambios 

[Ir a tabla de contenido](#Tabla-de-contenido)

## Control de versión con una solución 

- Integración con [Power Platform Integration](https://learn.microsoft.com/en-us/power-platform/alm/git-integration/overview) 

- Pipeline en DevOps 

  - Tareas [Build tools ](https://learn.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks#build-and-release-pipelines)
    
    [Ver caso Pipeline en DevOps con Build Tools](#Pipeline-en-DevOps-con-Build-Tools)

  - [YAML pipeline ](https://learn.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks#solution-tasks)

<img width="1053" height="436" alt="image" src="https://github.com/user-attachments/assets/7602a1c5-8ade-4e44-8fbb-30827119193a" />

[Ir a tabla de contenido](#Tabla-de-contenido)

## Pipeline en DevOps con Build Tools

Necesitamos disponer de los siguientes elementos:

- [Autorizacion de aplicación en Azure Entra ID para acceder al entorno](#Autorizacion-de-aplicación-en-Azure-Entra-ID-para-acceder-al-entorno)
- Cuenta en DevOps
- [Conexión de servicio hacia el entorno en el repositorio de DevOps](#Conexión-de-servicio-hacia-el-entorno-en-el-repositorio-de-DevOps)
- [PAT (Personal Access Token) con los permisos necesarios](#Personal-Access-Token)
- [Permisos sobre el repositorio](#Permisos-sobre-el-repositorio)
- [Agent pool](#Agent-tool)

[Ir a tabla de contenido](#Tabla-de-contenido)

### Autorizacion de aplicación en Azure Entra ID para acceder al entorno

Debemos disponer en Azure Entra ID de una autorización de aplicación hacia nuestro entornno de Power Platform.

#### Azure Entra Id

Crearemos un registro de applicación normal

<img width="1442" height="597" alt="image" src="https://github.com/user-attachments/assets/b5918301-a14d-4779-aaae-9829a5def1f4" />

Asignamos los siguientes permisos

<img width="1027" height="392" alt="image" src="https://github.com/user-attachments/assets/61194015-1ae2-4333-8722-d2c60b895250" />

Creamos un secreto y copiamos su valor antes de salir

<img width="1024" height="209" alt="image" src="https://github.com/user-attachments/assets/a323746f-8b26-4bfd-ac65-e8a1f58a8a05" />

[Ir a tabla de contenido](#Tabla-de-contenido)

#### Añadir aplicación a los entornos de Power Platform

Debemos agregar la aplicación registrada a cada uno de los entornos a los que queramos acceder desde DevOps.

<img width="489" height="295" alt="image" src="https://github.com/user-attachments/assets/1eb066dc-1eb8-4312-8d5b-3b34f2000240" />

Asignar permiso de Administrador de sistema

<img width="489" height="903" alt="image" src="https://github.com/user-attachments/assets/d2962a6b-071b-43f4-9c78-0b51287b279e" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Conexión de servicio hacia el entorno en el repositorio de DevOps

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

[Ir a tabla de contenido](#Tabla-de-contenido)

### Permisos sobre el repositorio

Conceder permiso en el repositorio destino a **Project Collection Build Service Accounts** ➡️ ***Contribute** ➡️ **Allow**

<img width="1904" height="910" alt="image" src="https://github.com/user-attachments/assets/8d0bbcaa-725c-4868-805b-da2180502662" />

Conceder permiso en el repositorio destino a **... Build Service (CRM-PowerPlatform)** ➡️ ***Contribute** ➡️ **Allow**

<img width="1861" height="868" alt="image" src="https://github.com/user-attachments/assets/7d9d2f10-d553-470c-aa2c-3a33dddfba8c" />

> 💡**Nota**
>
> En caso de no estar disponible **Power Platform** dentro de las opciones seleccionables para la creación del servicio, deberemos instalarlo desde [**Visual Studio Marketplace**](https://marketplace.visualstudio.com/search?term=power%20platform&target=AzureDevOps&category=All%20categories&sortBy=Relevance)

[Ir a tabla de contenido](#Tabla-de-contenido)

### Personal Access Token

<img width="300" height="371" alt="image" src="https://github.com/user-attachments/assets/97a411fe-701f-4fa6-a147-9a69d0eb9488" />

Crearemos un PAT con los siguientes permisos

|Permiso|Descripción|Grado|
|-|-|-|
|Agent Pools|Manage agent pools and agents|Read & manage|
|Build|Artifacts, definitions, requests, queue a build, and update build properties|Read & execute|
|Code|Source code, repositories, pull requests, and notifications|Read & write|

[Ir a tabla de contenido](#Tabla-de-contenido)

### Agent pool

Para correr nuestros pipelines necesitaremos un Agent Pool.

> 💡**Nota**
>
> Consultar más sobre los [DevOps Pools](https://learn.microsoft.com/en-us/azure/devops/managed-devops-pools/?view=azure-devops)

En este caso he optado por usar un [*Self-hosted* agent pool alojado en Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops)

[Ir a tabla de contenido](#Tabla-de-contenido)

## Exportar una solución desde un entorno de desarrollo

Con este pipeline, obtendremos todos los objetos de una solución en el entorno de desarrollo y los subiremos al repositorio mediante un commit.

El pipeline lo podríamos crear usando un fichero *YAML* o configuración clásica. En este caso uso la configuración clásica.

<img width="849" height="555" alt="image" src="https://github.com/user-attachments/assets/d395e6da-44b5-4100-b532-a2c7a83cfe26" />

Seleccionamos el repositorio

<img width="646" height="433" alt="image" src="https://github.com/user-attachments/assets/05630247-8037-4ee1-a292-d2c8dd077228" />

<img width="1343" height="207" alt="image" src="https://github.com/user-attachments/assets/76ee0fc8-f5ce-44af-925e-3600b89fa4c2" />

Nombramos nuestro pipeline y le asignamos nuestra Agent pool

<img width="929" height="140" alt="image" src="https://github.com/user-attachments/assets/8b836056-acf8-4870-bc00-a293a6093eee" />

Agregaremos los siguientes pasos

<img width="582" height="654" alt="image" src="https://github.com/user-attachments/assets/4d07dc15-c885-4e4f-a4b7-45519ddcc8db" />

Y necesitaremos las siguientes variables

<img width="1225" height="317" alt="image" src="https://github.com/user-attachments/assets/b2e37fa5-62c7-4ffb-8bc1-c82fdcea62e4" />

1. Power Platform Tool Installer
2. Power Platform Export Solution Unmanaged

  Obtenemos la solución usando la conexión *Developmet Service Connection* (desarrollo) y la comprimimos en la ruta de los artefactos como *[solucionName]_Unmanaged.zip*
  
  <img width="557" height="698" alt="image" src="https://github.com/user-attachments/assets/368ed5ff-0ea2-4c09-b965-331b9bb8c99a" />

3. Power Platform Export Solution Managed

   Lo mismo que el paso anterior, pero esta vez con la solución administrada.

    <img width="931" height="694" alt="image" src="https://github.com/user-attachments/assets/1a6d2cb0-5260-4d98-9ed4-531d084a138c" />
   
5. Power Platform Set Solution Version

   Con este paso, actualizamos la versión en el entorno de desarrollo a la pasada como parámetro.

   <img width="559" height="444" alt="image" src="https://github.com/user-attachments/assets/69b8ddba-f960-4465-80c8-f890571b31e6" />

6. Power Platform Unpack Solution

   Descomprimimos el paquete de la solución en nuestro repositorio, *../[solutionName]/Unmanaged*

   <img width="428" height="650" alt="image" src="https://github.com/user-attachments/assets/c0656383-38e6-4321-92b3-e0d81fd73dec" />

7. Power Platform Unpack Solution Managed

   Realizamos la misma acción que en el caso anterior, pero esta vez para la solución administrada.

   <img width="402" height="575" alt="image" src="https://github.com/user-attachments/assets/292663d4-76c2-450f-aa54-16a09bf75490" />

9. Publish Artifact: Unmanaged

   Publicación del directorio *Unmanaged* como artefacto del pipeline.

   > ⚙️*Artefactos*
   > <img width="1440" height="494" alt="image" src="https://github.com/user-attachments/assets/e554074a-fa62-4034-a1f7-d6bedecc6ab9" />
   > <img width="1138" height="455" alt="image" src="https://github.com/user-attachments/assets/56e39013-dba9-4dfe-8232-73ea97fff003" />

   <img width="370" height="443" alt="image" src="https://github.com/user-attachments/assets/8df06d82-8d16-4bf6-adc3-7bf662579707" />

11. Publish Artifact: Managed

    Realizamos la misma configuración para el artefacto de la solución administrada

    <img width="436" height="441" alt="image" src="https://github.com/user-attachments/assets/7415ab68-ef9e-4665-8707-1bf393f6ca17" />

12. Command Line Script

    Con la ejecución del siguiente script, realizamos un commit a nuestro repositorio agregando todos los cambios de la solución.

    <img width="600" height="369" alt="image" src="https://github.com/user-attachments/assets/1640b91a-7a9f-4f4a-97aa-c9b8a89b2fae" />

    ``` pws
    git config user.email "jmartinezfe@ibermatica01.onmicrosoft.com"
    git config user.name "Jairo Martínez Fernández"
    
    BRANCH="$(Build.SourceBranchName)"
    
    git checkout -B "$BRANCH"
    
    git add --all
    git commit -m "Export and unpack $(SolutionName) unmanaged solution $(SolutionVersion)"
    
    git -c http.extraheader="AUTHORIZATION: bearer $SYSTEM_ACCESSTOKEN" push origin "$BRANCH"
    ```
13. Ejecución del pipeline

    <img width="1620" height="229" alt="image" src="https://github.com/user-attachments/assets/5071c258-c30d-4c25-b6f7-c684a7c97244" />

    <img width="1643" height="490" alt="image" src="https://github.com/user-attachments/assets/a9eebbad-e021-46a6-a8b9-69b49c16bd6b" />

    Repositorio

    <img width="700" height="582" alt="image" src="https://github.com/user-attachments/assets/8fcf6f5f-6b60-48ba-bfe1-431ebc37c537" />

    Commints

    <img width="783" height="519" alt="image" src="https://github.com/user-attachments/assets/26c36cda-e4a4-488f-b2e4-73d3e8db1c37" />

    Cambios

    <img width="921" height="744" alt="image" src="https://github.com/user-attachments/assets/5780dc26-436e-4cd8-abd8-fc7f82b70092" />

> 🗒️*Ejemplo del código de la solución en el repositorio*
>
> Formato XML
>
> <img width="949" height="678" alt="image" src="https://github.com/user-attachments/assets/4fc12424-2eec-4383-85b1-7c371ff9e3bf" />

[Ir a tabla de contenido](#Tabla-de-contenido)

## Creación de una release y despliegue en entorno

  Mediante este proceso, generaremos una nuevo release y la desplegaremos en otro entorno como solución administrada.

  > 💡**Nota**
  > Se debería disponer de un trabajo distinto por entorno destino.
  > <img width="884" height="231" alt="image" src="https://github.com/user-attachments/assets/d21cfc5d-b0ee-4fce-b07a-aee37508e50f" />

1. Creamos un nuevo pipeline para generar el artefacto
   <img width="1331" height="633" alt="image" src="https://github.com/user-attachments/assets/9840f673-96cc-4a43-9abd-9b00ecddd029" />

   Configuramos el repositorio y la rama

   <img width="1323" height="712" alt="image" src="https://github.com/user-attachments/assets/2665f819-2a3d-442f-a75c-21ddddfb51fc" />

2. Crear el trabajo
   
   Crearemos las siguientes tareas

   <img width="575" height="397" alt="image" src="https://github.com/user-attachments/assets/177caf23-9c35-408a-ae37-3660a914866a" />

   a. Power Platform Tool Installer
   b. Power Platform Pack Solution

     Empaquetamos la solución como solución administrada.

     <img width="454" height="580" alt="image" src="https://github.com/user-attachments/assets/b7129012-4452-49ca-8182-41b6e9501630" />

   c. Power Platform Import Solution

     Importamos la solución al entorno destino, para ello seleccionamos la conexión de servicio adecuada.

     <img width="566" height="562" alt="image" src="https://github.com/user-attachments/assets/e228f1c0-fe59-4614-827b-008c3a910979" />

   d. Power Platform Publish Customizations

     Publicar la solución administrada en el entorno seleccionado

     <img width="567" height="471" alt="image" src="https://github.com/user-attachments/assets/e7bcd32e-4e0b-48e8-be18-9ae2eb567b51" />

[Ir a tabla de contenido](#Tabla-de-contenido)

## Integración Git Power Platform

La integración de Git con Power Platform nos permitirá mantener respaldadas nuestras soluciones o entornos respaldados en un sistema de control de versiones. Se requiere disponer de Azure DevOps y Power Platform en la misma organización. Estas son las cosas que podremos hacer:

  - Commit de los cambios realizados en la solución (Power Platform ➡️ DevOps)
  - Pull de otros cambios realizados sobre el repositorio a la solución (Power Platform ⬅️ DevOps)
  - Resolución de conflictos en caso de haberse hecho cambios tanto en la solución como en el repositorio

> 🗒️*Ejemplo de código de solución en repositorio*
>
> Formato YML
>
> <img width="957" height="684" alt="image" src="https://github.com/user-attachments/assets/aa55486e-9210-4470-843a-366792b93a84" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Configuración

La configuración de Git en Power Platform es sencilla. Lo único que debemos decidir es si integraremos las soluciones de forma individual o todo el entorno.

<img width="776" height="686" alt="image" src="https://github.com/user-attachments/assets/117e3dfe-ca53-404e-aa45-fb7c3b5e6633" />

- **Solución**
  Con este modo de integración, tendremos la libertad de elegir qué soluciones y dónde las integraremos en nuestro repositorio de forma individual.

  En estas imágenes podemos ver cómo las soluciones sincronizadas han generado una estructura diferenciada en el repositorio.

  |Power Platform|DevOps|
  |-|-|
  |<img width="1504" height="270" alt="image" src="https://github.com/user-attachments/assets/938969d8-7d84-44e2-ae74-c3bb2b16d293" />|<img width="244" height="479" alt="image" src="https://github.com/user-attachments/assets/4eed01c4-e1a1-4227-b813-ba6df67d10bb" />|
  |<img width="397" height="481" alt="image" src="https://github.com/user-attachments/assets/0d242eba-a8a7-478a-95ac-5d1020caa724" />|TestSolution2|
  |<img width="401" height="488" alt="image" src="https://github.com/user-attachments/assets/146e17df-2e23-4f8a-969d-79204b7ef5c7" />|TestSolution3|
  

- **Entorno**
  Todas las soluciones se integrarán en el mismo punto dentro del repositorio. Esto no implica que se vayan a sincronizar todas de forma automática, dado que las deberemos conectar de forma individual.

  En estas imágenes se puede observar cómo las soluciones *TestSolution2* y *TestSolution3*, generan una estructura distinta a la anterior.
  
  |Power Platform|DevOps|
  |-|-|
  |<img width="1638" height="287" alt="image" src="https://github.com/user-attachments/assets/90a133cd-b398-4198-8cb3-437cf8f3177c" />|<img width="292" height="404" alt="image" src="https://github.com/user-attachments/assets/b1147a5f-698f-4d88-989c-680d7735e756" />|

[Ir a tabla de contenido](#Tabla-de-contenido)

## Despliegues con pipelines Power Platform

En Power Platform disponemos de la posibilidad de realizar los despliegues de soluciones entre los distintos entornos mediante *pipelines*.

Podemos crear pipelines para despliegues sencillos, p.e. Desarrollo ➡️ Test o más complejos que incluyan más entornos Desarrollo ➡️ Test ➡️ Producción

<img width="1262" height="591" alt="image" src="https://github.com/user-attachments/assets/af4d9f13-6046-4711-ae08-f52cd5652ce0" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Requisitos

Se deben tener los permisos suficientes para instalar la aplicación pipelines, administrador Power Platform o administrador system Dataverse

Los entornos destino deben:
  - ser administrados (Managed Environment)
  - tener base Microsoft Dataverse

Configurar el host para los pipelines:
  - Platform host, por defecto, gestionado por Microsoft. Reglas de governanza estabdarizadas por Microsoft.
  - Custom host, configurable. Requiere un entorno de producción nuevo. 

[Ir a tabla de contenido](#Tabla-de-contenido)

### Creación de un flujo Desarrollo - Test

1. Comprobar que el entorno destino es administrado y en caso contrario configurarlo para que lo sea.
   
   <img width="504" height="207" alt="image" src="https://github.com/user-attachments/assets/79261667-58bd-4e27-bc43-da8b3c5f4f8d" />
   
2. En el entorno de origen, ir a la solución que queremos desplegar y seleccionar Pipeline <img width="40" height="30" alt="image" src="https://github.com/user-attachments/assets/b048192f-37e6-4ba3-9fe0-0326359e6c8b" />

3. Creamos un nuevo pipeline

   <img width="967" height="820" alt="image" src="https://github.com/user-attachments/assets/48775f02-23fc-40de-b40f-1ea41e4416a5" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Creación de un flujo Desarrollo - Test - Producción

1. Comprobar que el entorno destino es administrado y en caso contrario configurarlo para que lo sea.
   
   <img width="504" height="207" alt="image" src="https://github.com/user-attachments/assets/79261667-58bd-4e27-bc43-da8b3c5f4f8d" />

2. En el entorno de origen, ir a la solución que queremos desplegar y seleccionar Pipeline <img width="40" height="30" alt="image" src="https://github.com/user-attachments/assets/b048192f-37e6-4ba3-9fe0-0326359e6c8b" />

3. Creamos un nuevo pipeline
  3.1. Primer fase, Desarrollo ➡️ Test
   
    <img width="335" height="540" alt="image" src="https://github.com/user-attachments/assets/a3bb5064-10ba-466d-9ac4-1e95c9236445" />

  3.2. Segunda fase, Test ➡️ Producción (<img width="97" height="23" alt="image" src="https://github.com/user-attachments/assets/cb733768-a608-45a4-b71d-cb81b4df64b2" />)

    <img width="331" height="479" alt="image" src="https://github.com/user-attachments/assets/a171f141-4d31-48c7-987b-ae892073183d" />

  <img width="1227" height="578" alt="image" src="https://github.com/user-attachments/assets/5ea13386-cca6-4560-85c4-8e0abc707f99" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Administrar pipelines

A través del administrador de pipelines (Manage pipelines), podemos ver toda la información de los pipelines creados, los entornos, el histórico de ejecuciones, los artefactos...

Histórico

<img width="1697" height="493" alt="image" src="https://github.com/user-attachments/assets/00bc3bf6-80d6-4af5-a69b-2dd1bd06d37f" />

<img width="1518" height="332" alt="image" src="https://github.com/user-attachments/assets/340f151f-2f83-47fb-bb16-89af3404caea" />

Artefactos

<img width="1348" height="507" alt="image" src="https://github.com/user-attachments/assets/72be00ba-04c4-483a-bc7b-51906f47d26d" />

[Ir a tabla de contenido](#Tabla-de-contenido)

### Despliegue de pipelines Service Principal o Pipeline Stage Owner

Ampliar información sobre flujos de aprovación y despliegue delegado.

[Extend pipelines in Power Platform](https://learn.microsoft.com/en-us/power-platform/alm/extend-pipelines)

[Ir a tabla de contenido](#Tabla-de-contenido)



> 📄*Documentación*
>
> [Create a pipeline using a custom pipelines host](https://learn.microsoft.com/en-gb/power-platform/alm/custom-host-pipelines)
> [Overview of pipelines in Power Platform](https://learn.microsoft.com/en-us/power-platform/alm/pipelines)

[Ir a tabla de contenido](#Tabla-de-contenido)
