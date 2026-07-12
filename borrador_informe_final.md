# BORRADOR:
# Informe Final - Proyecto Integrador: Redes de Computadoras
## HidroNova S.A.

---
> [NOTA AI]
> Este es un borrador tentativo generado con IA, no debe entregarse tal cual. Debe ser revisado, corregido y completado por el grupo.

### 1. Introducción. Marco teórico.
En el presente trabajo se detalla el diseño e implementación de la red de datos para la compañía hidroeléctrica "HidroNova S.A.", la cual cuenta con tres sedes ubicadas en la Ciudad Autónoma de Buenos Aires (CABA), San Salvador de Jujuy y Catamarca. El objetivo principal es proveer conectividad, seguridad, segmentación y servicios esenciales (DNS, Web, DHCP, Correo) según los requerimientos solicitados, garantizando la fiabilidad y el rendimiento de la red.

*(Completar con mayor detalle teórico sobre tecnologías usadas si es necesario)*

### 2. Diseño de capa física (cableado estructurado, conectividad, etc.)
La red está distribuida en tres sedes. Las conexiones entre los edificios se realizan mediante enlaces Gigabit Ethernet punto-a-punto por fibra óptica entre routers. 
El acceso a Internet se realiza mediante un enlace dedicado serial en la sede CABA hacia el ISP (red `205.32.130.0/30`).

Para el edificio de CABA (10 pisos), el vínculo vertical de datos se encuentra galvánicamente aislado mediante enlaces de fibra óptica, lo que evita problemas eléctricos entre diferentes sectores de la red. 

*(Referencia al archivo "Conexiones de cableado y puertos.xlsx" para ver detalles de cableado horizontal, vertical, armarios, etc.)*

### 3. Diseño capa de enlace (Asignación de VLANs, 802.1q, etc.)
Para la sede de CABA, que contiene múltiples departamentos, se implementó una segmentación mediante VLANs bajo el estándar 802.1q. Las sedes de Jujuy y Catamarca utilizan un único segmento de red cada una.

**Distribución de VLANs (CABA):**
| VLAN ID | Nombre de VLAN / Red | Departamentos / Áreas asociadas |
| :---: | :--- | :--- |
| **10** | administracion | Contabilidad, Facturación y Liquidaciones, RRHH, Compras, Atención al Público |
| **20** | logistica | Logística y Transporte, Ingeniería, Desarrollo de Producto, Medio Ambiente y Sostenibilidad |
| **30** | gerencia | Directorio, Gerentes, Marketing, Sala de Reuniones, SUM |
| **40** | sistemas | Sistemas, Centro de Datos |

### 4. Diseño capa de red (despliegue IP, ruteo, NAT etc.)
El bloque principal para CABA es `172.29.0.0/23`. Para el resto de sedes y enlaces, se utilizó el bloque `192.168.145.0/24`.

**Subneteo interno:**
| Sede / VLAN | Red | Máscara | Rango de hosts |
| :--- | :--- | :--- | :--- |
| Sistemas (VLAN 40) | 172.29.0.0 | /24 | 172.29.0.1 - 172.29.0.254 |
| Gerencia (VLAN 30) | 172.29.1.0 | /25 | 172.29.1.1 - 172.29.1.126 |
| Administración (VLAN 10)| 172.29.1.128 | /26 | 172.29.1.129 - 172.29.1.190 |
| Logística (VLAN 20) | 172.29.1.192 | /26 | 172.29.1.193 - 172.29.1.254 |
| Jujuy | 192.168.145.0/26 | /26 | 192.168.145.1 - 192.168.145.62 |
| Catamarca | 192.168.145.64/26 | /26 | 192.168.145.65 - 192.168.145.126 |
| Enlace CABA - Jujuy | 192.168.145.128/30| /30 | 192.168.145.129 - 192.168.145.130 |
| Enlace CABA - Cata. | 192.168.145.132/30| /30 | 192.168.145.133 - 192.168.145.134 |
| Enlace Jujuy - Cata. | 192.168.145.136/30| /30 | 192.168.145.137 - 192.168.145.138 |

**Ruteo y NAT:**
Se implementó ruteo estático en todos los equipos para alcanzar las distintas subredes.
El proceso NAT/PAT se efectúa en la sede CABA para dar salida a Internet utilizando el segmento público `200.45.110.128/25`. 

### 5. Descripción de servicio DHCP
Para proveer configuración IP dinámica a los dispositivos finales (PCs, laptops, smartphones, excepto servidores e impresoras con IP estática), se implementaron múltiples pools de DHCP correspondientes a cada VLAN de CABA y a las redes de Jujuy y Catamarca.
*(Completar con más detalles técnicos de la configuración del DHCP si es necesario)*

