# Prisma Cloud Compute

## Introducción 🚀

Prisma Cloud Compute ofrece una protección integral para aplicaciones y cargas de trabajo Cloud Native en entornos modernos. Desde la seguridad en contenedores hasta la protección en tiempo de ejecución, Prisma Cloud cuenta cobertura en cada paso del ciclo de vida de tu aplicación.

### Características Clave:

- **Seguridad en Contenedores y Kubernetes**: Fortaleciendo tus contenedores y orquestaciones de Kubernetes contra vulnerabilidades.
- **Protección en Tiempo de Ejecución**: Monitorización constante para detectar y responder ante cualquier actividad sospechosa.
- **Integraciones de DevOps**: Integración fluida con tus herramientas y flujos de trabajo de CI/CD.
- **Visibilidad y Control de Vulnerabilidades**: Análisis profundos para mantener tus aplicaciones seguras y sólidas.

## Prerequisitos 🛠️

- **Acceso a Servidores Remotos**: Asegura una conexión a través del puerto 22.
- **Usuario de Prisma Cloud**: Accede a todas las funcionalidades de la consola de Prisma Cloud.

## Observaciones Importantes 🔍

- **Seguridad Primero**: Priorizamos un entorno controlado y seguro.
- **Colaboración**: Promovemos el aprendizaje colaborativo.

Conexión a Kubernetes vía SSH y Uso del Archivo Kubeconfig 🚀
-------------------------------------------------------------

### Objetivo

Este apartado te guiará en el proceso de conexión remota a un clúster de Kubernetes mediante SSH y cómo acceder al clúster utilizando el archivo kubeconfig.

### Conexión vía SSH a un Nodo del Clúster

Para administrar un clúster de Kubernetes, a menudo necesitas conectarte a uno de los nodos (generalmente el plano de control) a través de SSH. Este proceso te permite ejecutar comandos y administrar recursos del clúster directamente.

#### Pasos para la Conexión SSH

1.  **Abrir la Terminal**: Dependiendo de tu sistema operativo, abre tu terminal o cliente SSH de preferencia.
    
2.  **Conectar vía SSH**: Utiliza el comando SSH para conectarte al nodo. Necesitarás la dirección IP o el nombre de host del nodo, así como las credenciales de usuario:
    
    bashCopy code
    
    `ssh [usuario]@[dirección-IP-o-nombre-host]`
    
    Reemplaza `[usuario]` con tu nombre de usuario y `[dirección-IP-o-nombre-host]` con la dirección IP o el nombre del host del nodo a conectar.
    
3.  **Autenticación**: Proporciona la contraseña o utiliza una clave SSH para autenticarte, según la configuración de tu servidor.

### Acceso al Clúster de Kubernetes a través de Kubeconfig

Una vez conectado al nodo a través de SSH, puedes acceder al clúster de Kubernetes utilizando el archivo `kubeconfig`.

#### Uso del Archivo Kubeconfig

1.  **Ubicación de Kubeconfig**: Por defecto, el archivo `kubeconfig` se encuentra en `~/.kube/config` en el plano de control. Asegúrate de estar en el directorio correcto o especifica la ruta al archivo al ejecutar comandos de `kubectl`.

2.  **Acceso al clúster**: Para acceder al clúster, utilizamos el siguiente comando dentro del directorio de trabajo predeterminado:

    `export KUBECONFIG=kubeconfig`
    
3.  **Comprobación del Acceso**: Para verificar que tienes acceso al clúster, utiliza:
    
    `kubectl get nodes`
    
    Esto te mostrará información básica sobre el clúster de Kubernetes.
    
4.  **Gestión del Clúster**: Ahora puedes ejecutar comandos `kubectl` para administrar recursos, desplegar aplicaciones, monitorear el estado del clúster, y más.

Despliegue de DaemonSet Defender en Kubernetes 🌐
-------------------------------------------------------------

El DaemonSet Defender de Prisma Cloud es un agente de seguridad que se despliega en cada nodo de un clúster de Kubernetes. Funciona como un DaemonSet, asegurándose de que cada nodo en el clúster tenga un pod del Defender. Esto permite una cobertura completa y una protección consistente a través de todo el clúster.

### Objetivo

Configurar y desplegar el DaemonSet Defender de Prisma Cloud para una protección robusta en tu clúster de Kubernetes.

### Configuraciones en Prisma Cloud Compute

