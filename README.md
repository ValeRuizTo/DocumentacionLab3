# DocumentacionLab3

## Miembros: Tomas Barrios Guevara, Valentina Ruiz Torres y Darek Aljuri Martínez


## 1. Introducción
En este laboratorio, se busca reforzar los conocimientos fundamentales sobre redes mediante la implementación de una topología empresarial utilizando Cisco Packet Tracer. Se trabajará con tecnologías LAN y WAN, combinando IPv4 e IPv6 en un entorno dual stack para garantizar la compatibilidad y migración eficiente entre ambos protocolos. Además, se configurarán servicios esenciales como DHCP, DNS y VPN IPsec, asegurando una conexión segura entre las intranets de diferentes ubicaciones.

Se establecerán políticas de acceso mediante listas de control de acceso (ACL) para regular la comunicación en función de protocolos HTTP y HTTPS, y se configurarán VLANs con protocolos de enrutamiento como OSPF o EIGRP para la gestión del tráfico intra e inter VLANs. También se desarrollará e integrará una aplicación de seguimiento remoto de activos mediante sockets, que permitirá monitorizar el rendimiento de los dispositivos en tiempo real. Finalmente, se documentarán las configuraciones realizadas, se evaluará el flujo de datos generado por las aplicaciones de seguimiento, y se garantizará la correcta gestión de red mediante SNMP.

## 2. Topología de red

Esta topología segmenta una empresa multinacional en dos grandes sedes (Bogotá y Madrid) con redes LAN organizadas por VLANs y conectadas mediante una WAN que emplea protocolos dinámicos. La DMZ asegura el acceso seguro a servidores públicos, y la dualidad IPv4/IPv6 permite interoperabilidad y modernización.

### Zona Azul - DMZ (Zona Desmilitarizada):
Aloja servicios públicos expuestos al internet. Esta zona permite el acceso desde el exterior sin comprometer las redes internas.
  * Elementos:
    * Servidor Web: Server-PT (Web_Server): Según la página oficial de Hubspot "Un servidor web es una plataforma computacional que resguarda datos para ser consultados por diversos usuarios. Cuenta con un software que entrega la información solicitada por visitantes o miembros de un grupo de trabajo, a través de un dispositivo conectado a la red." [1].
  
    * Servidor DNS: Server-PT (DNS Server): "Un servidor DNS responde a las siglas Domain Name System, y nos permite relacionar una dirección escrita, a una dirección IP ." [2]. Dependiendo del registro que tengamos configurado y el software que soporte a dicha configuración.
  
    * Servidor Tracker: Server-PT (Tracker Server): Controla y registra eventos o datos específicos
    * Servidor DHCP: Asigna automáticamente direcciones IP a dispositivos dentro de la red. "Su función principal es simplificar la administración de direcciones IP, evitando conflictos y garantizando una conexión eficiente y ordenada."[3]
    * Switch 2960-24TT: SW_DMZ: Sirve para la interconexión de los servidores, segun la pagina oficial de cisco "es uno de los switches Cisco Catalyst de la serie 2960, que admiten voz, video, datos y acceso altamente seguro."[4].
  * Conexiones: Esta zona conecta con el Router R1_BOG para recibir y enviar solicitudes desde internet.


### Zona Amarilla - WAN (Red de Conexión ISP)

Conecta múltiples sedes a través de enlaces de internet y asegura la comunicación entre los routers usando algun protoolo de enrutamiento .
* Elementos:
  * Routers 2811:
    * ISP_NET (Core de los ISP)
    * R1_BOG (Router de la sede Bogotá)
    * R2_ESP (Router de la sede en España)
   
  * Los routers guían y dirigen los datos de red mediante paquetes que contienen varios tipos de datos, como archivos, comunicaciones y transmisiones simples como interacciones web.En este caso se usa el  2811 que "es un router de la serie 2800 de Cisco, diseñado principalmente para pequeñas y medianas empresas. Ofrece una combinación de funcionalidades de red y seguridad" [5]
