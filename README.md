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

## Tu Rol Como Participante 🌟

- **Ejecuta Comandos**: Sé parte activa de las pruebas de seguridad.
- **Observa y Aprende**: Descubre cómo Prisma Cloud maneja diversas amenazas en tiempo real.


