PARTE (1)

Work: Explicar en propias palabras la interacción en las Capas de Aplicación-Transporte-Internet cuando un CLiente se conecta a un servidor

R//

En este primer día del roadmap estudié a fondo cómo funciona la comunicación cliente–servidor desde las capas inferiores del modelo TCP/IP. El objetivo fue entender el flujo completo que sigue un mensaje cuando viaja desde un cliente hasta un servidor y de vuelta.

🔹 Capa de Red (Internet Layer — IP)

Exploré cómo el Internet Protocol (IP) identifica origen y destino, define rutas óptimas, separa dominios de red y permite el forwarding de paquetes.
También revisé la relación entre direcciones IP, subnets y VPCs como mecanismos para organizar y aislar redes.

🔹 Capa de Transporte (TCP/UDP)

Estudié cómo esta capa establece y administra conexiones.
Revisé:

El funcionamiento del TCP, su confiabilidad y el three-way handshake (SYN → SYN-ACK → ACK).

El rol de los firewalls y balanceadores al manejar conexiones con estado.

Las características del UDP, útil cuando la velocidad es más importante que la confiabilidad.

🔹 Capa de Aplicación (DNS, HTTP, TLS)

Finalmente, analicé cómo las aplicaciones solicitan recursos:

Un dominio es resuelto a una IP mediante DNS.

Se establece una conexión con TCP.

Se levanta un túnel seguro con TLS.

El cliente realiza solicitudes HTTP, dando lugar a HTTPS.

Este resumen representa el cierre del estudio del día, enfocado en comprender el flujo fundamental del networking moderno.