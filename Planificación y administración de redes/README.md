# Proyecto de Red: Carnicería & Minimarket Internacional 

## Descripción del Proyecto
Este repositorio contiene el diseño y la implementación de la red local para la "Carnicería & Minimarket Internacional", desarrollado como parte de un proyecto intermodular. Simula un escenario laboral real para un minimarket de alta gama especializado en productos cárnicos frescos y de importación.

El objetivo principal de la infraestructura es ofrecer una red escalable y robusta, capaz de garantizar un **99.9% de disponibilidad**. ]Este requisito es crítico para el negocio, ya que asegura el mantenimiento de la cadena de frío y el cobro ininterrumpido en los puntos de venta.

## Características Principales
La arquitectura de red se ha diseñado bajo estándares de Administración de Sistemas, enfocándose en la resiliencia y la seguridad:
* **Alta Disponibilidad:** Implementación de redundancia en la puerta de enlace mediante el protocolo HSRP, evitando puntos únicos de fallo.
* **Segmentación Avanzada (VLANs):** Aislamiento lógico estricto del tráfico para los departamentos de Administración, Ventas, Producción, Sistemas, Gestión y una zona desmilitarizada (DMZ).
* **Direccionamiento Optimizado (VLSM):** Uso de Máscaras de Subred de Longitud Variable sobre el direccionamiento `172.16.0.0/16` para optimizar las IPs según la densidad de cada departamento y limitar los dominios de broadcast.
* **Agregación de Enlaces:** Uso de EtherChannel entre el switch multicapa y el de acceso para sumar ancho de banda y proporcionar redundancia en el cableado físico.
* **Calidad de Servicio (QoS):** Priorización de red para los datáfonos y las balanzas IP, evitando cualquier latencia en el proceso de venta.

## Hardware y Tecnologías Empleadas
El proyecto ha sido diseñado y validado utilizando el simulador **Cisco Packet Tracer**, seleccionando equipos del ecosistema Cisco como estándar de enseñanza.
* **Routers (2x Cisco 2911):** Configurados en clúster HSRP para el enrutamiento perimetral redundante.
* **Switch Multicapa Core (1x Cisco 3560):** Encargado de centralizar las conexiones de servidores y gestionar el enrutamiento de alta velocidad (Inter-VLAN Routing).
* **Switch de Acceso (1x Cisco 2960):** Punto de conexión física de los dispositivos finales, encargado del etiquetado del tráfico VLAN.
* **Servicios Centralizados:**
  * **Servidor Web/DNS** aislado en la DMZ para la web pública (`www.carniceria-internacional.com`).
  * **Servidor de Gestión** encargado de la asignación dinámica de direcciones (DHCP) y de almacenar copias de seguridad vía FTP.
* **Seguridad y Mantenimiento:** Administración remota cifrada exclusivamente mediante SSH v2, estrategia de Backup 3-2-1 y protección eléctrica con sistemas SAI.

## Implementación y Uso
El documento detalla los comandos exactos empleados en la configuración de la topología. Algunas de las configuraciones clave incluyen:
1. **VLANs y Accesos:** Creación de las VLAN y asignación de puertos en modo acceso y troncal.
2. **EtherChannel:** Agrupación de puertos mediante el protocolo LACP (`channel-group 1 mode active`).
3. **SVIs e IP Helper:** Configuración de las Interfaces Virtuales de Switch en el equipo Core para actuar como puerta de enlace de cada subred, y redirección de peticiones DHCP hacia el servidor central mediante el comando `ip helper-address`.
4. **HSRP:** Configuración de las prioridades y la IP virtual compartida en los Routers para establecer el rol de Maestro y Reserva.

## Escalabilidad y Mejoras Futuras
El modelo actual de la red está preparado para soportar el crecimiento de la empresa sin cambiar la arquitectura base, contemplando a corto plazo:
* **Telefonía VoIP:** Despliegue de teléfonos IP utilizando el cableado existente mediante una VLAN de voz.
* **Monitorización IoT:** Instalación de sensores de temperatura y humedad conectados por Wi-Fi en las cámaras frigoríficas con alertas automáticas.
* **Acceso VPN:** Implementación de un túnel seguro para que la gerencia pueda consultar la contabilidad en remoto.

---
* **Autores:** Roy Sánchez Prato | Isaac García
* **Módulo:** Administración de redes
* **Centro/Curso:** 1°ASIR Caja Mágica 
* **Fecha:** 15/05/2026