1. Inicie sesión en la consola de Prisma Cloud.
2. Navegue a `Manage > Defenders > Manual Deploy`.
3. Seleccione **Kubernetes** como el tipo de Defender.
4. Configure las opciones de despliegue según sus necesidades.

### Despliegue del DaemonSet Defender

1. Inicie sesión en el servidor del Plano de Control.
2. Abra la terminal y cargue el archivo YAML.
3. Cree un namespace para todas las actividades que realizará durante el workshop con `kubectl create namespace <namespace>`
4. Ejecute `kubectl apply -f <nombre_del_archivo>.yaml -n <namespace>`.
5. Verifique los pods con `kubectl get pods -n <namespace>`.

### Verificación y Monitoreo

1. Monitoree los Defenders desde la consola de Prisma Cloud.
2. Revise el estado y los logs en `Manage > Defenders > Defenders:Deployed`.

Despliegue de Manifiestos para Pruebas de Runtime Protection 🛡️
--------------------------------------------------------------------------

La protección en tiempo de ejecución se refiere a la capacidad de detectar y mitigar amenazas y actividades maliciosas mientras las aplicaciones están en funcionamiento. A diferencia de las medidas preventivas que se aplican durante las etapas de desarrollo o despliegue, la protección en tiempo de ejecución se ocupa de identificar y reaccionar a los problemas en vivo.

### Objetivo

Experimentar las capacidades de protección en tiempo de ejecución de Prisma Cloud.

### Creación y Ejecución de un Pod de Pruebas

1. Cree un Pod para simular procesos comunes en ataques.
2. Use `kubectl apply -f pod-process.yaml -n <namespace>`.
3. Acceda al contenedor con `kubectl exec -it process-pod -n <NAME> -- /bin/sh`.
4. Ejecute comandos de prueba como `nmap`, `curl`, y `wget`.

### Ejemplo de Manifiesto de Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: process-pod
spec:
  containers:
  - name: tools-container
    image: alpine
    command: ["/bin/sh"]
    args: ["-c", "apk update; apk add --no-cache nmap curl wget; sleep infinity"]
```

Al realizar pruebas de seguridad en entornos de Kubernetes, es fundamental ser consciente de los riesgos asociados con el uso de herramientas como `nmap`, `wget` y `curl`. Aunque estas herramientas son esenciales para probar y asegurar redes y aplicaciones, su uso indebido o no controlado puede representar riesgos de seguridad significativos.

### Riesgos Asociados con Herramientas Comunes

1.  **nmap**:
    
    *   **Riesgo**: `nmap` es una potente herramienta de escaneo de red que puede revelar información sensible sobre los servicios en ejecución y sus puertos abiertos. Su uso no autorizado puede ser un indicativo de reconocimiento por parte de un atacante.
    *   **Protección**: Monitorizar el uso de `nmap` y establecer reglas para alertar sobre escaneos no autorizados.
2.  **wget**:
    
    *   **Riesgo**: `wget` puede descargar contenido y scripts maliciosos de Internet. En manos equivocadas, podría facilitar la introducción de malware o contenido no deseado en el sistema.
    *   **Protección**: Restringir el uso de `wget` a fuentes confiables y monitorizar las descargas para detectar comportamientos inusuales.
3.  **curl**:
    
    *   **Riesgo**: Similar a `wget`, `curl` puede ser utilizado para descargar archivos o interactuar con servicios externos. Puede ser utilizado en ataques de exfiltración de datos o para recibir comandos de un servidor de comando y control.
    *   **Protección**: Supervisar el uso de `curl`, especialmente en lo que respecta a destinos externos y asegurar que solo se comunique con destinos confiables.

### Acceso al Contenedor y Ejecución de Herramientas de Pruebas

Una vez que hayas desplegado el pod de pruebas, es momento de interactuar con él y ejecutar herramientas de seguridad comunes para observar cómo Prisma Cloud responde a diversas actividades.

### Pasos para Acceder al Contenedor y Ejecutar Pruebas

1.  **Acceder al Contenedor**: Utiliza el siguiente comando para acceder al contenedor que acabas de desplegar:
    
    `kubectl exec -it process-pod -n <namespace> -- /bin/sh`
    
2.  **Ejecución de nmap**:
    
    *   **Propósito**: `nmap` se utiliza para escanear puertos y detectar servicios en una red.
    *   **Comando de Ejemplo**:
        
        `nmap -p 80 google.com`
        
    *   Este comando escaneará el puerto 80 en `google.com`.
      
3.  **Uso de wget**:
    
    *   **Propósito**: `wget` se utiliza para descargar archivos o páginas web desde la línea de comandos.
    *   **Comando de Ejemplo**:
        
        `wget https://amazon.com`
        
    *   Este comando descargará la página de inicio de `amazon.com`.
      