### 6. Descripción de servicios de capa de aplicación implementados
* **DNS:** Dominio principal `hidronova.com.ar` administrado en CABA. Subdominio `logistica.hidronova.com.ar` delegado a servidor DNS en Jujuy.
* **Web (HTTP/HTTPS):** Se configuraron múltiples servidores. Un servidor principal en CABA y otro en Jujuy (Logística). Además, dos servidores HTTPS en CABA: uno para el sistema de Gestión Logística y otro para la Intranet de Administración (restringido por firewall local).
* **Correo Electrónico:** Servidor configurado para cuentas `@hidronova.com.ar`, soportando al menos 3 usuarios de distintas redes/sedes.
* **Wireless:** APs configurados con SSID "HN-2026", autenticación WPA2-PSK y encriptación AES en todos los pisos de todos los edificios.

### 7. Emulación
Se emuló la topología en un esquema reducido utilizando el software provisto por la cátedra, implementando todos los servicios mencionados. Se configuró un servidor simulando Internet (www.google.com en `64.223.190.94`) conectado al router principal.
Para análisis de tráfico, se incluyeron sniffers: uno monitoreando la salida a Internet y otros en los enlaces WAN (Jujuy, Catamarca, CABA).

### 8. Conclusiones, implementaciones pendientes, dificultades encontradas
*(A completar por el grupo: Agregar reflexiones sobre el trabajo, qué problemas hubo al configurar Packet Tracer / el emulador, y cómo se resolvieron).*

---

### Anexo A: Tabla de Equipos con IP Estática

| FQDN | FUNCIONES | IP / MASCARA | [VLAN] | SEDE | PISO/AREA | [IP PUBLICA NAT] |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| www.hidronova.com.ar | Web Principal | 172.29.0.3 / 24 | 40 (Sistemas) | CABA | 10º Piso | 200.45.110.129 |
| ns1.hidronova.com.ar | DNS Primario Público | 172.29.0.4 / 24 | 40 (Sistemas) | CABA | 10º Piso | 200.45.110.130 |
| ns2.hidronova.com.ar | DNS Secundario Público | 172.29.0.5 / 24 | 40 (Sistemas) | CABA | 10º Piso | 200.45.110.131 |
| correo.hidronova.com.ar | Servidor Correo | 172.29.0.11 / 24 | 40 (Sistemas) | CABA | 10º Piso | 200.45.110.132 |
| logistica.hidronova.com.ar| Web Logística | 172.29.0.6 / 24 | 40 (Sistemas) | CABA | 10º Piso | 200.45.110.133 |
| intranet.hidronova.com.ar | Web Intranet Admin. | 172.29.0.8 / 24 | 40 (Sistemas) | CABA | 10º Piso | N/A |
| dhcp.hidronova.com.ar | Servidor DHCP | 172.29.0.12 / 24 | 40 (Sistemas) | CABA | 10º Piso | N/A |
| resolver.hidronova.com.ar | DNS Local Resolver | 172.29.0.13 / 24 | 40 (Sistemas) | CABA | 10º Piso | N/A |
| dns1-priv.hidronova.com.ar| DNS Primario Privado | 172.29.0.14 / 24 | 40 (Sistemas) | CABA | 10º Piso | N/A |
| dns2-priv.hidronova.com.ar| DNS Secundario Privado | 172.29.0.15 / 24 | 40 (Sistemas) | CABA | 10º Piso | N/A |

> **Nota:** Existen otros equipos con IP estática que el grupo deberá agregar si corresponde (ej: router interfaces, switches, APs, impresoras de red).

### Anexo B: Tabla de Equipos con IP Dinámica (Pools DHCP)

| Pool DHCP | [VLAN] | SEDE | [IP PUBLICA NAT] |
| :--- | :--- | :--- | :--- |
| Pool_Sistemas | 40 | CABA | PAT sobre IP Pública de NAT (Ej: 200.45.110.x) |
| Pool_Gerencia | 30 | CABA | PAT sobre IP Pública de NAT |
| Pool_Administracion | 10 | CABA | PAT sobre IP Pública de NAT |
| Pool_Logistica | 20 | CABA | PAT sobre IP Pública de NAT |
| Pool_Jujuy | N/A | Jujuy | PAT sobre IP Pública de NAT (Ruteado a CABA) |
| Pool_Catamarca | N/A | Catamarca | PAT sobre IP Pública de NAT (Ruteado a CABA) |

*(Ajustar nombres de pools e IP públicas específicas si se configuró un NAT pool múltiple o sobrecarga a una única IP).*
