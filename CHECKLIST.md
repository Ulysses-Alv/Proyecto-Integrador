# Checklist de Implementación - HidroNova S.A.
## Proyecto Integrador Redes de Computadoras 1S2026

**Estado actual del .pkt:** 124 dispositivos, 115 conexiones

---

## FASE 1: INFRAESTRUCTURA FÍSICA Y TOPOLOGÍA

### 1.1 Sede CABA (172.29.0.0/23)
- [ ] **Edificio Principal**
  - [X] Router Core CABA (conectado a switches de piso)
  - [X] Router Borde CABA (conexión por FIBRA a Jujuy y Catamarca)
  - [X] Switches de piso (uno por piso con departamentos)
  - [ ] PCs por departamento (configurados con IP estática o DHCP)
  - [ ] Access Points inalámbricos (configurados con SSID y WPA2)
  - [ ] IP Phones (configurados)
  - [ ] Printers (configurados)

- [ ] **Edificio Secundario**
  - [X] Switches de piso
  - [ ] PCs por departamento
  - [ ] Access Points inalámbricos
  - [ ] IP Phones
  - [ ] Printers

- [ ] **Conexión entre edificios CABA**
  - [X] Fibra óptica configurada entre edificios
  - [ ] Interfaces de fibra configuradas en routers/switches

### 1.2 Sede Jujuy (172.29.2.0/24)
- [X] Router Borde Jujuy
- [X] Switches de piso
- [X] PCs por departamento
- [X] Access Points inalámbricos
- [X] IP Phones
- [X] Printers
- [X] Conexión WAN a CABA (serial)

### 1.3 Sede Catamarca (172.29.3.0/24)
- [ ] Router Borde Catamarca
- [ ] Switches de piso
- [ ] PCs por departamento
- [ ] Access Points inalámbricos
- [ ] IP Phones
- [ ] Printers
- [ ] Conexión WAN a CABA (serial)

### 1.4 Segmento Público / Internet
- [ ] Router Internet (conexión a "ISP")
- [ ] Interfaz serial hacia Internet (205.32.130.0/30)
- [ ] Servidor que simula Google (IP: 64.223.190.94)
- [ ] Switch Google
- [ ] DNS Local Resolver Google (8.8.8.8)
- [ ] PC Google (para pruebas)

---

## FASE 2: DIRECCIONAMIENTO IP Y VLANs

### 2.1 Subneteo y VLANs CABA
- [X] VLAN 10: Administración
- [X] VLAN 20: Logistica
- [X] VLAN 30: Gerencia
- [X] VLAN 40: Sistemas

### 2.2 Subneteo Jujuy

- [ ] Subnets dentro de 172.29.2.0/24

### 2.3 Subneteo Catamarca

- [ ] Subnets dentro de 172.29.3.0/24

### 2.4 Segmentos WAN
- [ ] Serial CABA-Jujuy: 10.200.0.0/30
- [ ] Serial CABA-Catamarca: 10.100.0.0/30
- [ ] Serial Internet: 205.32.130.0/30

### 2.5 Segmentos de Servidores
- [ ] Red de servidores CABA: 172.29.0.0/23
- [ ] Red de servidores Jujuy
- [ ] Red de servidores Catamarca

---

## FASE 3: CONFIGURACIÓN DE SWITCHES

### 3.1 Configuración VLANs en Switches
- [X] Crear todas las VLANs en switches de CABA
- [X] Asignar puertos de acceso a VLANs correspondientes
- [X] Configurar puertos trunk entre switches y routers

### 3.2 Trunking
- [X] Configurar trunk entre Router Core y Switches
- [X] Configurar trunk entre switches interconectados
- [] Verificar que las VLANs pasen por los trunks

### 3.3 Switches de Piso
- [X] Configurar switchport mode access en puertos de PCs
- [X] Configurar switchport access vlan [ID] en cada puerto
- [ ] Configurar puertos de Access Points
- [ ] Configurar puertos de IP Phones
- [ ] Configurar puertos de Printers

---

## FASE 4: CONFIGURACIÓN DE ROUTERS

### 4.1 Router Core CABA
- [X] Configurar subinterfaces para cada VLAN (router-on-a-stick)
- [X] Asignar IPs a subinterfaces (gateways de cada VLAN)
- [X] Configurar encapsulation dot1Q en cada subinterface
- [X] Habilitar interfaces (no shutdown)