* Espacio de Direcciones IPv4: 193.0.7.0/24


### Zona Naranja - Intranet Bogotá (BOG)

Segmenta la red local de la sede de Bogotá en varias VLANs para separar a los usuarios según su función.
* VLANs:
  * aa: VLAN Guest (PC1 - PC3)
  * bb: VLAN Internal (PC2 - PC4)
  * cc: VLAN Server/Printer
* Elementos:
  * Switches 2960 (SW1_Intranet, SW3_Intranet): Según la página oficial de Cisco los Switches Cisco Catalyst 2960-24TT forman parte de la serie Cisco Catalyst 2960. Esta serie es una familia de switches de capa 2 diseñados para implementaciones en redes de pequeñas y medianas empresas, además cuentan con "24 y 48 puertos de conectividad Gigabit Ethernet (GbE) de escritorio 10/100/1000 " [6].
  
  * Servidor DHCP para asignación dinámica de IPs
  * Impresoras: Printer0 y Printer1
  * Aplicación Tracker instalada en los PCs conectados

* Conexiones: Usa IPv6 en el espacio 2001:1200:A1X::/48

### Zona Verde - Intranet Madrid (MAD)
Implementa la red local de la sede Madrid con VLANs similares a las de Bogotá, pero para diferentes departamentos y funciones.
* VLANs:
  * ff: VLAN Guest (PC5 - PC8)
  * gg: VLAN Internal
  * hh: VLAN Server/Printer
* Elementos:
  * Switch 2960 y 3560 (multicapa para el enrutamiento local), "Los switches compactos Cisco Catalyst 3560-CX y 2960-CX Series ayudan a optimizar las implementaciones de red. Estos conmutadores administrados Gigabit Ethernet (GbE) y Multigigabit Ethernet (mGig) son ideales para conectividad de datos de alta velocidad" [7]. En esta topologia permite la conexión y el enrutamiento entre VLANs en la sede de Madrid.
  * Impresoras: Printer2 y Printer3
  * PCs con el Dashboard del Tracker
* Conexiones: Espacio IPv6 2001:1200:B2X::/48

## 3. Síntesis de la metodología y resultados de configuración: 
* **Montaje de la topología:**
  
Utilizando el cableado estructurado y los modelos de dispositivos indicados, se recreó la topología de red en Cisco Packet Tracer siguiendo la configuración establecida. En esta red, se utilizaron routers Cisco 2811 en todas las interconexiones principales y en la red WAN del ISP. Cada router está conectado entre sí para simular la interconexión WAN y permitir la conectividad entre las distintas sedes, haciendo uso de los protocolos de enrutamiento EIGRP y OSPF. Los routers se configuraron para manejar direcciones IPv4 e IPv6, asignando espacios de red como **193.0.7.0/24** para Internet y **2001:1200::/48** para enlaces internos y loopbacks.

Para gestionar el tráfico dentro de cada sede, se emplearon switches Cisco 2960 como elementos de acceso. En la sede de Madrid (MAD), se implementó un switch Multilayer 3650 como dispositivo de capa 3, encargado de realizar el enrutamiento entre VLANs locales para optimizar el tráfico y mejorar el rendimiento interno. Se configuraron múltiples VLANs en cada sede, asignando diferentes roles a los dispositivos finales, como VLAN de invitados, internos, servidores e impresoras.

En la zona DMZ, se instalaron servidores esenciales (DNS, web y tracker) para brindar servicios accesibles desde las sedes internas y desde Internet, aislando estos servicios críticos del tráfico de la intranet para mayor seguridad. El servidor DHCP se configuró para proporcionar direcciones IP automáticamente a los dispositivos de la red, mientras que los servidores web y DNS facilitan el acceso a recursos tanto internos como externos.

