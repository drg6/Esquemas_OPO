# Tema 47.- Identificación y firma electrónica. Algoritmos de cifrado simétricos y asimétricos. Prestación de servicios de certificación públicos y privados. Mecanismos de identificación y firma basados certificados X.500 y basados en datos biométricos.

## 1. Introducción

El Tema 46 estableció el marco jurídico de la identificación y firma electrónica (eIDAS, Ley 6/2020) y la base criptográfica (clave pública/privada). Este tema profundiza en los **algoritmos criptográficos** concretos (simétricos y asimétricos), en los **prestadores de servicios de certificación** (públicos y privados), en los **mecanismos de identificación basados en certificados del directorio X.500** y en los **sistemas biométricos**.

## 2. Algoritmos de Cifrado Simétrico

### 2.1. Concepto

En la criptografía simétrica, emisor y receptor utilizan la **misma clave secreta** (clave concertada) para cifrar y descifrar. La fortaleza reside en la longitud de la clave y en la complejidad del algoritmo.

### 2.2. Principales algoritmos

| Algoritmo | Longitud de clave | Tipo | Estado |
|-----------|------------------|------|--------|
| **DES** | 56 bits | Cifrado por bloques (64 bits) | **Obsoleto** (roto por fuerza bruta en 1999) |
| **3DES (Triple DES)** | 112/168 bits | Tres rondas de DES | **Deprecado** (NIST 2023) |
| **AES** | 128 / 192 / 256 bits | Cifrado por bloques (128 bits) | **Estándar vigente** (NIST, ENS) |
| **ChaCha20** | 256 bits | Cifrado de flujo | Alternativa moderna a AES (usado en TLS 1.3, WireGuard) |

### 2.3. AES (Advanced Encryption Standard)

*   Adoptado por el NIST en 2001 como sucesor de DES.
*   Cifrado por bloques de 128 bits con claves de 128, 192 o 256 bits.
*   Modos de operación:
    *   **CBC (Cipher Block Chaining):** Cada bloque depende del anterior (vector de inicialización IV).
    *   **GCM (Galois/Counter Mode):** Proporciona cifrado **y** autenticación (AEAD). **Recomendado** para TLS e IPSec.
    *   **CTR (Counter):** Convierte el cifrado por bloques en cifrado de flujo.
*   **AES-256-GCM** es el estándar de cifrado exigido por el ENS para categoría Alta.

### 2.4. Ventajas e inconvenientes

| Ventaja | Inconveniente |
|---------|--------------|
| Muy rápido (10.000x más rápido que asimétrico) | Problema de distribución de claves |
| Eficiente para cifrar grandes volúmenes de datos | No proporciona no repudio |
| Implementaciones hardware (AES-NI en procesadores Intel/AMD) | Cada par de comunicantes necesita una clave diferente |

## 3. Algoritmos de Cifrado Asimétrico

### 3.1. Concepto

Cada usuario posee un **par de claves** (pública y privada) matemáticamente relacionadas. Lo cifrado con una clave solo se puede descifrar con la otra.

### 3.2. Principales algoritmos

| Algoritmo | Fundamento matemático | Longitud de clave | Uso |
|-----------|----------------------|-------------------|-----|
| **RSA** | Factorización de números primos grandes | 2048 / 3072 / 4096 bits | Firma digital, intercambio de claves, cifrado |
| **DSA** | Logaritmo discreto | 2048+ bits | Solo firma digital (no cifrado) |
| **ECDSA** | Curvas elípticas (ECC) | 256 / 384 bits | Firma digital (DNIe, TLS) |
| **ECDH** | Curvas elípticas | 256 / 384 bits | Intercambio de claves (Diffie-Hellman sobre curvas elípticas) |
| **EdDSA (Ed25519)** | Curvas de Edwards | 256 bits | Firma digital moderna (SSH, WireGuard) |

### 3.3. RSA

*   Inventado en 1977 por Rivest, Shamir y Adleman.
*   Se basa en la dificultad de factorizar el producto de dos números primos muy grandes.
*   Longitudes mínimas de clave recomendadas: 2048 bits (uso general), 3072 bits (recomendado por NIST post-2030).
*   Más lento que ECC a igual nivel de seguridad, pero universalmente soportado.

### 3.4. Criptografía de Curvas Elípticas (ECC)

*   Proporciona el mismo nivel de seguridad que RSA con claves mucho más cortas:
    *   ECDSA 256 bits ≈ RSA 3072 bits.
*   Mayor rendimiento en dispositivos con recursos limitados (tarjetas inteligentes, DNIe).
*   El **DNIe 3.0** utiliza curvas elípticas para firma y autenticación.

### 3.5. Criptografía post-cuántica

Se anticipa que los ordenadores cuánticos podrán romper RSA y ECC. El NIST ha estandarizado algoritmos resistentes a computación cuántica:
*   **CRYSTALS-Kyber / ML-KEM:** Intercambio de claves.
*   **CRYSTALS-Dilithium / ML-DSA:** Firma digital.

## 4. Cifrado Híbrido

En la práctica, los protocolos combinan ambos tipos de criptografía:

1.  **Asimétrica** para intercambiar de forma segura una clave de sesión.
2.  **Simétrica (AES)** para cifrar el tráfico de datos (velocidad).

Ejemplo en TLS 1.3:
*   **ECDHE** (Diffie-Hellman sobre curvas elípticas) negocia la clave de sesión.
*   **AES-256-GCM** cifra todos los datos de la comunicación.

## 5. Prestadores de Servicios de Certificación

### 5.1. Concepto

