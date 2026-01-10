# Día 5 – DNS, ICMP, MTU y Diagnóstico de Red

## 🎯 Objetivos del día

- Comprender el rol del **DNS** y su relación con el caché en distintas capas.
- Diferenciar claramente **ICMP vs HTTP** según la naturaleza de la consulta.
- Analizar herramientas de diagnóstico de red: `dig`, `ping`, `tracepath`, `curl`.
- Entender **MTU / PMTU** y su impacto en la fragmentación.
- Mejorar el pensamiento de *troubleshooting* por capas (modelo OSI).

---

## 🧠 Resumen conceptual

El protocolo **DNS** es considerado principalmente de **capa de aplicación**, pero en la práctica se integra en múltiples capas debido a la jerarquía de resolución y a los distintos niveles de caché involucrados.

Cuando ocurre un problema de caché DNS, este puede encontrarse en diferentes puntos:

1. **Caché del navegador**
2. **Resolver local del sistema operativo**
   - Archivos como `/etc/hosts`
   - Servicios como `systemd-resolved`
3. **Router / Gateway (ISP)**
4. **Servidores DNS autoritativos**

El problema principal del caché ocurre cuando hay un **cambio en la relación dominio → IP**, pero algún nivel sigue entregando información desactualizada.  
Esto puede resolverse de dos formas:

- 🔧 **Intervención manual** (flush de caché, edición de archivos locales)
- ⏳ **Esperar el TTL**, hasta que los servidores actualicen la entrada

---

## 📝 Tarea 1 – DNS, IP y resolución de nombres

Una dirección **IP** es el identificador lógico que permite ubicar dispositivos dentro de una red local o global.  
IPv4 está compuesta por **4 octetos numéricos**, lo cual no es amigable para los humanos.

Para resolver este problema existe **DNS (Domain Name System)**, que permite asociar nombres legibles como `google.com` a direcciones IP.

El **resolver local** gestiona el caché del sistema y decide si:
- Ya posee la respuesta
- O debe consultar a servidores externos

DNS se cataloga como protocolo de **capa de aplicación**, aunque interactúa con:
- Transporte (UDP/TCP 53)
- Red (enrutamiento)
- Sistema operativo y hardware

---

## 📝 Tarea 2 – ICMP, HTTP y naturaleza de las consultas

### ICMP (ping)

- Protocolo de **capa de red**
- No utiliza puertos
- No establece sesiones
- Se usa para **diagnóstico de conectividad**
- Evalúa:
  - Alcance
  - Latencia
  - Pérdida de paquetes

`ping` realiza una resolución DNS previa si se usa un dominio, pero el tráfico principal es **ICMP Echo Request / Reply**.

---

### HTTP / HTTPS (curl)

- Protocolo de **capa de aplicación**
- Usa puertos (80 / 443)
- Solicita **recursos**, no conectividad
- Devuelve **respuestas HTTP** como:
  - `200 OK`
  - `301 Moved Permanently`

Una respuesta `301` indica que el recurso fue movido, lo cual explica por qué:
- `curl google.com` devuelve un 301
- El navegador redirige automáticamente a `https://www.google.com`

---

## 📝 Tarea 3 – Comandos y herramientas

### `dig`
- Herramienta avanzada de consulta DNS
- Protocolo de capa de aplicación
- Usa UDP/TCP puerto 53


## Extended Notes (Notion)
Additional theory, mental models, and personal annotations are documented in Notion:
👉 https://www.notion.so/D-a-5-exposici-n-cach-y-l-mites-de-la-red-2e2e10fabb2880cebce9e89a9995541f