La topología diseñada asegura una segmentación lógica eficiente mediante VLANs, conectividad global a través de la WAN y servicios críticos disponibles en la DMZ. El uso del switch Multilayer 3650 en Madrid permite además una gestión avanzada del tráfico entre las diferentes VLANs y mejora la eficiencia del enrutamiento en la red interna.

* **Esquema de direccionamiento:**
Se diseñó un esquema de direccionamiento basado en los requerimientos de las VLAN, asignando rangos de direcciones según la cantidad de dispositivos. Se realizó el Subneteo y se construyó una tabla de direccionamiento para toda la topología

* **Asignacion y Verificacion de IPV4-IPV6:**
  ¿Qué método(s) de asignación se debe(n) configurar? ¿En qué terminales se deben configurar los 
servicios requeridos?

* **Configuracion routers:**
Se configuraron los routers Cisco 2811 para soportar el enrutamiento estático y dinámico mediante ***OSPF y EIGRP***, permitiendo la interconexión eficiente de las redes de las sedes y la DMZ. Además, se habilitaron direcciones IPv6 en cada router, tanto en las interfaces internas como en las WANs.

* **Configuracion Switches 2960:**
Los switches de acceso Cisco 2960 se configuraron para soportar VLANs y permitir la conectividad dentro de cada sede. Se definieron las VLANs para separar el tráfico según los roles (invitados, internos, servidores). Además, se habilitó trunking en los puertos hacia los routers para permitir la propagación de las VLANs hacia otras sedes.

* **Configuracion  Multilayer Switch (MLSW) 3650 :**
El switch Multilayer 3650 en Madrid se configuró como dispositivo de capa 3 para gestionar el enrutamiento entre VLANs locales, lo que optimiza el tráfico dentro de la sede. Se habilitó Inter-VLAN routing en este dispositivo, y se configuraron las interfaces SVI para cada VLAN interna.

* **Filtros de paquetes y listas de control de acceso (ACLs):**
  * ***Requerimientos de filtrado de tráfico:***
      * Intranet Bogotá (BOG):
        * Los usuarios internos solo pueden acceder al servidor web mediante HTTPs (puerto 443).
        * Usuarios invitados acceden a la web mediante HTTP (puerto 80), no se les permite usar HTTPs.
      * Intranet Madrid (MAD):
        * Los usuarios internos acceden solo mediante HTTPs (puerto 443).
        * Usuarios invitados solo tienen acceso por HTTP (puerto 80), con el acceso a HTTPs (puerto 443) restringido.
      * Zona DMZ:
        * El servidor web estará disponible en la DMZ y gestionado por un servidor DNS.
        * El nombre de dominio para la página web personalizada tendrá el formato (dvt.net)
        
  * ***Implementación de ACLs (Listas de Control de Acceso):***
  "Una ACL es una serie de comandos del IOS que controlan si un router reenvía o descarta paquetes según la información   que se encuentra en el encabezado del paquete. Las ACL son una de las características del software IOS de Cisco más utilizadas." [8]. Las ACLs se aplicarán en los routers de las sedes y la DMZ para garantizar que el tráfico se filtre de acuerdo con las reglas definidas.
    * Ubicación de las ACLs:
      * En el router de salida de Bogotá (BOG):
        Configurar una ACL que permita:
          * HTTPS (puerto 443) solo para usuarios internos de la VLAN.
          * HTTP (puerto 80) solo para usuarios invitados.
      * En el router de Madrid (MAD):
      Configurar una ACL que:
        * Deniegue HTTPS (puerto 443) para los usuarios invitados.
        * Permita HTTP (puerto 80) solo para los invitados.
        * Permita HTTPS (puerto 443) exclusivamente para usuarios internos.
          
    * Configuración de las ACLs en los routers:
      Ejemplo de configuración para Bogotá (BOG):

                    access-list 100 permit tcp 172.23.17.0 0.0.0.255 any eq 443
                    access-list 100 deny tcp 172.23.27.0 0.0.0.255 any eq 443
                    access-list 100 permit tcp 172.23.27.0 0.0.0.255 any eq 80
                    access-list 100 deny ip any any
       
       Explicación línea por línea:
         * *access-list 101 deny tcp 172.24.17.0 0.0.0.255 any eq 443*
           
           ***Propósito:***
           Bloquea todo el tráfico TCP proveniente de la subred 172.24.17.0/24 (usuarios invitados de Madrid) hacia cualquier destino en el puerto 443 (HTTPS).
       
           ***Función:***
           Esta regla garantiza que los usuarios invitados de Madrid no puedan usar HTTPS para acceder al servidor web (política de seguridad). Solo podrán hacerlo mediante HTTP.


        * *access-list 101 permit tcp 172.24.17.0 0.0.0.255 any eq 80*
          
          ***Propósito:***
          Permite a los usuarios invitados de la subred 172.24.17.0/24 acceder al servidor web mediante el puerto 80 (HTTP).
          ***Función:***
          Esta regla habilita el acceso por HTTP para los invitados, cumpliendo la política que restringe su uso de HTTPS.

        * *access-list 101 permit tcp 172.24.7.0 0.0.0.255 any eq 443*

          ***Propósito:***
        Permite que los usuarios internos (subred 172.24.7.0/24) accedan al servidor web mediante HTTPS (puerto 443).

          ***Función:***
          Esta regla garantiza que los empleados y administrativos de Madrid puedan acceder al servidor por una conexión segura usando HTTPS.

       * *access-list 101 deny ip any any*

            ***Propósito:***
            Bloquea todo el tráfico IP que no haya sido permitido por las reglas anteriores.

            ***Función:***
            Esta línea actúa como una política de denegación implícita al final de la lista. Si un paquete no coincide con ninguna de las reglas anteriores, se deniega por defecto.


    * Aplicación de las ACLs en las interfaces:
          
          * Router Bogotá:

                          interface g0/1
                            ip access-group 100 in

          * Router Madrid:

                          interface g0/1
                            ip access-group 101 in





