# Tema 41.- Virtualización de servidores. Infraestructura del puesto de trabajo virtual (VDI). Virtualización de contenedores: Docker. Ventajas, funcionamiento y limitaciones. Plataformas para la organización de contenedores: Kubernetes.

# Tema 41.- Virtualización (Servidores y VDI), Docker (Contenedores) y Kubernetes (Orquestación).

## 1. Introducción
* **Problema histórico:** Paradigma 1 servidor físico = 1 SO = 1 App. Genera hardware infrautilizado (15% CPU) y enorme gasto energético (CPD saturado).
* **Solución:** La virtualización interpone un **Hipervisor** que fragmenta el hardware físico en múltiples Máquinas Virtuales (VM) independientes.

## 2. Virtualización de Servidores
* **2.1. El Hipervisor:** Capa de software que asigna recursos (CPU, RAM, Disco) a las VM.
  * **Tipo 1 (Bare Metal):** Directo al hardware (ESXi, Hyper-V, KVM, Proxmox).
  * **Tipo 2 (Hosted):** Sobre un SO (VirtualBox). Solo para desarrollo.
* **2.2. Ventajas:** Consolidación (Ratio 10:1), aislamiento, *Snapshots*, Aprovisionamiento rápido mediante plantillas y **Portabilidad** (*vMotion* / *Live Migration* para mover VMs sin corte de servicio).
* **2.3. Gestión Centralizada y Actualidad AAPP:**
  * vCenter (VMware) y System Center (Hyper-V).
  * ⚠️ *Detalle Táctico:* Ante el drástico aumento de costes tras la compra de VMware por Broadcom, soluciones basadas en KVM (como **Proxmox VE**) están liderando la migración en la Administración Local.

## 3. VDI (Infraestructura de Escritorio Virtual)
* **3.1. Arquitectura:** El escritorio Windows no corre en el PC, sino en el CPD.
  `Thin Client → Protocolo (RDP, PCoIP) → Connection Broker → VM en Hipervisor`
* **3.2. Ventajas AAPP (El factor Zero Trust / ENS):**
  * La información (Tributos, Padrón) nunca viaja al equipo físico.
  * Si un teletrabajador infecta su PC doméstico con ransomware, no afecta a la red porque solo recibe píxeles de pantalla. 
  * Sustitución de hardware averiado en minutos (el Thin Client es "tonto").
* **3.3. Inconvenientes:** Alta dependencia de red (latencia) y coste de licencias.

## 4. Virtualización de Contenedores (Docker)
* **4.1. Concepto:** Empaqueta una app y sus dependencias compartiendo el *kernel* del host. No virtualiza el hardware, virtualiza el SO.
* **4.2. Elementos:** *Docker Engine*, *Imagen* (Plantilla de solo lectura), *Contenedor* (Instancia viva), *Dockerfile* (Código para crear la imagen).
* **4.3. Comparativa de Oro (VM vs. Docker):**
  * **VM:** Gigabytes de tamaño, arranca en minutos, aislamiento HW completo, penalización de rendimiento (Overhead).
  * **Docker:** Megabytes, arranca en milisegundos, aislamiento de proceso (comparte Kernel), rendimiento cuasi-nativo.
* **4.4. Limitaciones:** El contenedor es *Stateless* (efímero, requiere configurar volúmenes para persistir datos). Menor aislamiento de seguridad que una VM.

## 5. Orquestación (Kubernetes y alternativas)
* **5.1. El problema:** Gestionar 5 contenedores a mano es fácil. Gestionar 500 requiere automatización.
* **5.2. Escala pequeña (Docker Compose):** Orquesta multicontenedores (ej. App + BBDD) en un único host mediante un fichero YAML.
* **5.3. Escala Enterprise (Kubernetes - K8s):**
  * **Arquitectura:** *Control Plane* (Cerebro), *Nodes* (Obreros), *Pods* (Unidad mínima, contiene contenedores) y *Kubelet* (Agente).
  * **Magia operativa:** *Autoescalado* (HPA, sube/baja réplicas por CPU), *Self-Healing* (mata y resucita pods colgados), y *Rolling Updates* (actualizaciones sin caída de servicio).
* **5.4. Soluciones Cloud (Managed):** EKS (AWS), AKS (Azure), GKE (Google).