### 4.2 Router Borde CABA
- [ ] Configurar interfaz serial hacia Jujuy (10.200.0.1/30)
- [ ] Configurar interfaz serial hacia Catamarca (10.100.0.1/30)
- [ ] Configurar interfaz hacia Router Core
- [ ] Configurar interfaz hacia Internet (205.32.130.1/30)

### 4.3 Router Borde Jujuy
- [ ] Configurar interfaz serial hacia CABA (10.200.0.2/30)
- [ ] Configurar interfaz hacia red local Jujuy
- [ ] Configurar gateway por defecto

### 4.4 Router Borde Catamarca
- [ ] Configurar interfaz serial hacia CABA (10.100.0.2/30)
- [ ] Configurar interfaz hacia red local Catamarca
- [ ] Configurar gateway por defecto

### 4.5 Router Internet
- [ ] Configurar interfaz serial hacia CABA (205.32.130.2/30)
- [ ] Configurar interfaz hacia servidor Google
- [ ] Configurar rutas estáticas hacia redes internas

---

## FASE 5: RUTEO ESTÁTICO

### 5.1 Rutas en Router Core CABA
- [ ] Ruta hacia red Jujuy (172.29.2.0/24) vía Router Borde CABA
- [ ] Ruta hacia red Catamarca (172.29.3.0/24) vía Router Borde CABA
- [ ] Ruta por defecto hacia Internet vía Router Internet

### 5.2 Rutas en Router Borde CABA
- [ ] Rutas hacia todas las VLANs de CABA vía Router Core
- [ ] Ruta hacia Jujuy vía interfaz serial
- [ ] Ruta hacia Catamarca vía interfaz serial
- [ ] Ruta hacia Internet vía Router Internet

### 5.3 Rutas en Router Borde Jujuy
- [ ] Ruta hacia CABA (172.29.0.0/23) vía serial
- [ ] Ruta hacia Catamarca vía CABA
- [ ] Ruta por defecto hacia Internet vía CABA

### 5.4 Rutas en Router Borde Catamarca
- [ ] Ruta hacia CABA (172.29.0.0/23) vía serial
- [ ] Ruta hacia Jujuy vía CABA
- [ ] Ruta por defecto hacia Internet vía CABA

### 5.5 Rutas en Router Internet
- [ ] Ruta hacia CABA (172.29.0.0/23)
- [ ] Ruta hacia Jujuy (172.29.2.0/24)
- [ ] Ruta hacia Catamarca (172.29.3.0/24)

---

## FASE 6: NAT (Network Address Translation)

### 6.1 Configuración NAT en Router Borde CABA
- [X] Configurar interfaz inside (hacia red interna)
- [X] Configurar interfaz outside (hacia Internet)
- [X] Crear ACL para redes internas permitidas
- [X] Configurar NAT overload (PAT) con IP pública
- [ ] Verificar traducción NAT

### 6.2 NAT en Routers de Sede (si aplica)
- [ ] Configurar NAT en Jujuy si tiene salida directa
- [ ] Configurar NAT en Catamarca si tiene salida directa

---

## FASE 7: SERVICIOS DHCP

### 7.1 DHCP Server CABA
- [ ] Configurar servicio DHCP habilitado
- [ ] Crear pool para cada VLAN de CABA
- [ ] Configurar gateway por defecto en cada pool
- [ ] Configurar DNS server en cada pool
- [ ] Configurar rango de IPs disponibles
- [ ] Excluir IPs de servidores y gateways

### 7.2 DHCP Server Jujuy
- [ ] Configurar servicio DHCP
- [ ] Crear pool para VLANs de Jujuy
- [ ] Configurar gateway y DNS

### 7.3 DHCP Server Catamarca
- [ ] Configurar servicio DHCP
- [ ] Crear pool para VLANs de Catamarca
- [ ] Configurar gateway y DNS

### 7.4 DHCP Relay (si es necesario)
- [X] Configurar ip helper-address en interfaces de router CABA
- [ ] Configurar ip helper-address en interfaces de router Jujuy
- [ ] Configurar ip helper-address en interfaces de router Catamarca
- [ ] Verificar que DHCP funcione entre VLANs

---

## FASE 8: SERVICIOS DNS

