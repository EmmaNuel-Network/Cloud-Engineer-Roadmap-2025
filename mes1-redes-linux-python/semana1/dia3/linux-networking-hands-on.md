# Reporte Técnico de Red: Diagnóstico y Análisis de Infraestructura (Día 3)

## 1. Identificación y Estado de Interfaces
A través del comando `ip addr`, se realizó la auditoría de las interfaces de red presentes en el host de la Virtual Machine (Ubuntu Devnet).

| Interfaz | Dirección IP | Máscara | Estado | Notas |
| :--- | :--- | :--- | :--- | :--- |
| **lo** | `127.0.0.1` | `/8` | `UP` | Loopback interna. Indica interacción en Capa 2 (`LOWER_UP`). |
| **enp0s3** | `192.168.1.6` | `/24` | `UP` | Interfaz de salida. Configurada en modo **Bridge**. |

> **Nota de Configuración:** La VM opera en modo Bridge, lo que le permite reservar una identidad propia en la red física a través del router, obteniendo una IP dentro del mismo segmento que el host anfitrión.

---

## 2. Enrutamiento y Salida a Internet
El análisis del comando `ip route` permite identificar el flujo de salida de los paquetes:

* **Default Gateway:** `192.168.1.1` (IP estándar de gestión del router).
* **Interfaz de Salida:** Los paquetes destinados a redes externas son direccionados a través de `enp0s3`.

---

## 3. Auditoría de Servicios y Sockets (`ss -tulnp`)
Se realizó un análisis de los sockets abiertos para identificar posibles vectores de ataque o servicios activos.

### Observaciones de TCP
* **Servicios en Escucha:** 6 procesos activos (incluyendo SSH en puerto 22).
* **Métrica Send-Q:** Se observa un valor de `4096`. Este representa el **Backlog** (límite de conexiones pendientes de aceptación), indicando una alta capacidad de respuesta del kernel.

### Observaciones de UDP
* **Estado:** No presentan cola de envío/recepción (`0`), debido a la naturaleza del protocolo sin conexión.
* **Alcance:** Mayoritariamente limitados al loopback o rangos locales, sugiriendo su uso para resolución de nombres interna o servicios de descubrimiento no críticos.

---

## 4. Resolución de Nombres (DNS)
Al consultar `/etc/resolv.conf`, se identificó el servidor:
`nameserver 127.0.0.53`

**Análisis:** Ubuntu utiliza `systemd-resolved`. Este actúa como un intermediario local que escucha peticiones en el protocolo UDP para optimizar la velocidad de resolución antes de consultar a servidores externos.

---

## 5. Herramientas de Diagnóstico de Conectividad

* **Ping:** Utilizado para verificar conectividad básica y medir el Round Trip Time (RTT).
* **Tracepath:** Utilizado para mapear los saltos intermedios (hops) hasta el destino, permitiendo identificar latencias en nodos específicos de la red.

---

## 6. Tabla de Vecinos (ARP Cache)
Mediante el comando `ip neigh`, se identificaron los siguientes nodos en el segmento local:

1.  **Gateway:** Estado `DELAY`. El host está verificando si la conexión sigue activa para refrescar la tabla ARP.
2.  **Host 192.168.1.33:** Estado `STALE`. Corresponde probablemente al host anfitrión. La dirección MAC es conocida, pero no ha habido tráfico reciente.

---
**Elaborado por:** [Tu Nombre/Usuario de GitHub]  
**Fecha:** 28 de diciembre de 2025  
**Contexto:** Formación en Ingeniería de Teleco / Roadmap DevOps