## 6. Conclusión
El CPD municipal ha evolucionado en tres saltos: la virtualización de servidores (ESXi/Proxmox) maximizó el hardware; la virtualización de escritorios (VDI) blindó el puesto de trabajo bajo premisas Zero Trust para el teletrabajo; y la revolución de Docker y Kubernetes ha llevado las aplicaciones corporativas al paradigma *Cloud Native*, garantizando escalabilidad inmediata y resiliencia ante caídas (Self-Healing).

--------------------------------------

## 1. Introducción

La **virtualización** permite aprovechar mejor los recursos del CPD mediante una capa de software (**hipervisor**) que permite ejecutar múltiples máquinas virtuales sobre un mismo servidor físico.
Soluciona el problema de servidores infrautilizados con solo una aplicacion por servidor.

## 2. Virtualización de Servidores

### 2.1. Concepto

Permite que un **servidor físico (host)** ejecute múltiples **máquinas virtuales (VM)** independientes, cada una con su propio SO y aplicaciones.

### 2.2. El Hipervisor (Virtual Machine Monitor — VMM)

Gestiona y asigna **CPU, RAM, disco y red** a las máquinas virtuales.

- **Tipo 1 – Bare Metal (Nativo):** instalado directamente sobre el hardware, sin sistema operativo intermedio. Entornos de producción
  - VMware ESXi
  - Microsoft Hyper-V
  - KVM
  - Citrix Hypervisor/XenServer

- **Tipo 2 – Hosted (Alojado):** instalado sobre un SO convencional. Entornos de desarrollo y pruebas.
  - Oracle VirtualBox
  - VMware Workstation/Fusion

### 2.3. Ventajas de la virtualización

- **Consolidación:** varios servidores lógicos en uno físico. Reducción del parque hardware.
- **Eficiencia energética:** menor consumo y refrigeración.
- **Aislamiento:** fallo de una VM no afecta a las demás.
- **Portabilidad:** migración entre servidores físicos sin interrupción del servicio.
  - VMware → **vMotion**
  - Hyper-V → **Live Migration**
- **Snapshots:** capturas para restauración rápida.
- **Aprovisionamiento rápido:** creación desde plantillas.

### 2.4. Gestión centralizada

- **VMware vCenter:** gestión clústeres de hosts ESXi, balanceo de carga (DRS), alta disponibilidad (HA), migración en caliente (vMotion).
- **System Center VMM:** gestión de Hyper-V.
- **Proxmox VE:** solución open source para KVM y LXC.

Ante el drástico aumento de costes tras la compra de VMware por Broadcom, soluciones basadas en KVM (como **Proxmox VE**) están liderando la migración en la Administración Local.

## 3. Infraestructura de Escritorio Virtual (VDI)

### 3.1. Concepto

**VDI (Virtual Desktop Infrastructure):** el escritorio completo del usuario se ejecuta como una **VM en el CPD**, en lugar de hacerlo en el PC físico.

### 3.2. Arquitectura

**Thin Client/PC → Protocolo de display → Broker → Hipervisor → VM de escritorio**

- **Thin Client:** dispositivo ligero que muestra el escritorio remoto.
- **Broker:** asigna una VM al usuario.
  - VMware Horizon
  - Citrix Virtual Apps and Desktops
  - Microsoft AVD
- **Protocolos:** PCoIP (VMware), ICA/HDX (Citrix), RDP (Microsoft).

* **3.2. Ventajas AAPP (El factor Zero Trust / ENS):**
  * La información (Tributos, Padrón) nunca viaja al equipo físico.
  * Si un teletrabajador infecta su PC doméstico con ransomware, no afecta a la red porque solo recibe píxeles de pantalla. 
  * Sustitución de hardware averiado en minutos (el Thin Client es "tonto").

### 3.3. Ventajas para las AAPP

- **Seguridad (El factor Zero Trust / ENS):** los datos permanecen en el CPD. La información (Tributos, Padrón) nunca viaja al equipo físico.
Si un teletrabajador infecta su PC doméstico con ransomware, no afecta a la red porque solo recibe píxeles de pantalla. 
- **Gestión centralizada:** actualización de una imagen maestra.
- **Teletrabajo:** acceso al escritorio desde cualquier lugar.
- **Continuidad:** sustitución rápida del dispositivo físico.

### 3.4. Limitaciones

- **Dependencia de la red:** requiere conexión estable y baja latencia.
- **Coste de licencias:** especialmente VMware/Citrix.
- **Rendimiento gráfico:** puede ser inferior en CAD, GIS, etc.

## 4. Virtualización de Contenedores: Docker

