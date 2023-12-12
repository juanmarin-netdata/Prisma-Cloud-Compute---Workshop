# Prisma Cloud Compute

## Introducción

El modulo Compute de Prisma Cloud está diseñado para proteger cargas de trabajo y aplicaciones Cloud Native en entornos modernos; proporcionando seguridad integral en el ciclo de vida de las aplicaciones, desde la creación hasta la ejecución, mediante:

1. Seguridad en Contenedores y Kubernetes: Protege contenedores, imágenes de Docker y orquestación en Kubernetes, asegurando las configuraciones y mitigando la explotación de vulnerabilidades.
   
2. Protección en Tiempo de Ejecución: Vigila las cargas de trabajo Cloud Native en tiempo real, detectando y respondiendo a actividades sospechosas o maliciosas.

3. Integraciones de DevOps: Se integra con herramientas y flujos de trabajo de CI/CD para seguridad continua en el desarrollo y despliegue de aplicaciones.
   
4. Visibilidad y Control de vulnerabilidades: Ofrece un análisis periódico sobre las vulnerabilidades y brechas de seguridad en las aplicaciones y cargas de trabajo Cloud Native.

Prisma Cloud Compute se enfoca en asegurar el entorno de ejecución y las aplicaciones en sí, abarcando desde la fase de desarrollo hasta la producción.

## Prerequisitos:

- Conexión a servidores remotos a través del puerto 22.
- Usuario habilitado a la consola de Prisma Cloud.

## Desplegando un DeamonSet Defender en un clúster de Kubernetes

**Objetivo:** Configurar, descargar y desplegar el DaemonSet Defender de Prisma Cloud en un clúster de Kubernetes.

**Configuraciones en Prisma Cloud Compute:**

1. Inicie sesión en la consola de Prisma Cloud.

   `Prisma Cloud URL: https://apps.paloaltonetworks.com/apps`

3. Diríjase a la sección de **Compute** y busque la ruta **Manage > Defenders > Manual Deploy**.

4. Dentro del método de despliegue, seleccione **Kubernetes** como el tipo de Defender que desea desplegar. 

5. Configure las opciones de despliegue según sus necesidades específicas.

**Despliegue del DaemonSet Defender:**

1. Inicie sesión en el servidor destinado al Plano de Control.

   `ssh user@public_ip`

2. Abra la terminal del Plano de Control de Kubernetes y cargue el archivo YAML en el directorio de trabajo.

   `vim twistlock_defender.yaml`

3. Ejecute el siguiente comando para desplegar el DaemonSet Defender:

   `kubectl apply -f <nombre_del_archivo>.yaml`

4. Verifique que los pods del Defender se estén ejecutando correctamente con:

   `kubectl get pods -A`

**Verificación y Monitoreo:**

1. Una vez desplegado el Defender, puede monitorearlo desde la consola de Prisma Cloud.
   
2. Revise el estado de conexión y los logs del Defender desde la ruta **Manage > Defenders > Defenders:Deployed** en la sección de **Compute**.
 


<!-- ## Asegurando el cumplimiento en IaC con Prisma Cloud - Checkov

**Objetivo:** instalar el motor de escaneo de IaC **_Checkov_**

**Actividades:**