4.  **Aplicación de curl**:
    
    *   **Propósito**: `curl` es una herramienta para transferir datos desde o hacia un servidor, utilizando una variedad de protocolos.
    *   **Comando de Ejemplo**:
        
        bashCopy code
        
        `curl -I https://facebook.com`
        
    *   Este comando muestra las cabeceras HTTP de `facebook.com`.

### Observación y Aprendizaje

Mientras ejecutas estos comandos, observa cómo Prisma Cloud detecta y responde a estas actividades. Estas herramientas, aunque benignas en sí mismas, pueden ser utilizadas para actividades maliciosas y, por lo tanto, pueden desencadenar alertas en sistemas de seguridad.

*   **Monitorización en Prisma Cloud**: Asegúrate de revisar las alertas y registros en Prisma Cloud para ver cómo se manejan estas herramientas y comandos.

## Tu Rol Como Participante 🌟

- **Ejecuta Comandos**: Sé parte activa de las pruebas de seguridad.
- **Observa y Aprende**: Descubre cómo Prisma Cloud maneja diversas amenazas en tiempo real.

Despliegue y Análisis de Imagen XMRig con Prisma Cloud 🛡️
---------------------------------------------------------------------

La imagen `rcmelendez/xmrig` es una implementación de XMRig, un software popular para la minería de criptomonedas. Esta imagen, creada por el usuario `rcmelendez`, se utiliza en este workshop para demostrar la eficacia de Prisma Cloud en la detección de actividades potencialmente maliciosas.

El objetivo de esta sección del workshop es experimentar cómo Prisma Cloud detecta y maneja la ejecución de un contenedor que podría estar realizando actividades no deseadas, como la minería de criptomonedas.

### Despliegue del Pod

1. **Creación del Pod**:
 Utilice el siguiente manifiesto para crear un pod con XMRig:

```
   apiVersion: v1
   kind: Pod
   metadata:
     name: xmrig-pod
   spec:
     containers:
     - name: xmrig-container
       image: rcmelendez/xmrig
       resources:
         limits:
           cpu: "1"
           memory: "500Mi"
         requests:
           cpu: "0.5"
           memory: "250Mi"
```

### Ejecución:

Despliegue el pod con el siguiente comando:

```
kubectl apply -f xmrig-pod.yaml -n <namespace>
```

### Monitoreo con Prisma Cloud

**Observación**: Monitoree las alertas y eventos en Prisma Cloud tras el despliegue del pod para observar su comportamiento.

### Consideraciones Importantes

*   **Uso de un Entorno Controlado**: Es vital realizar estas pruebas en un entorno seguro y aislado, para evitar impactos no deseados.
*   **Propósito Educativo**: Este ejercicio está diseñado con fines educativos, enfocándose en entender cómo Prisma Cloud responde a actividades potencialmente riesgosas.

Despliegue de Damn Vulnerable Web Application (DVWA) en Kubernetes 🕸️
-----------------------------------------------------------------------

Damn Vulnerable Web Application (DVWA) es una aplicación web PHP/MySQL que ha sido diseñada con múltiples vulnerabilidades de seguridad intencionales. Es una herramienta educativa utilizada por profesionales de la seguridad y desarrolladores para aprender y practicar las habilidades de seguridad web en un entorno controlado.

### Objetivo

Desplegar DVWA en tu clúster de Kubernetes para permitir la interacción con una aplicación web intencionalmente vulnerable, proporcionando un entorno práctico para aprender sobre seguridad en aplicaciones web.

### Creación de Namespace Individual

Cada participante debe crear un namespace único en Kubernetes para aislar su instancia de DVWA:

### Despliegue del DVWA

Utilice el siguiente manifiesto de Kubernetes para desplegar DVWA en su namespace:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dvwa-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dvwa
  template:
    metadata:
      labels:
        app: dvwa
    spec:
      containers:
      - name: dvwa
        image: vulnerables/web-dvwa
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: dvwa-service
spec:
  selector:
    app: dvwa
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### Ejecución:

