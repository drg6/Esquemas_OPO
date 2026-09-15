# Tema 47.- Identificación y firma electrónica. Algoritmos de cifrado simétricos y asimétricos. Prestación de servicios de certificación públicos y privados. Mecanismos de identificación y firma basados certificados X.500 y basados en datos biométricos.

## 1. Introducción
* **El Objetivo Principal:** Trasladar la seguridad y confianza del mundo físico al entorno digital de las AAPP.
* **El Doble Pilar:** El motor matemático (Criptografía) y la infraestructura (Prestadores de certificación).
* **La Evolución de la Identidad:** Del "lo que el usuario tiene" (Certificados) al "lo que el usuario es" (Biometría).

## 2. Algoritmos de Cifrado Simétrico
### 2.1. Concepto
* Misma clave secreta para cifrar y descifrar. Rápidos para grandes volúmenes.
* *Problema:* Distribución segura de la clave.
### 2.2. Principales algoritmos
* DES / 3DES → Obsoletos / Deprecados.
* **AES** → 128/192/256 bits → Estándar vigente.
* **ChaCha20** → 256 bits → Alternativa ágil en móviles (usado en TLS 1.3, WireGuard).
### 2.3. AES y sus Modos de Operación (Clave ENS)
* Estándar NIST (2001). Cifrado por bloques.
* **GCM (Galois/Counter Mode):** Cifrado + Autenticación. Ideal para túneles de red (IPSec, TLS). **AES-256-GCM** es el estándar para categoría Alta del ENS.
* ⚠️ **XTS (XEX Tweakable Block Cipher):** Modo especializado en cifrado de almacenamiento. Es el utilizado corporativamente por **Windows BitLocker** (AES-XTS 256) para proteger los portátiles ante robos.

## 3. Algoritmos de Cifrado Asimétrico
### 3.1. Concepto
* Par de claves (Pública/Privada) relacionadas matemáticamente. Resuelve el problema de la distribución de claves.
### 3.2. RSA vs. ECC (Curvas Elípticas)
* **RSA:** Factorización de números primos. Claves de 2048/3072 bits. Amplia compatibilidad.
* **ECC (Ej. ECDSA, ECDH):** Logaritmo discreto en curvas elípticas. Mayor eficiencia con menor longitud de clave (ECDSA 256 bits ≈ RSA 3072 bits). Utilizado por el **DNIe 3.0**.
### 3.3. Criptografía Post-Cuántica (PQC)
* Amenaza: La computación cuántica romperá RSA y ECC (Algoritmo de Shor).
* Futuro: Algoritmos NIST como **ML-KEM** (Kyber) para intercambio y **ML-DSA** (Dilithium) para firma.

## 4. Cifrado Híbrido (El mundo real)
* Combina lo mejor de ambos mundos (ej. protocolo **TLS 1.3**):
  1. *Asimétrica (ECDHE):* Para acordar la clave de sesión de forma segura por Internet.
  2. *Simétrica (AES-GCM):* Para cifrar el tráfico rápido y pesado de la conexión.

## 5. Prestadores de Servicios de Certificación
* **5.1. QTSP (Qualified Trust Service Provider):** Entidad bajo el Reglamento eIDAS que emite certificados y da presunción de veracidad legal.
* **5.2. Ecosistema en España:** FNMT-RCM (Física, Sede, Sello), DGP (DNIe), ACCV (Comunidad Valenciana) y Camerfirma.
* **5.3. Supervisión:** Ministerio de Transformación Digital (mantiene la TSL - *Trusted Service List*).

## 6. Identificación basada en X.500
* **6.1. Directorio X.500 y LDAP:** Estándar de directorio jerárquico. LDAP es el protocolo ligero de facto para su consulta.
* **6.2. Distinguished Name (DN):** Los certificados X.509 identifican al usuario mapeando atributos X.500 (CN=Nombre, SERIALNUMBER=DNI, OU=Departamento, O=Ayuntamiento).
* **6.3. Casos de uso (Active Directory):**
  * *Smart Card Logon:* Iniciar sesión en el PC metiendo el DNIe o tarjeta corporativa en el lector.
  * ⚠️ *Integración Biométrica (Windows Hello):* La huella o la cara desbloquean localmente un certificado mapeado contra el LDAP/Active Directory corporativo, eliminando el uso de contraseñas.