### 8.1 DNS ROOT Server
- [ ] Configurar IP: 193.0.14.129/24
- [ ] Configurar registros NS para root (.)
- [ ] Configurar registros A para root servers
- [ ] Habilitar servicio DNS

### 8.2 DNS .ar .com.ar Server
- [ ] Configurar IP: 200.108.148.50/24
- [ ] Configurar registros NS para .ar
- [ ] Configurar delegaciones para edu.ar
- [ ] Habilitar servicio DNS

### 8.3 DNS edu.ar Server
- [ ] Configurar IP: 170.210.5.56/16
- [ ] Configurar registros A para servidores educativos
- [ ] Habilitar servicio DNS

### 8.4 DNS Local Resolver CABA
- [ ] Configurar IP: 172.29.0.66/24
- [ ] Configurar root hints (NS . y A records)
- [ ] Configurar forwarders si es necesario
- [ ] Habilitar servicio DNS

### 8.5 DNS Primario CABA
- [ ] Configurar IP: 172.29.0.4/24
- [ ] Crear registros A para servidores web
- [ ] Crear registros A para servidor de correo
- [ ] Crear registros MX para correo
- [ ] Crear registros CNAME si es necesario
- [ ] Habilitar servicio DNS

### 8.6 DNS Secundario CABA
- [ ] Configurar IP: 172.29.0.5/24
- [ ] Configurar como secundario del DNS primario
- [ ] Habilitar servicio DNS

### 8.7 DNS Primario Logística y Transporte
- [ ] Configurar IP: 172.29.0.10/24
- [ ] Crear registros para servidores de L y T
- [ ] Habilitar servicio DNS

### 8.8 DNS Secundario Logística y Transporte
- [ ] Configurar IP: 172.29.0.9/24
- [ ] Configurar como secundario
- [ ] Habilitar servicio DNS

### 8.9 DNS Local Resolver Google
- [ ] Configurar IP: 8.8.8.8/24
- [ ] Configurar root hints
- [ ] Habilitar servicio DNS

---

## FASE 9: SERVIDORES WEB

### 9.1 Web Principal
- [ ] Configurar IP: 172.29.0.3/24
- [ ] Habilitar servicio HTTP
- [ ] Habilitar servicio HTTPS (si es necesario)
- [ ] Crear página index.html
- [ ] Configurar DNS A record (www.hidronova.com.ar)

### 9.2 Web Logística y Transporte
- [ ] Configurar IP: 172.29.0.6/24
- [ ] Habilitar servicio HTTP
- [ ] Crear página index.html
- [ ] Configurar DNS A record

### 9.3 Web Seguro L y T
- [ ] Configurar IP: 172.29.0.7/24
- [ ] Habilitar servicio HTTPS
- [ ] Crear página segura
- [ ] Configurar DNS A record

### 9.4 Web Intranet ADM
- [ ] Configurar IP: 172.29.0.8/24
- [ ] Habilitar servicio HTTP
- [ ] Crear página intranet
- [ ] Configurar DNS A record
- [ ] Configurar firewall para restringir acceso

---

## FASE 10: SERVICIO DE CORREO

### 10.1 Servidor de Correo
- [ ] Configurar IP: 172.29.0.11/24
- [ ] Habilitar servicio SMTP
- [ ] Habilitar servicio POP3
- [ ] Configurar dominio: hidronova.com.ar
- [ ] Crear cuentas de usuario (ej: admin@hidronova.com.ar)
- [ ] Configurar registro MX en DNS
- [ ] Configurar registro A para mail.hidronova.com.ar

---

## FASE 11: SERVICIOS ADICIONALES

### 11.1 Servidor FTP
- [ ] Configurar servicio FTP habilitado
- [ ] Crear usuarios FTP con permisos
- [ ] Configurar directorios

### 11.2 Servidor NTP
- [ ] Configurar servicio NTP habilitado
- [ ] Configurar como servidor de tiempo

### 11.3 Servidor Syslog
- [ ] Configurar servicio Syslog habilitado
- [ ] Configurar dispositivos para enviar logs

---

## FASE 12: CONFIGURACIÓN DE CLIENTES

### 12.1 PCs con IP Estática
- [ ] Configurar IP, máscara, gateway, DNS en cada PC
- [ ] Verificar conectividad

### 12.2 PCs con DHCP
- [ ] Configurar para obtener IP automáticamente
- [ ] Verificar que reciban IP del servidor DHCP
- [ ] Verificar conectividad

