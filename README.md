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