### 4.1. Concepto

Los **contenedores** empaquetan una aplicación y sus dependencias compartiendo el **kernel del SO host**.
A diferencia de una VM, **no necesitan un SO completo**.

### 4.2. Docker: Arquitectura

- **Docker Engine:** gestiona los contenedores.
- **Imagen (Image):** plantilla de solo lectura (código, librerías, configuración).
- **Contenedor:** instancia en ejecución de una imagen.
- **Dockerfile:** define cómo construir una imagen.
- **Docker Hub/Registry:** repositorio de imágenes.

### 4.3. VM vs. Contenedor

**Máquina Virtual (VM):**
- **Tamaño:** Gigabytes → incluye el SO completo.
- **Arranque:** Minutos.
- **Aislamiento:** Completo → hardware virtualizado.
- **Portabilidad:** Portable entre hipervisores.
- **Rendimiento:** Overhead del hipervisor.
- **Seguridad:** Mayor aislamiento.

**Contenedor Docker:**
- **Tamaño:** Megabytes → solo aplicación y dependencias.
- **Arranque:** Segundos/milisegundos.
- **Aislamiento:** A nivel de proceso → comparte el kernel.
- **Portabilidad:** Portable entre cualquier host con Docker.
- **Rendimiento:** Cercano al nativo → sin overhead de un SO completo.
- **Seguridad:** Menor aislamiento → comparte el kernel.

### 4.4. Ventajas de Docker

- **Portabilidad:** "Build once, run anywhere" Funciona igual en desarrollo, pruebas y producción.
- **Ligereza:** bajo consumo y rápido arranque.
- **Reproducibilidad:** Dockerfile define el entorno.
- **DevOps / CI-CD:** integración con despliegues automatizados.

### 4.5. Limitaciones de Docker

- **Seguridad:** comparte kernel con el host. Superficie de ataque mayor. Un escape de contenedor podría comprometer el host.
- **Datos persistentes:** requiere volúmenes y configuración adicional.
- **Escalabilidad:** muchos contenedores requieren una plataforma de **orquestación**.

Escala pequeña (Docker Compose):** Orquesta multicontenedores (ej. App + BBDD) en un único host mediante un fichero de configuración YAML.

## 5. Orquestación de Contenedores: Kubernetes

### 5.1. Concepto

**Kubernetes (K8s)** es una plataforma open source para **orquestar contenedores**, automatizando su despliegue, escalado y gestión.

### 5.2. Arquitectura

- **Pod:** unidad mínima de despliegue; contiene uno o varios contenedores.
- **Node:** máquina física o virtual que ejecuta Pods.
- **Cluster:** conjunto de Nodes.
- **Control Plane:** gestiona el clúster.
  - API Server
  - Scheduler
  - Controller Manager
  - etcd
- **kubelet:** agente que ejecuta y monitoriza Pods en cada Node.
- **Service:** proporciona acceso estable (IP fija, DNS) a un conjunto de Pods.

### 5.3. Funcionalidades principales

- **Automatiza el despliegue:** Pone en marcha tus aplicaciones de forma automática sin intervención manual. 
- **Autoescalado (HPA):** aumenta/disminuye réplicas según la carga.
- **Self-healing:** reinicia contenedores y reemplaza Pods fallidos.
- **Rolling updates:** actualizaciones graduales sin interrupción del servicio.
- **Service Discovery:** descubrimiento mediante DNS interno.
- **Balanceo de carga:** distribuye el tráfico de red de forma equilibrada entre los diferentes contenedores.
- **ConfigMaps / Secrets:** gestión de configuración y secretos para inyectar configuración sin modificar las imágenes.
- **PersistentVolumes:** almacenamiento persistente.

### 5.4. Servicios gestionados de Kubernetes

- **Amazon EKS** → AWS.
- **Azure AKS** → Microsoft Azure.
- **Google GKE** → Google Cloud.
- **OpenShift** → Red Hat.

## 6. Conclusión

- **Virtualización →** consolida servidores mediante **VMs**, reduce costes y mejora agilidad.
- **VDI →** virtualiza el **puesto de trabajo**, facilitando el teletrabajo.
- **Docker →** empaqueta aplicaciones en **contenedores ligeros**.
- **Kubernetes →** **orquesta y escala** contenedores.

Estas tecnologías permiten **mejor aprovechamiento de recursos, mayor agilidad, escalabilidad, disponibilidad y resiliencia** en las AAPP.