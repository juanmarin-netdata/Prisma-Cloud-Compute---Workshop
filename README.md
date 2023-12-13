# Prisma Cloud Compute

## Introducción 🚀

Prisma Cloud Compute ofrece una protección integral para aplicaciones y cargas de trabajo Cloud Native en entornos modernos. Desde la seguridad en contenedores hasta la protección en tiempo de ejecución, te cubrimos en cada paso del ciclo de vida de tu aplicación.

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

## Despliegue de DaemonSet Defender en Kubernetes 🌐

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
3. Ejecute `kubectl apply -f <nombre_del_archivo>.yaml`.
4. Verifique los pods con `kubectl get pods -A`.

### Verificación y Monitoreo

1. Monitoree los Defenders desde la consola de Prisma Cloud.
2. Revise el estado y los logs en `Manage > Defenders > Defenders:Deployed`.

## Despliegue de Manifiestos para Pruebas de Runtime Protection 🛡️

### Objetivo

Experimente las capacidades de protección en tiempo real de Prisma Cloud.

### Creación y Ejecución de un Pod de Pruebas

1. Cree un Pod para simular procesos comunes en ataques.
2. Use `kubectl create namespace <NAME>` y `kubectl apply -f pod-process.yaml -n <NAME>`.
3. Acceda al contenedor con `kubectl exec -it process-pod -n <NAME>`.
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

## Tu Rol Como Participante 🌟

- **Ejecuta Comandos**: Sé parte activa de las pruebas de seguridad.
- **Observa y Aprende**: Descubre cómo Prisma Cloud maneja diversas amenazas en tiempo real.

## Despliegue y Análisis de Imagen XMRig con Prisma Cloud

La imagen `rcmelendez/xmrig` es una implementación de XMRig, un software popular para la minería de criptomonedas. Esta imagen, creada por el usuario `rcmelendez`, se utiliza en este workshop para demostrar la eficacia de Prisma Cloud en la detección de actividades potencialmente maliciosas.

El objetivo de esta sección del workshop es experimentar cómo Prisma Cloud detecta y maneja la ejecución de un contenedor que podría estar realizando actividades no deseadas, como la minería de criptomonedas.

### Despliegue del Pod

1. **Creación del Pod**:
 Utilice el siguiente manifiesto para crear un pod con XMRig:

```yaml
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

```bash
kubectl apply -f <archivo>.yaml -n <NAME>
```

### Monitoreo con Prisma Cloud

**Observación**: Monitoree las alertas y eventos en Prisma Cloud tras el despliegue del pod para observar su comportamiento.

### Consideraciones Importantes

*   **Uso de un Entorno Controlado**: Es vital realizar estas pruebas en un entorno seguro y aislado, para evitar impactos no deseados.
*   **Propósito Educativo**: Este ejercicio está diseñado con fines educativos, enfocándose en entender cómo Prisma Cloud responde a actividades potencialmente riesgosas.

## Despliegue de Damn Vulnerable Web Application (DVWA) en Kubernetes 🕸️

### Objetivo

Desplegar DVWA en tu clúster de Kubernetes para permitir a los participantes interactuar con una aplicación web intencionalmente vulnerable, proporcionando un entorno práctico para aprender sobre seguridad en aplicaciones web.

### Creación de Namespace Individual

Cada participante debe crear un namespace único en Kubernetes para aislar su instancia de DVWA:

1. **Creación del Namespace**:
   Ejecute el siguiente comando, reemplazando `<NOMBRE_PARTICIPANTE>` con su nombre o identificador único:
   ```bash
   kubectl create namespace <NOMBRE_PARTICIPANTE>
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

1.  Guarde el manifiesto anterior en un archivo llamado `dvwa-kubernetes.yaml`.
2.  Reemplace `<NOMBRE_PARTICIPANTE>` con su namespace específico.
3.  Despliegue DVWA en su namespace con el siguiente comando:
    
    bashCopy code
    
    `kubectl apply -f dvwa-kubernetes.yaml -n <NOMBRE_PARTICIPANTE>`
    

### Verificación y Acceso

1.  Verifique que los pods estén funcionando correctamente:
    
    bashCopy code
    
    `kubectl get pods -n <NOMBRE_PARTICIPANTE>`
    
2.  Acceda a DVWA a través del LoadBalancer o mediante port-forwarding.

### Monitoreo y Observación con Prisma Cloud

*   **Monitoreo Continuo**: Utilice Prisma Cloud para monitorear las instancias DVWA desplegadas y observar las vulnerabilidades y eventos de seguridad.

### Consideraciones Importantes

*   **Entorno Seguro**: DVWA es una aplicación vulnerable por diseño. Asegúrese de desplegarla en un entorno controlado y aislado.
*   **Fines Educativos**: Este despliegue está orientado al aprendizaje y la experimentación en el campo de la seguridad informática.

Pruebas de Seguridad en DVWA 🛡️
--------------------------------

### Objetivo

Realizar pruebas de seguridad utilizando DVWA para entender y aprender sobre diferentes tipos de vulnerabilidades web. Se cubrirán tres tipos de ataques: inyección SQL, Cross-Site Scripting (XSS) y Fuerza Bruta.

