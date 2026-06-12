# DTP-VLAN-Hopping

**Estudiante:**  Juan Francisco Burgos Hiciano

**Matrícula:**  2023-1981

**Asignatura:**  Seguridad en Redes

**Fecha:**  12 Junio 2026

**Link del video**: https://youtu.be/jxBDHxcn1EE


---

### Descripción y Topología del Escenario

El laboratorio fue implementado en GNS3 con el propósito de analizar el funcionamiento del ataque DTP VLAN Hopping en una infraestructura Cisco. La topología está compuesta por un switch Cisco IOU1 conectado a la red física mediante un Cloud (VMnet8). El atacante aprovecha el protocolo Dynamic Trunking Protocol (DTP) para negociar automáticamente un enlace troncal con el switch, obteniendo acceso a múltiples VLAN de la red. Esta configuración permite demostrar cómo una mala implementación de los puertos de acceso puede ser explotada para interceptar tráfico o acceder a segmentos de red que normalmente estarían aislados.

### Detalles de la Topología

**Segmentación de Red:** VLAN 1 (predeterminada).
**Infraestructura:**
* **Switch Cisco IOU L2.**
* **Cloud VMnet8.**
* **Kali Linux conectado a la red de laboratorio.**
**Actores:**
* **Equipo de análisis: Kali Linux (interfaz conectada a VMnet8).**
**Dispositivo analizado:**
* **Switch Cisco IOU L2.**
**Direccionamiento**
* **Red utilizada:** 192.168.140.0/24.
* **Kali Linux:** 192.168.140.132/24.
---

<img width="888" height="650" alt="Image" src="https://github.com/jburgoshiciano-source/Ataque-STP-Claim-Root-Attack-Gns3/blob/5300189ef57af51b281541c3d23d9345d455df25/wwwwwwwww.png" />

### Tabla de Direccionamiento

| Dispositivo | Dirección IP | Máscara de Subred | Gateway Predeterminado |
| :--- | :--- | :--- | :--- |
| **Router Gateway** | 192.168.140.1 | 255.255.255.0 (/24) | N/A |
| **Kali Linux (Atacante)** | 192.168.140.132 | 255.255.255.0 (/24) | 192.168.140.132 |
---

 Requisitos Previos y Herramientas

Para la ejecución exitosa de estos scripts, se requiere el siguiente entorno:

* **Sistema Operativo:** Kali Linux o cualquier distribución Linux compatible.
* **Lenguaje:** Python 3.x.
* **Librerías:** `Scapy` (Instalación: `sudo apt install python3-scapy`).
* Simulador de Red: GNS3.
Dispositivos Simulados:
Switch Cisco IOU L2.
Cloud VMnet8.
Permisos: Acceso de superusuario (root) para la ejecución de herramientas de red y captura de paquetes.

---

 Ataque : DTP VLAN Hopping

 ### Objetivo del Script
El script implementa una práctica de laboratorio orientada al análisis del protocolo Dynamic Trunking Protocol (DTP) y a la demostración del ataque conocido como VLAN Hopping mediante negociación de enlaces troncales. Su funcionamiento consiste en generar y enviar tramas DTP hacia un switch Cisco con el objetivo de negociar automáticamente un enlace trunk, permitiendo observar el comportamiento del protocolo y las vulnerabilidades asociadas a configuraciones inseguras.

La práctica permite comprender cómo DTP facilita la creación dinámica de enlaces troncales entre dispositivos de red y cómo un atacante puede aprovechar esta funcionalidad para obtener acceso a múltiples VLAN sin autorización. Asimismo, facilita el estudio de escenarios donde los puertos de acceso son configurados incorrectamente con negociación automática habilitada, lo que incrementa el riesgo de acceso no autorizado a segmentos de red aislados.

El objetivo principal es reforzar el conocimiento sobre el funcionamiento interno de DTP, analizar los riesgos de seguridad asociados a una configuración inadecuada de los puertos switch y destacar la importancia de implementar medidas de protección como la deshabilitación de DTP mediante el comando switchport nonegotiate, la configuración manual de puertos de acceso y la aplicación de políticas de seguridad de capa 2 para preservar la integridad y segmentación de la infraestructura de red.

### Parámetros Usados
**Interfaz de red:** eth0

**Topología:** Switch Cisco IOU L2, Cloud VMnet8.

**Red:** 192.168.140.0/24

**Protocolo analizado:** Dynamic Trunking Protocol (DTP).

**Herramienta utilizada:** Python 3.x con Scapy.

**Mensajes analizados:** Dynamic Trunking Protocol (DTP) Frames.

**Captura de tráfico:** Almacenamiento opcional de tramas DTP y tráfico VLAN en formato .pcap para su análisis posterior mediante Wireshark, permitiendo verificar el proceso de negociación de enlaces troncales, identificar las tramas DTP intercambiadas y evaluar el comportamiento de la red durante la ejecución del ataque VLAN Hopping.

**Objetivo:** Analizar el intercambio de tramas DTP, identificar el proceso de negociación automática de enlaces troncales (trunk) y observar cómo una configuración insegura puede ser explotada para obtener acceso no autorizado a múltiples VLAN mediante un ataque de VLAN Hopping. Asimismo, evaluar las vulnerabilidades asociadas al uso de DTP y las medidas de mitigación necesarias para proteger la infraestructura de red.

**Resultado esperado:** Visualizar el proceso de negociación de enlaces troncales mediante tramas DTP, verificar si el switch establece una conexión trunk con el dispositivo atacante y comprender cómo una configuración insegura puede permitir el acceso no autorizado a múltiples VLAN. Asimismo, analizar la importancia de las medidas de protección de capa 2, como la deshabilitación de DTP, la configuración manual de puertos de acceso y la correcta segmentación de la red para prevenir ataques de VLAN Hopping.
---

### Medidas de Mitigación

Para reducir el riesgo de ataques VLAN Hopping mediante DTP, se recomienda deshabilitar la negociación automática de enlaces troncales en todos los puertos que no requieran esta funcionalidad. Los puertos destinados a usuarios finales deben configurarse explícitamente en modo acceso y asociarse únicamente a la VLAN correspondiente. Asimismo, es recomendable deshabilitar los puertos no utilizados, implementar mecanismos de seguridad de capa 2 como Port Security y realizar auditorías periódicas de la configuración de los switches. Estas medidas ayudan a evitar que dispositivos no autorizados negocien enlaces trunk y obtengan acceso a múltiples VLAN, preservando la segmentación y seguridad de la infraestructura de red.

```bash
Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate
```
### Beneficios

**Evita que dispositivos no autorizados negocien enlaces troncales (trunk) con el switch.**

**Protege la segmentación de la red al impedir el acceso indebido a múltiples VLAN.**

**Reduce el riesgo de ataques de VLAN Hopping y la interceptación de tráfico entre VLANs.**

**Mantiene la seguridad, estabilidad y disponibilidad de la infraestructura de red.**

**Incrementa la seguridad de la infraestructura de capa 2.**