Un **Prestador de Servicios de Confianza (TSP — Trust Service Provider)** es una entidad que emite certificados digitales y presta servicios de firma electrónica, sellado de tiempo y entrega electrónica certificada. Los **Prestadores Cualificados (QTSP)** cumplen los requisitos más estrictos del Reglamento eIDAS y están supervisados por el organismo competente de cada Estado miembro.

### 5.2. Prestadores en España

| Prestador | Ámbito | Tipo de certificados |
|-----------|--------|---------------------|
| **FNMT-RCM** | Nacional (público) | Persona física, representante, sede, componente, sello |
| **DNIe — DGP** | Nacional (público) | Autenticación y firma (chip del DNI) |
| **ACCV** | Comunidad Valenciana (público) | Empleado público, ciudadano, sede electrónica |
| **Camerfirma** | Nacional (privado) | Persona jurídica, factura electrónica |
| **DigitalSign / Firmaprofesional** | Nacional (privado) | Certificados profesionales, empleado |

### 5.3. Supervisión

En España, el **Ministerio de Asuntos Económicos y Transformación Digital** (actualmente Ministerio de Transformación Digital) supervisa a los prestadores cualificados y mantiene la **Lista de Confianza (TSL — Trusted Service List)** publicada conforme al eIDAS.

## 6. Mecanismos de Identificación basados en X.500

### 6.1. Directorio X.500

**X.500** es un estándar ITU-T/ISO para servicios de directorio distribuido. Define la estructura jerárquica del directorio y el protocolo de acceso:

*   **DAP (Directory Access Protocol):** Protocolo original, pesado y complejo.
*   **LDAP (Lightweight Directory Access Protocol):** Versión ligera de DAP sobre TCP/IP. Es el estándar de facto para acceso a directorios.

### 6.2. Nombres Distinguidos (DN)

Los certificados X.509 v3 identifican al titular mediante un **Distinguished Name (DN)** conforme a X.500:

```
CN=Juan García López, SERIALNUMBER=12345678Z, OU=Informática, O=Ayuntamiento de Alicante, L=Alicante, ST=Alicante, C=ES
```

| Atributo | Significado |
|----------|-------------|
| CN | Common Name (nombre del titular) |
| SERIALNUMBER | Número de serie / DNI |
| OU | Organizational Unit (departamento) |
| O | Organization (organización) |
| L | Locality (localidad) |
| ST | State (provincia) |
| C | Country (país) |

### 6.3. Active Directory y LDAP

**Active Directory** de Microsoft implementa LDAP como protocolo de acceso al directorio. Los certificados digitales de los usuarios se almacenan en el directorio y se utilizan para autenticación (smart card logon) y firma de documentos.

## 7. Identificación Biométrica

### 7.1. Concepto

La **biometría** permite identificar o autenticar a una persona basándose en sus **características físicas o comportamentales** únicas.

### 7.2. Tipos de biometría

| Tipo | Característica | Dispositivo |
|------|---------------|------------|
| **Huella dactilar** | Patrones de crestas y valles del dedo | Lector de huellas |
| **Reconocimiento facial** | Geometría y rasgos del rostro | Cámara (2D/3D) |
| **Reconocimiento de iris** | Patrón del iris del ojo | Cámara de infrarrojos |
| **Reconocimiento de voz** | Patrón de voz (timbre, frecuencia) | Micrófono |
| **Reconocimiento de firma** | Dinámica de la firma manuscrita (presión, velocidad) | Tableta digitalizadora |
| **Patrón venoso** | Mapa de venas del dedo o la mano | Sensor de infrarrojos |

### 7.3. Proceso de autenticación biométrica

1.  **Registro (Enrollment):** Se captura la muestra biométrica del usuario y se almacena una representación matemática (template) en la base de datos.
2.  **Captura:** Se obtiene una nueva muestra biométrica del usuario.
3.  **Comparación (Matching):** Se compara la muestra capturada contra el template almacenado.
4.  **Decisión:** Si el grado de similitud supera el umbral predefinido, la identidad queda verificada.

### 7.4. Métricas de rendimiento

| Métrica | Descripción |
|---------|-------------|
| **FAR (False Acceptance Rate)** | Probabilidad de aceptar incorrectamente a un impostor |
| **FRR (False Rejection Rate)** | Probabilidad de rechazar incorrectamente al usuario legítimo |
| **EER (Equal Error Rate)** | Punto donde FAR = FRR. A menor EER, mayor precisión del sistema |

### 7.5. Biometría en las AAPP

*   **DNIe 3.0:** Almacena las huellas dactilares y la fotografía facial del titular en el chip.
*   **Control de acceso al CPD:** Lectores biométricos de huella o iris para acceso a salas de servidores.
*   **Consideraciones RGPD:** Los datos biométricos son datos de categoría especial (art. 9 RGPD). Su tratamiento requiere base legal reforzada, evaluación de impacto (EIPD) y medidas de seguridad adicionales.

## 8. Conclusión

Los algoritmos criptográficos — simétricos (AES-256) para el cifrado de datos masivos y asimétricos (RSA, ECDSA) para la firma digital y el intercambio de claves — proporcionan los cimientos técnicos de la identificación y firma electrónica. Los prestadores de servicios de confianza cualificados (FNMT, DNIe) garantizan la vinculación fiable entre la identidad del titular y su clave pública mediante certificados X.509 v3 con nombres distinguidos X.500. La biometría complementa los mecanismos basados en certificados, ofreciendo autenticación inherente al individuo, con especial atención a los requisitos de protección de datos del RGPD. La evolución hacia la criptografía post-cuántica asegurará la vigencia de estos mecanismos ante las amenazas futuras de la computación cuántica.