## 7. Identificación Biométrica
* **7.1. Concepto y Tipos:** Autenticación por características únicas (Huella, Facial 2D/3D, Iris, Voz, Patrón venoso).
* **7.2. Rendimiento (Las métricas vitales):**
  * **FAR (Falsa Aceptación):** Cuela un impostor (Grave en seguridad lógica).
  * **FRR (Falso Rechazo):** Falla el usuario legítimo (Grave en usabilidad).
  * **EER (Equal Error Rate):** El punto de equilibrio. A menor EER, mejor es el sensor.
* **7.3. Biometría y Legalidad (El freno del RGPD):**
  * El dato biométrico es **Categoría Especial** (Art. 9 RGPD). Exige base legal reforzada y EIPD (Evaluación de Impacto).
  * ⚠️ *Actualidad AAPP:* La AEPD ha restringido drásticamente el uso de biometría para el **control horario (fichaje)** de empleados públicos, obligando a usar alternativas menos intrusivas (tarjetas RFID), limitando la biometría al control de acceso a zonas críticas (ej. el CPD).

## 8. Conclusión
La ciberseguridad administrativa descansa sobre la agilidad de los cifrados simétricos (AES) y la distribución segura de los asimétricos (RSA/ECC). Los prestadores cualificados y los directorios X.500 orquestan las identidades corporativas, mientras que la biometría avanza como método de autenticación robusto, siempre y cuando se equilibre la usabilidad con el estricto cumplimiento del RGPD.

-----------------------------

# Tema 47.- Identificación y firma electrónica. Algoritmos de cifrado simétricos y asimétricos. Prestación de servicios de certificación públicos y privados. Mecanismos de identificación y firma basados certificados X.500 y basados en datos biométricos.

## 1. Introducción

* El Objetivo Principal:
    - Trasladar la seguridad y confianza del mundo físico al entorno digital de las AAPP.

* El Doble Pilar (La Base del Sistema):
    - El motor matemático: La criptografía (blinda y protege la información).
    - La infraestructura: Los prestadores de certificación (otorgan validez legal y presunción de veracidad).

* La Evolución de la Identidad (El Cambio de Paradigma):
    - Del "lo que el usuario tiene" → Elementos lógicos (Certificados electrónicos). 
    - Al "lo que el usuario es" → Factores inherentes (Sistemas biométricos).

## 2. Algoritmos de Cifrado Simétrico

### 2.1. Concepto

- Emisor y receptor utilizan la **misma clave secreta** para cifrar y descifrar.
- Muy rápidos y adecuados para grandes volúmenes de datos.
- Principal problema: **distribución segura de la clave**.

### 2.2. Principales algoritmos

- **DES** → 56 bits → obsoleto.
- **3DES** → 112/168 bits → deprecado.
- **AES** → 128/192/256 bits → **estándar vigente**.
- **ChaCha20** → 256 bits → alternativa moderna a AES (usado en TLS 1.3, WireGuard)

### 2.3. AES (Advanced Encryption Standard)

- Estándar adoptado por NIST(National Institute of Standards and Technology) en 2001.
- Cifrado por bloques de 128 bits.
- Claves: **128, 192 o 256 bits**.
- Modos principales:
  - **CBC (Cipher Block Chaining)** → encadenamiento de bloques.
  - **GCM (Galois/Counter Mode)** → cifrado + autenticación (AEAD). **Recomendado** para TLS e IPSec.
  - **CTR (Counter)** → convierte el cifrado por bloques en flujo.
- **AES-256-GCM** → estándar indicado para categoría Alta del ENS.

* ⚠️ **XTS (XEX Tweakable Block Cipher):** Modo especializado en cifrado de almacenamiento. Es el utilizado corporativamente por **Windows BitLocker** (AES-XTS 256) para proteger los portátiles ante robos.