### 1\. Inyección SQL

La inyección SQL aprovecha las vulnerabilidades en la interacción de una aplicación con su base de datos. El objetivo es manipular o extraer datos.

**Ejemplo de Comando de Prueba**:

*   Acceda a la sección SQL Injection en DVWA.
*   En el campo de entrada, intente el siguiente comando:
    
    sqlCopy code
    
    `' OR '1'='1`
    
*   Este comando busca alterar la lógica de la consulta SQL para acceder a datos no autorizados.

### 2\. Cross-Site Scripting (XSS)

XSS permite a un atacante inyectar scripts en páginas vistas por otros usuarios, potencialmente robando sesiones de usuario o dañando la experiencia del usuario.

**Ejemplo de Comando de Prueba**:

*   Vaya a la sección XSS en DVWA.
*   En el campo de entrada correspondiente, intente el siguiente script:
    
    javascriptCopy code
    
    `<script>alert('XSS')</script>`
    
*   Este script ejecutará una alerta en el navegador, demostrando la ejecución de código del lado del cliente.

### 3\. Ataque de Fuerza Bruta

Este ataque busca ganar acceso a un sitio probando numerosas combinaciones de usuario y contraseña hasta que una sea exitosa.

**Ejemplo de Comando de Prueba**:

*   Diríjase a la sección Brute Force en DVWA.
*   Utilice herramientas como Hydra o realice intentos manuales para tratar de adivinar las credenciales.
*   Un ejemplo de credenciales comunes es usar "admin" tanto para el usuario como para la contraseña.

### Monitoreo y Observación con Prisma Cloud

*   **Observación**: Utilice Prisma Cloud para monitorear estas pruebas y observe cómo se detectan y gestionan los intentos de ataque.

### Consideraciones Importantes

*   **Entorno Seguro y Controlado**: Realice estas pruebas en un entorno seguro y no en aplicaciones de producción.
*   **Fines Educativos**: Estas pruebas deben realizarse solo con fines educativos y de aprendizaje sobre la seguridad informática.

Introducción a Contenedores de Red de Seguridad (CNNS) en Kubernetes 🛡️
------------------------------------------------------------------------

### Objetivo

Comprender los fundamentos de los Contenedores de Red de Seguridad (CNNS) en Kubernetes y aprender a configurar y aplicar políticas de seguridad mediante Prisma Cloud.

### Introducción Teórica a CNNS

Los CNNS son una parte crucial de la seguridad en entornos de Kubernetes. Actúan como barreras virtuales, controlando el tráfico de red que entra y sale de los contenedores. Aseguran que solo el tráfico autorizado pueda comunicarse con los contenedores, mitigando así los riesgos de ataques y vulnerabilidades de red.

**Características Clave de los CNNS**:

*   **Aislamiento de Red**: Separan el tráfico de red entre diferentes servicios y aplicaciones.
*   **Reglas de Tráfico**: Permiten definir políticas detalladas para controlar cómo los contenedores pueden comunicarse entre sí.
*   **Monitorización y Registro**: Proporcionan herramientas para supervisar y registrar el tráfico de red, facilitando la detección de actividades sospechosas.

### Despliegue de Entorno de Prueba para CNNS

Para experimentar con CNNS, desplegaremos una aplicación de muestra en Kubernetes.

**Manifiesto de Kubernetes**:

yamlCopy code

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cnns-test-pod
spec:
  containers:
  - name: cnns-container
    image: nginx
    ports:
    - containerPort: 80
```

**Instrucciones de Despliegue**:

1.  Guarde el manifiesto en un archivo llamado `cnns-test.yaml`.
2.  Despliegue el pod en Kubernetes con:
    
    bashCopy code
    
    `kubectl apply -f cnns-test.yaml`
    

### Configuración de Reglas en Prisma Cloud para CNNS

1.  **Acceso a Prisma Cloud**: Inicie sesión en la consola de Prisma Cloud.
2.  **Navegación**: Vaya a `Network Security` > `Container Network Security`.
3.  **Creación de Reglas**:
    *   Cree una nueva regla seleccionando `Add Policy`.
    *   Defina el alcance de la regla (por ejemplo, aplicar a todos los pods o a un namespace específico).
    *   Configure las reglas de tráfico permitido (por ejemplo, permitir el tráfico HTTP en el puerto 80).
4.  **Aplicación de la Regla**:
    *   Asegúrese de que la regla esté activa y aplicada a los contenedores relevantes.
5.  **Monitorización**:
    *   Monitoree las actividades de red en la sección de logs y alertas para observar cómo se aplican las políticas.

### Monitoreo y Observación con Prisma Cloud

*   **Observación**: Utilice Prisma Cloud para monitorear el cumplimiento de las políticas de CNNS y detectar cualquier desviación o intento de ataque.

### Consideraciones Importantes

*   **Prácticas Seguras**: Asegúrese de seguir las mejores prácticas de seguridad al configurar CNNS.
*   **Fines Educativos**: Este módulo está destinado para fines educativos y de aprendizaje sobre la implementación y gestión de CNNS en Kubernetes.
