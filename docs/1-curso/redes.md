# Planificación y Administración de Redes (PAR)

Conceptos esenciales de redes locales, direccionamiento IP, conmutación y enrutamiento.

---

## 🌐 Direccionamiento IPv4 y Subnetting

- **Clases de direcciones:**
  - Clase A: `10.0.0.0/8` (Privada)
  - Clase B: `172.16.0.0/12` (Privada)
  - Clase C: `192.168.0.0/16` (Privada)
- **Fórmulas de cálculo:**
  - Número de subredes: $2^n$ (donde $n$ = bits prestados de host).
  - Número de hosts útiles: $2^h - 2$ (restando Red y Broadcast).

---

## 🖧 Configuración en Cisco Packet Tracer

```text
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```