### 2.4. Ventajas e inconvenientes

- **Ventajas:**
  - Muy rápido.
  - Eficiente para grandes volúmenes.
  - Puede acelerarse mediante hardware (AES-NI).

- **Inconvenientes:**
  - Problema de distribución de claves.
  - No proporciona no repudio.
  - Se necesita una clave diferente para cada par de comunicantes.

## 3. Algoritmos de Cifrado Asimétrico

### 3.1. Concepto

- Utiliza un **par de claves**:
  - **Clave pública** → se puede distribuir.
  - **Clave privada** → debe mantenerse secreta.
- Ambas están matemáticamente relacionadas.
- Permite cifrado, intercambio de claves y firma digital.

### 3.2. Principales algoritmos

- **RSA** → factorización de números primos → firma, cifrado e intercambio de claves.
- **DSA** → logaritmo discreto → firma digital.
- **ECDSA** → curvas elípticas → firma digital (DNIe, TLS)
- **ECDH** → curvas elípticas → intercambio de claves (Diffie-Hellman sobre curvas elípticas)
- **EdDSA (Ed25519)** → curvas de Edwards → firma digital moderna (SSH, WireGuard)

### 3.3. RSA

- Basado en la dificultad de factorizar números grandes.
- Claves habituales: **2048 / 3072 / 4096 bits**.
- 2048 bits → uso general.
- 3072 bits → recomendado para escenarios posteriores a 2030.
- Más lento que ECC, pero ampliamente compatible.

### 3.4. Criptografía de Curvas Elípticas (ECC)

- Permite seguridad equivalente a RSA utilizando claves más pequeñas.
- **ECDSA 256 bits ≈ RSA 3072 bits**.
- Mayor eficiencia en dispositivos con recursos limitados.
- El **DNIe 3.0** utiliza curvas elípticas.

### 3.5. Criptografía post-cuántica

- La computación cuántica podría comprometer **RSA y ECC**.
- Algoritmos resistentes a ataques cuánticos:
  - **ML-KEM (CRYSTALS-Kyber)** → intercambio de claves.
  - **ML-DSA (CRYSTALS-Dilithium)** → firma digital.

## 4. Cifrado Híbrido

- Combina criptografía **asimétrica + simétrica**:
  1. **Asimétrica** → intercambio seguro de la clave de sesión.
  2. **Simétrica (AES)** → cifrado rápido de los datos.