## 7. Referencias 

 1: HubSpot, "¿Qué es un servidor web?", HubSpot Blog. [En línea]. Disponible en: https://blog.hubspot.es/website/que-es-servidor-web#que. 
 
 2: Geeknetic, "¿Qué es un servidor DNS y para qué sirve?", Geeknetic. [En línea]. Disponible en: https://www.geeknetic.es/Servidor-DNS/que-es-y-para-que-sirve.
 
 3: IT Software, "¿Qué es y para qué sirve un servidor DHCP?", IT Software. [En línea]. Disponible en: https://itsoftware.com.co/content/que-es-sirve-servidor-dhcp/.
 
 4: Cisco, "Catalyst 2960 Series Switches", Cisco. [En línea]. Disponible en: https://www.cisco.com/c/es_mx/support/switches/catalyst-2960-series-switches/series.html.
 
 5: Cisco, "Cisco 2811 Integrated Services Router," Cisco Community, [En línea] Disponible en: https://community.cisco.com/t5/networking-knowledge-base/cisco-2811-integrated-services-router/ta-p/3116259.
 
 6: Cisco, "Cisco Catalyst 2960 Series Switches Data Sheet," Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/products/collateral/switches/catalyst-2960-series-switches/product_data_sheet0900aecd806b0bd8.html
 
 7: SP Digital, "Switch Cisco Gigabit Ethernet Catalyst 3560-CX 8 Port 10/100/1000 Mbps + 2 Port SFP Gestionado", SP Digital. [En línea]. Disponible en: https://www.spdigital.cl/switch-cisco-gigabit-ethernet-catalyst-3560-cx-8-port-101001000mbps-2-port-sfp-gestionado/.
 
 8:  CCNA desde Cero, "Listas de control de acceso (ACL) en routers Cisco", CCNA desde Cero. [En línea]. Disponible en: https://ccnadesdecero.es/listas-control-acceso-acl-router-cisco/#1_%C2%BFQue_es_la_lista_de_control_de_acceso.






