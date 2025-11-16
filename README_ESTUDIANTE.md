# Proyecto de Telecomunicaciones
## Red Empresarial Multisede

---

## Descripción

Proyecto de red empresarial que conecta una sede central con 2 sucursales remotas.
Implementa VLANs, enrutamiento OSPF, telefonía IP, WiFi y servicios de red.

---

## Estructura del Proyecto

```
Telecomunicaciones/
├── README_ESTUDIANTE.md (este archivo)
├── CONSTRUIR_PKT.md (guía paso a paso)
├── CLASE DE RED.pdf (tabla de subnetting)
├── Proyecto_Telecomunicaciones (2).pdf (requisitos)
│
└── configs_limpias/ (configuraciones listas)
    ├── 01_R-Central.txt
    ├── 02_R-SucursalA.txt
    ├── 03_R-SucursalB.txt
    ├── 04_SW-Core1-Central.txt
    ├── 05_SW-Acceso1-Central.txt
    ├── 06_SW-SucursalA.txt
    ├── 07_SW-SucursalB.txt
    └── 08_Configuracion-Servidores-Dispositivos.txt
```

---

## Características

- 3 sedes interconectadas (Central + 2 Sucursales)
- 11 VLANs segmentadas
- Enrutamiento OSPF (IPv4 + IPv6)
- 6+ teléfonos IP con llamadas inter-sede
- WiFi corporativo + invitados (aislado)
- HSRP, STP, EtherChannel
- ACLs, Port Security
- DHCP, DNS, HTTP, NAT, Syslog

---

## Cómo Construir el Proyecto

1. Abre **CONSTRUIR_PKT.md** para guía paso a paso
2. Abre Cisco Packet Tracer
3. Crea dispositivos según la guía
4. Conecta dispositivos
5. Aplica configuraciones desde carpeta `configs_limpias/`
6. Configura dispositivos finales
7. Verifica conectividad

---

## VLANs Principales

### Sede Central
- VLAN 10: Corporativo (500 hosts)
- VLAN 20: Finanzas (36 hosts)
- VLAN 30: Soporte Técnico (30 hosts)
- VLAN 40: RRHH (16 hosts)
- VLAN 60: Impresoras (12 hosts)
- VLAN 80: Telefonía IP (40 hosts)
- VLAN 90: Admin Remota (4 hosts)

### Sucursal B - IMPORTANTE
- **VLAN 70: Invitados WiFi (AISLADA)**
  - Solo acceso a internet
  - Bloqueada de red interna por ACL

---

## Contraseñas

- Enable: `Cisco123!`
- Console: `Console123`
- VTY: `VTY123`
- WiFi Corporativo: `Empresa2024!`
- WiFi Invitados: `Invitados2024`

---

## Verificación

### En Routers:
```
show ip ospf neighbor
show ip route
show standby brief
show ip dhcp binding
```

### En Switches:
```
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show port-security
```

### En PCs:
```
ping [gateway]
ping [servidor]
tracert [destino]
ipconfig /all
```

---

## Pruebas Clave

1. Ping entre sedes
2. OSPF con vecindades activas
3. DHCP asigna IPs
4. DNS resuelve nombres
5. HTTP accesible
6. Telefonía IP funciona
7. WiFi conecta
8. **WiFi Invitados BLOQUEADO de red interna**
9. HSRP failover
10. Syslog recibe eventos

---

## Topología

```
              [INTERNET]
                  |
            [R-CENTRAL]
              /      \
         [R-SucA]  [R-SucB]
            |          |
        [SW-SucA]  [SW-SucB]
```

---

## Servicios

- **DNS**: 10.0.2.10
- **HTTP**: 10.0.2.10
- **Syslog**: 10.0.2.20
- **DHCP**: En routers (helper-address)

---

## Dispositivos por Sede

### Central:
- 1 Router, 2 Switches (Core + Acceso)
- 3-4 Servidores
- 15-20 PCs
- 4 Teléfonos IP
- 1 Impresora
- 1 Access Point

### Sucursal A:
- 1 Router, 1 Switch
- 8-10 PCs
- 2 Teléfonos IP
- 1 Impresora
- 1 Access Point

### Sucursal B:
- 1 Router, 1 Switch
- 12-15 PCs
- 3-5 Laptops (WiFi)
- 2 Teléfonos IP
- 1 Impresora
- 2 Access Points (corporativo + invitados)

---

## Direccionamiento

- Sede Central: 10.0.0.0/16
- Sucursal A: 10.0.16.0/24
- Sucursal B: 10.0.32.0/23
- WAN Central-SucA: 10.255.1.0/30
- WAN Central-SucB: 10.255.2.0/30

Ver **CLASE DE RED.pdf** para tabla completa.

---

## Próximos Pasos

1. Leer **CONSTRUIR_PKT.md**
2. Abrir Cisco Packet Tracer
3. Crear dispositivos
4. Aplicar configuraciones
5. Verificar funcionalidad
6. Capturar evidencias
7. Completar informe

---

**Proyecto desarrollado para curso de Telecomunicaciones**
**Herramienta**: Cisco Packet Tracer 2023