- Ejemplo: **TLS 1.3**
  - **ECDHE (Diffie-Hellman sobre curvas elípticas** → negociación de la clave de sesión.
  - **AES-256-GCM** → cifrado de los datos.

## 5. Prestadores de Servicios de Certificación

### 5.1. Concepto

- **TSP (Trust Service Provider)** → entidad que presta servicios de confianza:
  - Emisión de certificados.
  - Firma electrónica.
  - Sellado de tiempo.
  - Entrega electrónica certificada.
- **QTSP** → prestador cualificado sometido a requisitos y supervisión más estrictos según eIDAS.

### 5.2. Prestadores en España

- **FNMT-RCM** → persona física, representante, sede, componente y sello.
- **DNIe / DGP** → autenticación y firma.
- **ACCV** → empleado público, ciudadano y sede electrónica.
- **Camerfirma** → certificados empresariales y factura electrónica.
- **DigitalSign / Firmaprofesional** → certificados profesionales y de empleado.

### 5.3. Supervisión

- El **Ministerio de Transformación Digital** supervisa a los prestadores cualificados.
- Mantiene la **TSL (Trusted Service List)** conforme al eIDAS.

## 6. Mecanismos de Identificación basados en X.500

### 6.1. Directorio X.500

- **X.500** → estándar ITU-T/ISO para servicios de directorio distribuido.
- Define una estructura jerárquica de directorio.
- Protocolos:
  - **DAP (Directory Access Protocol)** → protocolo original, complejo.
  - **LDAP (Lightweight Directory Access Protocol)** → versión ligera de DAP sobre TCP/IP. Estándar de facto para acceso a directorios.

### 6.2. Nombres Distinguidos (DN)

- Los certificados **X.509 v3** identifican al titular mediante un **Distinguished Name (DN)** basado en X.500.
- Principales atributos:
  - **CN** → nombre del titular.
  - **SERIALNUMBER** → número de serie / DNI.
  - **OU** → unidad organizativa.
  - **O** → organización.
  - **L** → localidad.
  - **ST** → provincia/estado.
  - **C** → país.

### 6.3. Active Directory y LDAP

- **Active Directory** implementa **LDAP** como protocolo de acceso al directorio.
- Los certificados pueden utilizarse para:
  - Autenticación.
  - Smart Card Logon.
  - Firma de documentos.
  
  * ⚠️ *Integración Biométrica (Windows Hello):* La huella o la cara desbloquean localmente un certificado mapeado contra el LDAP/Active Directory corporativo, eliminando el uso de contraseñas.

## 7. Identificación Biométrica

### 7.1. Concepto

- Identificación/autenticación basada en **características físicas o comportamentales** únicas.

### 7.2. Tipos de biometría

- **Huella dactilar** → patrones de crestas y valles (Lector de huellas)
- **Reconocimiento facial** → geometría y rasgos del rostro (Cámara 2D/3D)
- **Iris** → patrón del iris (Cámara de infrarrojos)
- **Voz** → características del patrón de voz (Micrófono)
- **Firma** → dinámica de la firma manuscrita (Tableta digitalizadora)
- **Patrón venoso** → mapa de venas (Sensor de infrarrojos)

### 7.3. Proceso de autenticación biométrica

1. **Registro (Enrollment)** → captura y generación del patrón biométrico (*template*).
2. **Captura** → nueva muestra del usuario.
3. **Comparación (Matching)** → comparación con el template.
4. **Decisión** → se acepta si supera el umbral establecido.

### 7.4. Métricas de rendimiento

- **FAR (False Acceptance Rate)** → aceptación incorrecta de un impostor.
- **FRR (False Rejection Rate)** → rechazo incorrecto de un usuario legítimo.
- **EER (Equal Error Rate)** → punto donde **FAR = FRR**.
- **Menor EER → mayor precisión**.

### 7.5. Biometría en las AAPP

- **DNIe 3.0** → huellas y fotografía facial.
- **Control de acceso a CPD** → huella o iris.
- **RGPD**:
  - Datos biométricos = **categoría especial de datos**.
  - Requieren base legal reforzada.
  - Puede requerirse **EIPD**.
  - Exigen medidas de seguridad adicionales.

La AEPD ha restringido drásticamente el uso de biometría para el **control horario (fichaje)** de empleados públicos, obligando a usar alternativas menos intrusivas (tarjetas RFID) o un pin, limitando la biometría al control de acceso a zonas críticas (ej. el CPD).

**El problema del Fichaje (Centralizado):**
    - La AEPD lo restringe porque las huellas van a una base de datos central.
    - Riesgo: Una brecha de seguridad expone los datos biométricos de toda la plantilla de forma irreversible.

**La solución permitida: Windows Hello for Business (Descentralizado):**
    - Privacidad (Estándar FIDO2): El patrón biométrico NUNCA sale del PC. Se guarda cifrado en el hardware del equipo (chip TPM).
    - Cumplimiento ENS: Actúa como Doble Factor de Autenticación (MFA) transparente: el portátil físico (algo que tienes) + la huella/cara (algo que eres).

## 8. Conclusión

Los algoritmos criptográficos — simétricos (AES-256) para el cifrado de datos masivos y asimétricos (RSA, ECDSA) para la firma digital y el intercambio de claves — proporcionan los cimientos técnicos de la identificación y firma electrónica.  
La biometría ofrece autenticación inherente al individuo, con especial atención a los requisitos de protección de datos del RGPD. 
La evolución hacia la criptografía post-cuántica asegurará la vigencia de estos mecanismos ante las amenazas futuras de la computación cuántica.