1.  Despliegue DVWA en su namespace con el siguiente comando:
    
    `kubectl apply -f dvwa.yaml -n <namespace>`

### Verificación y Acceso

1.  Verifique que los pods estén funcionando correctamente:
    
    `kubectl get pods -n <namespace>`
    
2.  Acceda a DVWA a través del LoadBalancer utilizando la IP externa del servicio:

   `kubectl get svc -n <namespace>

### Monitoreo y Observación con Prisma Cloud

*   **Monitoreo Continuo**: Utilice Prisma Cloud para monitorear las instancias DVWA desplegadas y observar las vulnerabilidades y eventos de seguridad.

### Consideraciones Importantes

*   **Entorno Seguro**: DVWA es una aplicación vulnerable por diseño. Asegúrese de desplegarla en un entorno controlado y aislado.
*   **Fines Educativos**: Este despliegue está orientado al aprendizaje y la experimentación en el campo de la seguridad informática.

Pruebas de Seguridad en DVWA 🛡️
--------------------------------

### Objetivo

Realizar pruebas de seguridad utilizando DVWA para entender y aprender sobre diferentes tipos de vulnerabilidades web. Se abordarán tres tipos de ataques: inyección SQL, Cross-Site Scripting (XSS) y Command Injection.

### 1\. Inyección SQL Avanzada

La inyección SQL explota vulnerabilidades en la interacción entre una aplicación y su base de datos. El objetivo es manipular o extraer datos de manera no autorizada.

**Ejemplo de Comando de Prueba**:

*   Acceda a la sección SQL Injection en DVWA.
*   Intente el siguiente comando en el campo de entrada:
    
    sqlCopy code
    
    `' UNION SELECT null, username, password FROM users --`
    
*   Este comando utiliza la operación `UNION` para combinar el resultado de la consulta con datos seleccionados de la tabla de usuarios, revelando nombres de usuario y contraseñas.

### 2\. Cross-Site Scripting (XSS) Reflejado Avanzado

XSS permite a un atacante inyectar scripts en páginas vistas por otros usuarios, lo que puede llevar al robo de sesiones de usuario o a la alteración de la experiencia del usuario.

**Ejemplo de Comando de Prueba**:

*   Vaya a la sección XSS (Reflected) en DVWA.
*   En el campo de entrada correspondiente, intente el siguiente script:
    
    javascriptCopy code
    
    `<script>fetch('/').then(resp => resp.text()).then(text => alert(text))</script>`
    
*   Este script más avanzado utiliza `fetch` para obtener el contenido de la página actual y luego lo muestra en una alerta, demostrando la capacidad de ejecutar scripts más complejos.

### 3\. Command Injection

La inyección de comandos permite a un atacante ejecutar comandos arbitrarios en el servidor a través de una aplicación web vulnerable.

**Ejemplo de Comando de Prueba**:

*   Diríjase a la sección Command Injection en DVWA.
*   Utilice el siguiente comando en el campo de entrada:
    
    bashCopy code
    
    `; cat /etc/passwd #`
    
*   Este comando aprovecha la vulnerabilidad para ejecutar el comando `cat /etc/passwd`, permitiendo al atacante ver los nombres de usuario en el servidor.

### Consideraciones Importantes

*   Estos ejemplos de comandos son para propósitos educativos y deben ser usados solo en entornos de prueba seguros y controlados.
*   La comprensión y prueba de estos ataques ayuda a desarrollar mejores prácticas de programación y estrategias de defensa en aplicaciones web.
### Monitoreo y Observación con Prisma Cloud

*   **Observación**: Utilice Prisma Cloud para monitorear estas pruebas y observe cómo se detectan y gestionan los intentos de ataque.

### Consideraciones Importantes

*   **Entorno Seguro y Controlado**: Realice estas pruebas en un entorno seguro y no en aplicaciones de producción.
*   **Fines Educativos**: Estas pruebas deben realizarse solo con fines educativos y de aprendizaje sobre la seguridad informática.

Introducción a Contenedores de Red de Seguridad (CNNS) en Kubernetes con Prisma Cloud 🛡️
-----------------------------------------------------------------------------------------

### Objetivo

Comprender los fundamentos de Cloud Native Network Segmentation (CNNS) en Kubernetes y aprender a configurar y aplicar políticas de seguridad mediante Prisma Cloud, una plataforma integral para la seguridad de aplicaciones cloud native.