### 12.3 Laptops
- [ ] Configurar para DHCP
- [ ] Verificar conectividad inalámbrica

### 12.4 Access Points
- [ ] Configurar SSID
- [ ] Configurar WPA2-PSK key
- [ ] Configurar IP de gestión
- [ ] Verificar conectividad inalámbrica

### 12.5 IP Phones
- [ ] Configurar IPs
- [ ] Verificar conectividad

### 12.6 Printers
- [ ] Configurar IPs
- [ ] Verificar que sean alcanzables desde PCs

---

## FASE 13: SEGURIDAD

### 13.1 ACLs (Access Control Lists)
- [ ] Crear ACLs para restringir acceso entre VLANs
- [ ] Aplicar ACLs en interfaces de router
- [ ] Verificar que las ACLs funcionen correctamente

### 13.2 Firewall en Servidores
- [ ] Configurar reglas de firewall en servidores web
- [ ] Configurar reglas de firewall en servidor de correo
- [ ] Configurar reglas de firewall en otros servidores
- [ ] Permitir solo tráfico necesario (puertos 80, 443, 25, 110, etc.)

### 13.3 Seguridad Inalámbrica
- [ ] Verificar que WPA2 esté configurado en todos los APs
- [ ] Verificar que las keys sean seguras
- [ ] Configurar ocultar SSID si es necesario

### 13.4 Seguridad de Switches
- [ ] Deshabilitar puertos no utilizados
- [ ] Configurar port security si es necesario
- [ ] Configurar storm control

---

## FASE 14: PRUEBAS DE CONECTIVIDAD

### 14.1 Pruebas de Ping
- [ ] Ping entre PCs de la misma VLAN
- [ ] Ping entre PCs de diferentes VLANs (misma sede)
- [ ] Ping entre PCs de diferentes sedes
- [ ] Ping desde PCs internas a Internet (Google)
- [ ] Ping desde PC Google a servidores internos

### 14.2 Pruebas de DNS
- [ ] nslookup www.hidronova.com.ar desde PC interna
- [ ] nslookup mail.hidronova.com.ar
- [ ] nslookup de dominios externos desde PC interna
- [ ] Verificar resolución recursiva

### 14.3 Pruebas de Web
- [ ] Acceder a http://www.hidronova.com.ar desde PC interna
- [ ] Acceder a https://www.hidronova.com.ar (si aplica)
- [ ] Acceder a web de Logística y Transporte
- [ ] Acceder a web segura
- [ ] Acceder a intranet

### 14.4 Pruebas de Correo
- [ ] Enviar correo entre cuentas del mismo dominio
- [ ] Enviar correo a cuenta externa (si es posible)
- [ ] Recibir correo desde cuenta externa

### 14.5 Pruebas de DHCP
- [ ] Verificar que PCs reciban IP automáticamente
- [ ] Verificar que reciban gateway correcto
- [ ] Verificar que reciban DNS correcto
- [ ] Verificar renew/release de IPs

### 14.6 Pruebas de NAT
- [ ] Verificar que tráfico interno se traduzca a IP pública
- [ ] Verificar conectividad a Internet
- [ ] Verificar que no se pueda acceder desde Internet a servidores internos (sin port forwarding)

### 14.7 Pruebas de Ruteo
- [ ] traceroute desde CABA a Jujuy
- [ ] traceroute desde CABA a Catamarca
- [ ] traceroute desde Jujuy a Catamarca
- [ ] traceroute desde red interna a Internet

### 14.8 Pruebas de ACLs
- [ ] Verificar que ACLs bloqueen tráfico no permitido
- [ ] Verificar que ACLs permitan tráfico autorizado

---

## FASE 15: DOCUMENTACIÓN

### 15.1 Tabla de Direccionamiento IP
- [ ] Documentar todas las subnets utilizadas
- [ ] Documentar rangos de IPs para cada VLAN
- [ ] Documentar IPs de gateways
- [ ] Documentar IPs de servidores

### 15.2 Tabla de VLANs
- [ ] Documentar ID de cada VLAN
- [ ] Documentar nombre de cada VLAN
- [ ] Documentar rango de IPs asociado
- [ ] Documentar puertos asignados

