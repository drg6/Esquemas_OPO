# Tema 41.- Virtualización de servidores. Infraestructura del puesto de trabajo virtual (VDI). Virtualización de contenedores: Docker. Ventajas, funcionamiento y limitaciones. Plataformas para la organización de contenedores: Kubernetes.

## 1. Introducción

Hasta finales de la década de 1990, los Centros de Proceso de Datos operaban bajo el paradigma "**un servidor físico = un sistema operativo = una aplicación**". Un servidor dedicado a la base de datos de tributos utilizaba únicamente el 10-15% de su capacidad de CPU, pero no podía compartir esos recursos con el servidor de correo electrónico. El resultado era un parque de servidores infrautilizados, costosos de mantener, difíciles de escalar y energéticamente ineficientes.

La **virtualización** resuelve este problema interponiendo una capa de software — el **hipervisor** — entre el hardware físico y los sistemas operativos, permitiendo ejecutar múltiples máquinas virtuales independientes sobre un mismo servidor físico.

## 2. Virtualización de Servidores

### 2.1. Concepto

La virtualización de servidores permite que un único servidor físico (host) ejecute múltiples **máquinas virtuales (VM)**, cada una con su propio sistema operativo y aplicaciones, de forma completamente aislada.

### 2.2. El Hipervisor (Virtual Machine Monitor — VMM)

El **hipervisor** es el software que gestiona y asigna los recursos del hardware físico (CPU, RAM, disco, red) a las máquinas virtuales.

#### Tipo 1 — Bare Metal (Nativo)

Se instala directamente sobre el hardware físico, sin sistema operativo intermedio. Es el estándar en entornos de producción empresarial.

| Hipervisor | Fabricante |
|-----------|-----------|
| **VMware ESXi** | VMware (Broadcom) |
| **Microsoft Hyper-V** | Microsoft |
| **KVM** | Open source (integrado en el kernel de Linux) |
| **Citrix Hypervisor (XenServer)** | Citrix |

#### Tipo 2 — Hosted (Alojado)

Se instala como una aplicación sobre un sistema operativo convencional. Adecuado para entornos de desarrollo y pruebas, no para producción.

| Hipervisor | Uso |
|-----------|-----|
| **Oracle VirtualBox** | Desarrollo, laboratorios |
| **VMware Workstation / Fusion** | Desarrollo en Windows/Mac |

### 2.3. Ventajas de la virtualización

*   **Consolidación:** Múltiples servidores lógicos en un servidor físico (ratio típico 10:1). Reducción del parque hardware.
*   **Eficiencia energética:** Menor consumo eléctrico y menor necesidad de refrigeración.
*   **Aislamiento:** Un fallo en una VM no afecta a las demás.
*   **Portabilidad:** Las VMs pueden migrarse entre servidores físicos (vMotion en VMware, Live Migration en Hyper-V) sin interrupción del servicio.
*   **Snapshots:** Capturas del estado de la VM para restauración rápida en caso de fallo tras una actualización.
*   **Aprovisionamiento rápido:** Crear una nueva VM a partir de una plantilla en minutos, frente a días o semanas para adquirir e instalar un servidor físico.

### 2.4. Gestión centralizada

*   **VMware vCenter Server:** Plataforma para gestionar clústeres de hosts ESXi, balanceo de carga (DRS), alta disponibilidad (HA), migración en caliente (vMotion).
*   **Microsoft System Center VMM:** Gestión de hosts Hyper-V.
*   **Proxmox VE:** Solución open source para gestión de KVM y contenedores LXC.

## 3. Infraestructura de Escritorio Virtual (VDI)

### 3.1. Concepto

**VDI (Virtual Desktop Infrastructure)** extiende la virtualización al puesto de trabajo del usuario. En lugar de ejecutar el sistema operativo y las aplicaciones en un PC físico, el escritorio completo (Windows 10/11) se ejecuta como una máquina virtual en los servidores del CPD.

### 3.2. Arquitectura

```
[Thin Client / PC] → [Protocolo de display] → [Broker de conexiones] → [Hipervisor] → [VM de escritorio]
       (oficina)       (PCoIP, ICA, RDP)          (Connection Broker)      (CPD)
```

*   **Thin Client:** Dispositivo ligero (bajo coste, bajo consumo, sin disco duro) que solo muestra la imagen del escritorio remoto.
*   **Broker de conexiones:** Software que asigna dinámicamente una VM de escritorio a cada usuario que se conecta (VMware Horizon, Citrix Virtual Apps and Desktops, Microsoft AVD).
*   **Protocolos de display:** PCoIP (VMware), ICA/HDX (Citrix), RDP (Microsoft).

### 3.3. Ventajas para las AAPP

*   **Seguridad:** Los datos nunca residen en el dispositivo del usuario; toda la información permanece en el CPD.
*   **Gestión centralizada:** Actualización de una imagen maestra afecta a todos los escritorios.
*   **Teletrabajo:** El funcionario accede a su escritorio corporativo desde cualquier lugar.
*   **Continuidad:** Si se rompe un Thin Client, se sustituye en minutos; el escritorio sigue disponible.

### 3.4. Limitaciones

*   **Dependencia de la red:** Requiere conectividad estable y con baja latencia.
*   **Coste de licencias:** VMware Horizon, Citrix tienen costes de licencia significativos.
*   **Rendimiento gráfico:** Aplicaciones con requisitos gráficos intensivos (CAD, GIS) pueden tener menor rendimiento.

## 4. Virtualización de Contenedores: Docker

