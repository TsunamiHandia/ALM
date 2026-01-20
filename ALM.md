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

Necesitamos disponer de los siguientes elementos:

- [Autorizacion de aplicación en Azure Entra ID para acceder al entorno](#Autorizacion-de-aplicación-en-Azure-Entra-ID-para-acceder-al-entorno)
- Cuenta en DevOps
- [Conexión de servicio hacia el entorno en el repositorio de DevOps](#Conexión-de-servicio-hacia-el-entorno-en-el-repositorio-de-DevOps)
- [PAT (Personal Access Token) con los permisos necesarios](#Personal-Access-Token)
- [Permisos sobre el repositorio](#Permisos-sobre-el-repositorio)
- [Agent pool](#Agent-tool)

### Autorizacion de aplicación en Azure Entra ID para acceder al entorno

Debemos disponer en Azure Entra ID de una autorización de aplicación hacia nuestro entornno de Power Platform.

#### Azure Entra Id

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

### Permisos sobre el repositorio

Conceder permiso en el repositorio destino a **Project Collection Build Service Accounts** ➡️ ***Contribute** ➡️ **Allow**

<img width="1904" height="910" alt="image" src="https://github.com/user-attachments/assets/8d0bbcaa-725c-4868-805b-da2180502662" />

Conceder permiso en el repositorio destino a **... Build Service (CRM-PowerPlatform)** ➡️ ***Contribute** ➡️ **Allow**

<img width="1861" height="868" alt="image" src="https://github.com/user-attachments/assets/7d9d2f10-d553-470c-aa2c-3a33dddfba8c" />

> 💡**Nota**
>
> En caso de no estar disponible **Power Platform** dentro de las opciones seleccionables para la creación del servicio, deberemos instalarlo desde [**Visual Studio Marketplace**](https://marketplace.visualstudio.com/search?term=power%20platform&target=AzureDevOps&category=All%20categories&sortBy=Relevance)

### Personal Access Token

<img width="300" height="371" alt="image" src="https://github.com/user-attachments/assets/97a411fe-701f-4fa6-a147-9a69d0eb9488" />

Crearemos un PAT con los siguientes permisos

|Permiso|Descripción|Grado|
|-|-|-|
|Agent Pools|Manage agent pools and agents|Read & manage|
|Build|Artifacts, definitions, requests, queue a build, and update build properties|Read & execute|
|Code|Source code, repositories, pull requests, and notifications|Read & write|

### Agent pool

Para correr nuestros pipelines necesitaremos un Agent Pool.

> 💡**Nota**
>
> Consultar más sobre los [DevOps Pools](https://learn.microsoft.com/en-us/azure/devops/managed-devops-pools/?view=azure-devops)

En este caso he optado por usar un [*Self-hosted* agent pool alojado en Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops)


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


    