1. Para poder instalar Checkov previamente debe tener instalado Python >= 3.10, puede descargarlo en [este enlace](https://www.python.org/downloads/) y realizar su instalación por defecto.
2. Puede verificar la versión de Python instalado ejecutando el siguiente comando en su CLI:

```
python --version
```

3. Instalar Checkov, puede utilizar cualquiera de los dos comandos:

```
pip install checkov
pip3 install checkov
```

4. Descargue o clone el repositorio "xxxxx" en su maquina local.

5.

## Asegurando el cumplimiento en Tiempo Real desde el IDE

**Objetivo:** instalar la extensión de Checkov en Visual Studio Code.

**Actividades:**

1. Para poder instalar la extensión de Checkov, previamente debe tener instalado Visual Studio Code, puede descargarlo en [este enlace](https://code.visualstudio.com/download) y realizar su instalación por defecto.

2. Abrir Visual Studio Code e instalar la extensión de Checkov en la opción: **Extensiones**, **buscar “Checkov”** dar click en **instalar.**
   ![VSC Checkov Extension](./images/VSC_Checkov_Ext.png)
-->

## Asegurando mi proceso de despliegue de IaC con GitHub Actions

**Objetivo:** Crear un pipeline en GitHub Actions con un Job de Prisma Cloud qué escanee por incumplimientos de controles en IaC.

**Actividades:**

1. Obtener una Access Key y Secret Key de Prisma Cloud, puede encontrar el [paso a paso para crearla en este enlace](https://docs.prismacloud.io/en/classic/cspm-admin-guide/manage-prisma-cloud-administrators/create-access-keys)

_`Nota: Asegúrese de establecer una expiración para la access key, esta es la buena práctica`_

2. Ir a Github y hacerle un fork a este repositorio en su cuenta de GitHub (mismos pasos que en el laboratorio anterior).

3. En su nuevo repositorio, seleccionar las siguientes opciones **Settings >> Secrets and variables >> Actions >> New repository secret** y crear las siguientes variables en GitHub:

- `Name: BC_API_KEY `
- `Secret: AccessKey::SecretKey`

  ![Create Secrets in GitHub](./images/GitHub_Secrets.png)

_`Nota: Asegúrese de no incluir espacios en blanco en el secret y de separar los dos valores por los caracteres "::"`_

4. Configurar el Workflow de GitHub Actions para escanear los archivos de terraform del directorio `./terraform`, para ello seleccione **Actions >> Buscar Prisma Cloud >> Configure**
   ![Prisma Cloud Workflow](./images/GitHub_Prisma.png)

5. Reemplace todo el contenido del editor con el siguiente bloque de código y realice un **commit de los cambios en la rama main** Deje todo lo demás por defecto.

```
name: Prisma Cloud IaC Scan

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]
  #schedule:
  #- cron: '26 17 * * 0'

jobs:
  prisma_cloud_iac_scan:
    runs-on: ubuntu-latest
    name: Run Prisma Cloud IaC Scan to check Compliance
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Run Scan on IaC .tf files in the repository
        uses: bridgecrewio/checkov-action@master
        id: iac-scan
        env:
          PRISMA_API_URL: https://api2.prismacloud.io
        with:
          api-key: ${{ secrets.BC_API_KEY }}
          directory: ./terraform
          framework: terraform
          quiet: true
          use_enforcement_rules: true
          #open_api_key: 'xxxxxx'

```

![GitHub Prisma Cloud IaC Commit](./images/GitHub_Commit.png)

_`Nota: el código anterior también está disponible en el archivo ./workflow.yml en este repositorio`_

Toda la información para configuración de la tarea de escaneo IaC de Prisma puede encontrarla en [este enlace](https://github.com/bridgecrewio/checkov-action) y el command reference completo lo puede encontrar en [este enlace.](https://www.checkov.io/2.Basics/CLI%20Command%20Reference.html)

6. Realice un commit o push cualquiera dentro del repositorio, puede abrir el archivo `./README.md` y agregar al final del archivo una linea de texto cualquiera y realizar el commit de los cambios para disparar el Pipeline de escaneo de los archivos de terraform.

7. En cada evento push en el repositorio se va a correr la tarea de escaneo IaC de Prisma Cloud, si desea ver los resultados del escaneo, puede ir a **Actions y seleccionar el último workflow** Al final puede encontrar el enlace directo a Prisma Cloud para ver los hallazgos en Prisma Cloud, pero también los va a encontrar en el mismo output del CLI.
   ![GitHub Actions Results](./images/GitHubActions_Results.png)

Made with Love :blue_heart: by Netdata Cloud & Automate Team.
