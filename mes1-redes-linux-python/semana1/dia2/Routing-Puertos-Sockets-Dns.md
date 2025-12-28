Día 2 – Fundamentos de Networking
Routing, Puertos, Sockets y DNS
1. Tabla de Enrutamiento (Routing Table)

Una tabla de enrutamiento es el mecanismo mediante el cual un sistema (host o router) decide cómo encaminar paquetes hacia su destino. Puede definirse como una base de datos dinámica de rutas IP que indica por dónde debe salir un paquete según su dirección de destino.

El sistema selecciona la mejor ruta utilizando algoritmos como Longest Prefix Match (LPM), el cual prioriza la ruta más específica (la que tenga la máscara más larga) para maximizar la eficiencia del enrutamiento.

Es importante aclarar que el kernel del sistema operativo es quien consulta la tabla de enrutamiento y toma la decisión final, no las aplicaciones.

Ruta por defecto (Default Gateway)

La ruta por defecto o Default Gateway es la dirección IP utilizada como último recurso cuando un host intenta enviar un paquete y la IP destino no pertenece a la red local o no existe una ruta más específica en la tabla de enrutamiento.

Esta ruta se representa como:

0.0.0.0/0

Normalmente, el default gateway es el router o módem, ya que este tiene acceso tanto a redes privadas como a Internet (IP pública).
La decisión de usar esta ruta la toma directamente el kernel.

Tipos de rutas

Ruta local
Corresponde a destinos dentro de la misma red física o subred. Permite que los dispositivos se comuniquen entre sí sin pasar por el router.

Ruta remota
Aplica para destinos que no pertenecen a la red local, como redes WAN o Internet. Requiere uno o más routers intermedios.

Ruta por defecto (DF)
Ruta comodín que coincide con cualquier destino no contemplado en las rutas anteriores.
Se representa como 0.0.0.0/0.

2. Puertos y Sockets
Puertos

Un puerto no es un elemento físico. Es un identificador lógico de servicios, implementado como un número de 16 bits, con un rango de:

0 – 65535

El kernel asigna puertos a los servicios para clasificar el tráfico entrante y saliente, permitiendo que múltiples servicios operen simultáneamente sobre la misma dirección IP.

Gracias a los puertos, el sistema sabe qué aplicación debe recibir cada paquete.

Estados de los puertos

Los puertos pueden encontrarse en diferentes estados según la fase de la comunicación TCP. Algunos de los más comunes son:

LISTEN: el servicio está esperando conexiones entrantes.

ESTABLISHED: la conexión ya fue establecida y está activa.

Existen más estados, y todos describen la situación del puerto respecto al proceso de comunicación.

Sockets

Un socket representa la comunicación completa entre dos servicios. No es solo un puerto, sino el canal de comunicación TCP o UDP establecido entre dos extremos.

Un socket se define mediante la 5-tupla:

IP origen

Puerto origen

IP destino

Puerto destino

Protocolo

Conexiones concurrentes

Aunque el número de puertos es limitado, un host puede manejar muchísimas conexiones simultáneas. Esto es posible porque las conexiones no dependen solo del puerto, sino de la 5-tupla completa.

Dado que cada dirección IP es única dentro de la red, el número real de conexiones concurrentes depende principalmente de:

Memoria RAM

Capacidad del kernel

Recursos del sistema operativo

En la práctica, el límite no es el número de puertos, sino la capacidad del sistema.

3. Resolución de Nombres DNS
Archivo /etc/resolv.conf

En sistemas Linux, el directorio /etc almacena las configuraciones del sistema.
Dentro de este directorio, el archivo /etc/resolv.conf es el archivo principal para la resolución de nombres DNS.

En este archivo se define a qué servidores DNS debe consultar el sistema operativo.
Un ejemplo típico de contenido es:

nameserver 8.8.8.8

Resolver local

El resolver local es una biblioteca o proceso del sistema operativo que actúa como intermediario cuando cualquier aplicación solicita resolver un nombre de dominio.

Flujo de resolución:

Una aplicación solicita la resolución DNS

El resolver local consulta primero /etc/hosts (atajos locales)

Si no hay coincidencia, consulta los servidores definidos en /etc/resolv.conf

Obtiene la dirección IP

Entrega la IP a la aplicación solicitante

nslookup vs dig

nslookup
Herramienta más básica, menos detallada y potencialmente imprecisa en algunos escenarios.

dig
Herramienta mucho más detallada y técnica.
Es la preferida por ingenieros, administradores de sistemas y perfiles DevOps.

DNS y el navegador

DNS no depende del navegador.
Cualquier servicio del sistema operativo puede requerir resolución DNS, y siempre es el sistema operativo quien realiza la resolución y entrega la IP a la aplicación.

Sin embargo, existen tecnologías como DoH (DNS over HTTPS), donde algunos navegadores realizan la resolución DNS directamente sobre HTTPS para mejorar la privacidad y evitar el uso del resolver del sistema operativo.

4. Observaciones generales

Este día fue principalmente conceptual, enfocado en comprender:

Cómo se enrutan paquetes en un sistema

Cómo se identifican y gestionan los servicios en red

Cómo funciona realmente la resolución de nombres DNS

Estos conceptos son fundamentales para networking avanzado, DevOps y telecomunicaciones, y constituyen una base sólida para continuar con el roadmap de aprendizaje.

## Extended Notes (Notion)
This section contains deeper explanations, personal reflections and expanded theory related to this lab.

👉 https://www.notion.so/D-a-2-Interfaces-de-redes-2d5e10fabb28806b95f1d151365d3842?source=copy_link