### 15.3 Tabla de Ruteo Estático
- [ ] Documentar todas las rutas estáticas configuradas
- [ ] Documentar destino, máscara, siguiente salto
- [ ] Documentar en qué router está configurada

### 15.4 Tabla de DHCP Pools
- [ ] Documentar nombre de cada pool
- [ ] Documentar rango de IPs
- [ ] Documentar gateway y DNS
- [ ] Documentar exclusiones

### 15.5 Tabla de DNS Records
- [ ] Documentar registros A creados
- [ ] Documentar registros MX
- [ ] Documentar registros NS
- [ ] Documentar registros CNAME

### 15.6 Diagrama de Topología
- [ ] Generar diagrama completo de la red
- [ ] Incluir todas las IPs y interfaces
- [ ] Incluir VLANs y trunks
- [ ] Incluir conexiones WAN

### 15.7 Informe Técnico
- [ ] Describir arquitectura de red
- [ ] Describir servicios implementados
- [ ] Describir configuración de seguridad
- [ ] Incluir capturas de pantalla de pruebas
- [ ] Documentar problemas encontrados y soluciones

---

## FASE 16: VALIDACIÓN FINAL

### 16.1 Validación de Consigna
- [ ] Verificar que se cumplan todos los requisitos de la consigna
- [ ] Verificar que todas las sedes estén implementadas
- [ ] Verificar que todos los servicios estén funcionando
- [ ] Verificar que la documentación esté completa

### 16.2 Validación de Funcionalidad
- [ ] Ejecutar todas las pruebas de conectividad
- [ ] Verificar que no haya errores en validación del proyecto
- [ ] Verificar que no haya conflictos de IP
- [ ] Verificar que no haya puertos duplicados

### 16.3 Validación de Documentación
- [ ] Revisar que todas las tablas estén completas
- [ ] Revisar que el diagrama sea claro y completo
- [ ] Revisar que el informe técnico sea coherente
- [ ] Verificar ortografía y formato

### 16.4 Preparación para Entrega
- [ ] Guardar proyecto final
- [ ] Hacer backup del .pkt
- [ ] Exportar documentación a PDF
- [ ] Verificar que todo esté listo para presentar

---

## NOTAS IMPORTANTES

### IPs Críticas a Verificar
- **Router Internet:** 193.0.14.1 (Ethernet1/0), 205.32.130.1 (Serial hacia Internet)
- **DNS ROOT:** 193.0.14.129
- **DNS .ar:** 200.108.148.50
- **DNS edu.ar:** 170.210.5.56
- **DNS Local Resolver CABA:** 172.29.0.66
- **DNS Primario CABA:** 172.29.0.4
- **DNS Secundario CABA:** 172.29.0.5
- **Web Principal:** 172.29.0.3
- **Web L y T:** 172.29.0.6
- **Web Seguro:** 172.29.0.7
- **Web Intranet:** 172.29.0.8
- **Correo:** 172.29.0.11
- **DHCP Server:** 172.29.0.70
- **Google Simulator:** 64.223.190.94
- **DNS Google:** 8.8.8.8

### Segmentos WAN
- **CABA-Jujuy:** 10.200.0.0/30
- **CABA-Catamarca:** 10.100.0.0/30
- **Internet:** 205.32.130.0/30

### Bloques de Red Principales
- **CABA:** 172.29.0.0/23 (510 hosts útiles)
- **Jujuy:** 172.29.2.0/24 (254 hosts útiles)
- **Catamarca:** 172.29.3.0/24 (254 hosts útiles)

---

## ESTADO ACTUAL (A completar durante la auditoría)

### Dispositivos sin Configurar
- [ ] Listar PCs sin IP
- [ ] Listar PCs sin gateway
- [ ] Listar switches sin VLANs configuradas
- [ ] Listar routers sin rutas configuradas

### Dispositivos con Configuración Incorrecta
- [ ] Listar dispositivos con IPs fuera de rango
- [ ] Listar dispositivos con gateway incorrecto
- [ ] Listar dispositivos con DNS incorrecto

### Servicios sin Configurar
- [ ] Listar servicios faltantes
- [ ] Listar servicios mal configurados

### Conectividad Pendiente
- [ ] Listar conexiones faltantes
- [ ] Listar rutas faltantes
- [ ] Listar ACLs faltantes

---

**Última actualización:** [Fecha]
**Responsable:** [Nombre]