### 4.1. Concepto

Mientras que la virtualización de servidores ejecuta sistemas operativos completos dentro de VMs, la **virtualización de contenedores** empaqueta la aplicación y sus dependencias (bibliotecas, runtime) en un **contenedor** ligero que comparte el kernel del sistema operativo host. No necesita arrancar un SO completo por instancia.

### 4.2. Docker: Arquitectura

| Componente | Función |
|-----------|---------|
| **Docker Engine** | Daemon que gestiona los contenedores sobre el kernel Linux (namespaces, cgroups) |
| **Imagen (Docker Image)** | Plantilla de solo lectura que define el contenido del contenedor (código, librerías, configuración) |
| **Contenedor (Container)** | Instancia en ejecución de una imagen |
| **Dockerfile** | Fichero de texto que define los pasos para construir una imagen |
| **Docker Hub / Registry** | Repositorio de imágenes (públicas y privadas) |

### 4.3. Ejemplo de Dockerfile

```dockerfile
FROM openjdk:17-slim
COPY target/app.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

### 4.4. VM vs. Contenedor

| Aspecto | Máquina Virtual | Contenedor Docker |
|---------|----------------|-------------------|
| Tamaño | Gigabytes (incluye SO completo) | Megabytes (solo app + dependencias) |
| Arranque | Minutos | Segundos / milisegundos |
| Aislamiento | Completo (hardware virtualizado) | A nivel de proceso (kernel compartido) |
| Portabilidad | Portable entre hipervisores | Portable entre cualquier host con Docker |
| Rendimiento | Overhead del hipervisor | Rendimiento nativo (sin overhead de SO) |
| Seguridad | Mayor aislamiento | Menor aislamiento (comparten kernel) |

### 4.5. Ventajas de Docker

*   **Portabilidad:** "Build once, run anywhere". Un contenedor funciona igual en desarrollo, pruebas y producción.
*   **Ligereza:** Arranque instantáneo, consumo mínimo de recursos.
*   **Reproducibilidad:** El Dockerfile documenta exactamente cómo se construye el entorno.
*   **DevOps y CI/CD:** Integración natural con pipelines de integración y despliegue continuos.

### 4.6. Limitaciones de Docker

*   **Seguridad:** El kernel compartido implica una superficie de ataque mayor que las VMs. Un escape de contenedor podría comprometer el host.
*   **Estado (stateful):** Los contenedores son efímeros por diseño. Gestionar datos persistentes (bases de datos) requiere volúmenes y configuración adicional.
*   **Complejidad operativa:** A escala (cientos de contenedores), la gestión manual es inviable.

## 5. Orquestación de Contenedores: Kubernetes

### 5.1. Concepto

**Kubernetes (K8s)** es una plataforma open source de **orquestación de contenedores** desarrollada originalmente por Google y actualmente mantenida por la Cloud Native Computing Foundation (CNCF). Automatiza el despliegue, el escalado y la gestión de aplicaciones containerizadas.

### 5.2. Arquitectura

| Componente | Función |
|-----------|---------|
| **Pod** | Unidad mínima de despliegue. Contiene uno o más contenedores que comparten red y almacenamiento |
| **Node** | Máquina (física o virtual) que ejecuta Pods |
| **Cluster** | Conjunto de Nodes gestionados por Kubernetes |
| **Control Plane** | Componentes de gestión: API Server, Scheduler, Controller Manager, etcd |
| **kubelet** | Agente en cada Node que ejecuta y monitoriza los Pods |
| **Service** | Punto de acceso estable (IP fija, DNS) a un conjunto de Pods |

### 5.3. Funcionalidades principales

*   **Autoescalado (HPA — Horizontal Pod Autoscaler):** Ajusta automáticamente el número de réplicas de un Pod según la carga (CPU, memoria, métricas personalizadas).
*   **Self-healing:** Reinicia automáticamente contenedores que fallan, reemplaza Pods en nodos caídos.
*   **Rolling updates:** Actualización gradual de la aplicación sin interrupción del servicio.
*   **Service discovery y balanceo de carga:** Los servicios se descubren automáticamente por DNS interno.
*   **Gestión de secretos y configuración:** ConfigMaps y Secrets para inyectar configuración sin modificar las imágenes.
*   **Almacenamiento persistente:** PersistentVolumes para datos que deben sobrevivir al ciclo de vida del Pod.

### 5.4. Servicios gestionados de Kubernetes

| Servicio | Proveedor |
|----------|----------|
| **Amazon EKS** | AWS |
| **Azure AKS** | Microsoft Azure |
| **Google GKE** | Google Cloud |
| **OpenShift** | Red Hat (on-premise / cloud) |

## 6. Conclusión

La virtualización de servidores (VMware ESXi, Hyper-V, KVM) permite consolidar la infraestructura del CPD, reducir costes y mejorar la agilidad. La infraestructura de escritorio virtual (VDI) extiende estos beneficios al puesto de trabajo, centralizando la seguridad y facilitando el teletrabajo. Docker lleva la virtualización un paso más allá, empaquetando aplicaciones en contenedores ligeros y portables. Y Kubernetes proporciona la orquestación necesaria para gestionar aplicaciones containerizadas a escala, con autoescalado, self-healing y despliegues sin interrupción.

Estas tecnologías constituyen los pilares de la modernización tecnológica de las Administraciones Públicas, permitiendo despliegues más ágiles, mejor aprovechamiento de los recursos y mayor resiliencia de los servicios públicos digitales.
