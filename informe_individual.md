# Resumen de Aporte Individual — Proyecto Integrador
## Redes de Computadoras

**Nombre:** Ulises J. Alvarenga  
**Grupo:** 13  
**Materia:** Redes de Computadoras  

---

### 1. Resumen del Aporte Individual al Trabajo
* **Configuración de DNS Completo:** Diseñé e implementé la totalidad del servicio de resolución de nombres de la red. Esto incluyó la configuración del servidor DNS primario en la sede de CABA para el dominio `hidronova.com.ar` (configurando los registros SOA, A, NS y CNAME correspondientes) y la delegación del subdominio `logistica.hidronova.com.ar` al servidor DNS primario en la sede de Jujuy.
* **Routing Parcial:** Colaboré en la configuración del ruteo estático en los routers del backbone, estableciendo las rutas necesarias para garantizar la comunicación inter-sede de manera selectiva.
* **Pool DHCP Parcial:** Colaboré en la configuración de los pools de asignación dinámica de direcciones IP en los dispositivos de red correspondientes para abastecer a los clientes dinámicos de los segmentos de red asignados.
* **NAT Parcial:** Colaboré en la implementación de la traducción de direcciones de red (NAT/PAT) en el router de CABA utilizando el bloque público `200.45.110.128/25` provisto.

### 2. Observaciones
* **Complejidad del Ruteo Estático:** La asignación manual de rutas estáticas y el mantenimiento de las mismas.
* **Importancia del Diseño previo:** Planificar de forma anticipada la asignacion de cada IP a cada servidor, tanto privada como pública, los subneteos y VLANS previas facilitó enormemente el TP, junto con una checklist que armamos lo que nos permitió seguirla fácilmente para saber que habia y que no habia implementado. 

### 3. Dificultades Encontradas
* **Problema de Conectividad DHCP en Catamarca:** La principal complicación surgió al intentar que las terminales cliente de la sede de Catamarca obtuvieran direccionamiento dinámico por DHCP. Las PCs no lograban contactar al servidor y no encontrábamos el motivo a simple vista.
* **Uso del Modo Simulación:** Para diagnosticar este fallo, la herramienta de simulación y visualización de paquetes en tiempo real de Cisco Packet Tracer fue crucial. Nos permitió seguir el rastro exacto de los paquetes DHCP (Discover/Offer) y detectar en qué nodo e interfaz se estaban descartando, lo que nos facilitó corregir la configuración del agente relay (`ip helper-address`) y el ruteo de retorno.