### Introducción Teórica a CNNS en Prisma Cloud

CNNS es una capacidad esencial en la protección de entornos Kubernetes, especialmente en la era de la nube y las arquitecturas de microservicios. Prisma Cloud, una solución avanzada para la seguridad en la nube, aborda estos desafíos ofreciendo una protección integral en la red para contenedores y servicios en la nube.  

#### Aplicación de CNNS con Prisma Cloud

Prisma Cloud permite a las organizaciones implementar CNNS de forma efectiva en sus clústeres de Kubernetes, aprovechando sus capacidades avanzadas para obtener una protección sólida y una gestión de riesgos eficaz. Los usuarios pueden definir políticas detalladas, monitorear el tráfico e investigar rápidamente los incidentes, todo dentro de una interfaz unificada y fácil de usar.

### Despliegue de Entorno de Prueba para CNNS

Para experimentar con CNNS, desplegaremos una aplicación de muestra en Kubernetes.

**Manifiesto de Kubernetes**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_HOST
          value: mysql
        - name: WORDPRESS_DB_USER
          value: wordpress
        - name: WORDPRESS_DB_PASSWORD
          value: wordpress
        - name: WORDPRESS_DB_NAME
          value: wordpress

---
# mysql-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:latest
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: rootpassword
        - name: MYSQL_DATABASE
          value: wordpress
        - name: MYSQL_USER
          value: wordpress
        - name: MYSQL_PASSWORD
          value: wordpress

---
# service.yaml

apiVersion: v1
kind: Service
metadata:
  name: wordpress
spec:
  selector:
    app: wordpress
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
---
# service-mysql.yaml

apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```

**Instrucciones de Despliegue**:

1.  Despliegue el pod en Kubernetes con:
    
    bashCopy code
    
    `kubectl apply -f cnns.yaml -n <namespace>`
    
### Pasos para la Verificación de Conexión

1.  **Comprobación del Estado de los Pods**: Utilice el comando `kubectl get pods` para revisar el estado de los pods de WordPress y MySQL. Ambos deben estar en estado `Running`.
    
    `kubectl get pods -n <namespace>`
    
2.  **Acceso al Interfaz de WordPress**: Si ha configurado un servicio LoadBalancer, intente acceder a la interfaz web de WordPress a través de un navegador. La capacidad de iniciar sesión y realizar operaciones indica una conexión exitosa. Obtenga la IP externa con `kubectl get svc -n <namespace>`
    
4.  **Pruebas de Conectividad Directa**: Realice un chequeo de conectividad directa desde el pod de WordPress al pod de MySQL.
    
    `kubectl exec -it [nombre-pod-wordpress] -- /bin/sh`
    
    Intente obtener información de manera directa al pod de MySQL `curl hhtp://<EXTERNAL_IP>:<PORT>`
    
### Monitoreo y Observación con Prisma Cloud

*   **Observación**: Utilice Prisma Cloud para monitorear el cumplimiento de las políticas de CNNS y detectar cualquier desviación o intento de ataque.

### Consideraciones Importantes

*   **Prácticas Seguras**: Asegúrese de seguir las mejores prácticas de seguridad al configurar CNNS.
*   **Fines Educativos**: Este módulo está destinado para fines educativos y de aprendizaje sobre la implementación y gestión de CNNS en Kubernetes.

Agradecimiento por Participar en el Workshop 🌟
-----------------------------------------------

### Gracias por su Participación

Queremos expresar nuestro más sincero agradecimiento a todos los participantes por dedicar su tiempo y esfuerzo en este workshop. Su entusiasmo y compromiso han sido fundamentales para el éxito de esta jornada de aprendizaje.

### Reflexiones Finales

Esperamos que las sesiones y las actividades del workshop hayan sido enriquecedoras y que hayan proporcionado valiosos conocimientos y habilidades prácticas en Kubernetes, seguridad en la nube y las tecnologías relacionadas.

### Continuemos Aprendiendo

Les animamos a todos a continuar explorando, aprendiendo y aplicando lo que han adquirido aquí en sus proyectos con Prisma Cloud. Manténganse conectados para futuros eventos y oportunidades de aprendizaje con **NETDATA INNOVATION CENTER**.

### ¡Hasta Pronto!

Esperamos verlos nuevamente en nuestros próximos eventos y talleres. ¡Hasta pronto y sigan innovando!
