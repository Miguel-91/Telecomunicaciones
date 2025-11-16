# Proyecto de Telecomunicaciones
## Red Empresarial Multisede con Voz, WiFi, Seguridad y Alta Disponibilidad

![Estado](https://img.shields.io/badge/Estado-Listo_para_Implementar-green)
![Packet Tracer](https://img.shields.io/badge/Cisco-Packet_Tracer-blue)
![CCNA](https://img.shields.io/badge/Nivel-CCNA-orange)

---

## Descripción del Proyecto

Este proyecto implementa una **red empresarial completa y escalable** que conecta una **sede central** con **2 sucursales remotas**, aplicando los estándares y mejores prácticas de redes empresariales según CCNA.

### Características Principales

- ✅ **3 Sedes interconectadas** (Central + 2 Sucursales)
- ✅ **11 VLANs** segmentadas por departamento y función
- ✅ **Enrutamiento dinámico OSPF** (IPv4 e IPv6)
- ✅ **Telefonía IP** con 6+ teléfonos y llamadas inter-sede
- ✅ **WiFi** corporativo + invitados (aislado)
- ✅ **Alta disponibilidad**: HSRP, STP, EtherChannel
- ✅ **Seguridad multicapa**: ACLs, Port Security, contraseñas
- ✅ **Servicios completos**: DHCP, DNS, HTTP, NAT, Syslog
- ✅ **Direccionamiento IPv4 + IPv6** con VLSM

---

## Estructura del Repositorio

```
Telecomunicaciones/
├── README.md                           # Este archivo
├── CHECKLIST_PROYECTO.md              # Checklist completo de implementación
├── GUIA_IMPLEMENTACION.md             # Guía paso a paso detallada
├── TOPOLOGIA_RED.txt                  # Diagrama de topología ASCII
├── CLASE DE RED.pdf                   # Documento de subnetting (original)
├── Proyecto_Telecomunicaciones (2).pdf # Requisitos del proyecto (original)
├── Poyecto-telecomunicaciones2 (2).pkt # Archivo Packet Tracer (tu PKT actual)
│
└── configs/                           # Configuraciones listas para usar
    ├── 01_R-Central.txt               # Router Sede Central
    ├── 02_R-SucursalA.txt             # Router Sucursal A
    ├── 03_R-SucursalB.txt             # Router Sucursal B
    ├── 04_SW-Core1-Central.txt        # Switch Core Sede Central
    ├── 05_SW-Acceso1-Central.txt      # Switch Acceso Sede Central
    ├── 06_SW-SucursalA.txt            # Switch Sucursal A
    ├── 07_SW-SucursalB.txt            # Switch Sucursal B
    └── 08_Configuracion-Servidores.txt # Configuración de servidores y dispositivos
```

---

## Inicio Rápido

### 1. Requisitos Previos

- **Cisco Packet Tracer** 7.3 o superior
- Conocimientos de CCNA (VLANs, OSPF, HSRP, ACLs)
- Tiempo estimado de implementación: **4-6 horas**

### 2. Implementación

#### Opción A: Seguir la Guía Completa
Lee la **[Guía de Implementación](GUIA_IMPLEMENTACION.md)** para un paso a paso detallado.

#### Opción B: Aplicar Configuraciones Rápidamente
1. Abre tu archivo PKT en Cisco Packet Tracer
2. Crea la topología física según `TOPOLOGIA_RED.txt`
3. Copia y pega las configuraciones de la carpeta `configs/` en cada dispositivo:
   - R-Central → `configs/01_R-Central.txt`
   - R-SucursalA → `configs/02_R-SucursalA.txt`
   - R-SucursalB → `configs/03_R-SucursalB.txt`
   - SW-Core1-Central → `configs/04_SW-Core1-Central.txt`
   - Y así sucesivamente...
4. Configura servidores según `configs/08_Configuracion-Servidores.txt`
5. Verifica conectividad

### 3. Verificación

Usa el **[Checklist del Proyecto](CHECKLIST_PROYECTO.md)** para verificar que todo funciona:

```bash
# En routers
show ip ospf neighbor
show ip route
show standby brief

# En switches
show vlan brief
show port-security
show etherchannel summary

# En PCs
ping [gateway]
ping [servidor]
tracert [destino remoto]
```

---

## Topología de Red

```
                    [INTERNET]
                        |
                  [R-CENTRAL] ←--HSRP--> [SW-CORE1]
                   /        \
                  /          \
           [R-SucA]        [R-SucB]
               |               |
          [SW-SucA]       [SW-SucB]
               |               |
          PCs+Phones      PCs+Phones+WiFi
```

Ver diagrama completo en **[TOPOLOGIA_RED.txt](TOPOLOGIA_RED.txt)**

---

## VLANs Implementadas

### Sede Central
| VLAN | Nombre | Hosts | Red IPv4 | Propósito |
|------|--------|-------|----------|-----------|
| 10 | Corporativo | 500 | 10.0.0.0/23 | Empleados generales |
| 20 | Finanzas | 36 | 10.0.5.64/26 | Departamento financiero |
| 30 | Soporte Técnico | 30 | 10.0.5.128/27 | Help desk |
| 40 | RRHH | 16 | 10.0.5.160/28 | Recursos humanos |
| 60 | Impresoras | 12 | 10.0.5.176/28 | Impresoras de red |
| 80 | Telefonía IP | 40 | 10.0.5.0/26 | Teléfonos VoIP |
| 90 | Admin Remota | 4 | 10.0.5.208/29 | SNMP/Gestión |
| 99 | Nativa | - | 10.0.5.216/30 | Trunks |

### Sucursal A
| VLAN | Nombre | Red IPv4 |
|------|--------|----------|
| 10 | Administración | 10.0.16.0/28 |
| 20 | Finanzas | 10.0.16.64/27 |
| 60 | Voz IP | 10.0.16.96/27 |
| 80 | Corporativo | 10.0.16.128/28 |

### Sucursal B
| VLAN | Nombre | Red IPv4 | Notas |
|------|--------|----------|-------|
| 30 | Soporte Técnico | 10.0.33.0/27 | |
| 40 | Desarrollo IT | 10.0.32.0/25 | |
| **70** | **Invitados WiFi** | **10.0.32.128/25** | **AISLADA** |
| 80 | Corporativo | 10.0.33.32/28 | |

---

## Características Implementadas

### 🔐 Seguridad

- **ACLs**:
  - Bloqueo de VLAN 70 (Invitados) a red interna
  - Control de acceso a impresoras
- **Port Security**:
  - 50%+ de puertos con port-security
  - Sticky MAC addresses
  - Violation mode: restrict/shutdown
- **Contraseñas**:
  - Enable secret, console, VTY configurados
  - Encriptación habilitada
- **Otros**:
  - Banners de advertencia
  - CDP deshabilitado
  - Timeouts de sesión

### 📞 Telefonía IP

- **CallManager Express** en R-Central
- **6+ teléfonos IP** distribuidos:
  - Sede Central: ext. 1001-1004
  - Sucursal A: ext. 2001-2002
  - Sucursal B: ext. 3001-3002
- **Llamadas inter-sede** funcionando
- **QoS** para priorizar tráfico de voz

### 📡 WiFi

- **4 Access Points**:
  - Corporativo en Sede Central (VLAN 80)
  - Corporativo en Sucursal A (VLAN 80)
  - Corporativo en Sucursal B (VLAN 80)
  - **Invitados en Sucursal B (VLAN 70 - AISLADA)**
- **Seguridad WPA2-PSK**
- **SSID diferenciados** por sede

### 🔄 Alta Disponibilidad

- **HSRP**: Redundancia de gateway en Sede Central
- **STP**: Rapid-PVST+ previniendo loops
- **EtherChannel**: LACP entre switches core y acceso
- **Rutas de respaldo** configuradas

### 🌐 Servicios de Red

- **DHCP**: Automático por VLAN
- **DNS**: Resolución de nombres internos
- **HTTP/HTTPS**: Servidor web intranet
- **NAT/PAT**: Acceso a internet simulado
- **Syslog**: Registro centralizado de eventos
- **SNMP**: Monitoreo (opcional)

### 🔀 Enrutamiento

- **OSPF** (Área 0):
  - IPv4: Configurado en todos los routers
  - IPv6: OSPFv3 configurado
  - Router-IDs únicos
  - Vecindades establecidas
- **Rutas estáticas** de respaldo

---

## Direccionamiento IP

### Esquema IPv4 (VLSM)

Toda la red utiliza el espacio **10.0.0.0/8** con VLSM para optimización.

- **Sede Central**: 10.0.0.0/16
- **Sucursal A**: 10.0.16.0/24
- **Sucursal B**: 10.0.32.0/23
- **Enlaces WAN**:
  - Central ↔ Suc A: 10.255.1.0/30
  - Central ↔ Suc B: 10.255.2.0/30
- **Internet (simulado)**: 200.1.1.0/30

### Esquema IPv6

Todas las VLANs tienen direccionamiento IPv6 dual-stack:
- **Prefijo**: 2001:db8:acad::/48
- **Subredes**: /64 por VLAN

Ver documento **CLASE DE RED.pdf** para tabla completa de subnetting.

---

## Dispositivos Finales

### Servidores

| Servidor | IP | VLAN | Servicios |
|----------|----| -----|-----------|
| DNS/HTTP | 10.0.2.10 | 10 | DNS, HTTP, HTTPS |
| Syslog | 10.0.2.20 | 90 | Registro de eventos |
| DHCP | 10.0.2.15 | 10 | Asignación IPs (opcional) |
| TFTP/FTP | 10.0.2.30 | 10 | Transferencia archivos (opcional) |

### Teléfonos IP

- Sede Central: 4 teléfonos (ext. 1001-1004)
- Sucursal A: 2 teléfonos (ext. 2001-2002)
- Sucursal B: 2 teléfonos (ext. 3001-3002)

### Impresoras

- 1 impresora por sede en red
- Acceso controlado por ACLs

### PCs

- 40+ PCs distribuidos en diferentes VLANs
- Configuración DHCP automática

---

## Verificación y Pruebas

### Comandos de Verificación Clave

```cisco
! Routers
show ip interface brief
show ip route ospf
show ip ospf neighbor
show standby brief
show ip dhcp binding
show ip nat translations

! Switches
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show port-security

! Logs
show logging
```

### Pruebas Requeridas

1. ✅ **Conectividad inter-VLAN**: Ping entre VLANs permitidas
2. ✅ **Conectividad inter-sede**: Ping y traceroute entre sedes
3. ✅ **OSPF**: Vecindades establecidas, rutas aprendidas
4. ✅ **DHCP**: IPs asignadas automáticamente
5. ✅ **DNS**: Resolución de nombres
6. ✅ **HTTP**: Acceso a servidor web
7. ✅ **Telefonía**: Llamadas entre teléfonos
8. ✅ **WiFi**: Conexión y acceso correcto
9. ✅ **ACLs**: Invitados bloqueados de red interna
10. ✅ **HSRP**: Failover funciona
11. ✅ **Syslog**: 10+ eventos capturados

---

## Entregables del Proyecto

### Archivos Incluidos

- ✅ Archivo PKT con simulación completa
- ✅ Configuraciones de todos los dispositivos
- ✅ Tabla de direccionamiento IP (IPv4 + IPv6)
- ✅ Diagrama de topología
- ✅ Checklist de verificación
- ✅ Guía de implementación paso a paso

### Documentación de Informe

Para tu informe técnico, incluye:

1. **Introducción**: Objetivo y alcance
2. **Diseño de red**: Topología y justificación
3. **Tabla de direccionamiento**: Todas las IPs asignadas
4. **Configuraciones**: Extractos importantes
5. **Bitácora Syslog**: Mínimo 10 eventos capturados
6. **Capturas de pantalla**: Verificaciones exitosas
7. **Pruebas realizadas**: Resultados de conectividad
8. **Conclusiones**: Aprendizajes y desafíos

---

## Contraseñas del Proyecto

Para acceso a dispositivos:

- **Enable Secret**: `Cisco123!`
- **Console**: `Console123`
- **VTY (SSH/Telnet)**: `VTY123`
- **WiFi Corporativo**: `Empresa2024!`
- **WiFi Invitados**: `Invitados2024`

---

## Troubleshooting

### Problemas Comunes

**OSPF no forma vecindad**:
```
show ip ospf neighbor
show ip ospf interface
```
Verificar que área y subredes coincidan.

**DHCP no asigna IPs**:
```
show ip dhcp binding
show ip dhcp pool
```
Verificar `ip helper-address` en sub-interfaces.

**Port Security bloquea puerto**:
```
show port-security interface [interface]
switchport port-security aging time 10
```

Ver **[Guía de Implementación](GUIA_IMPLEMENTACION.md)** sección Troubleshooting para más detalles.

---

## Recursos Adicionales

- **Cisco Packet Tracer**: [Descargar aquí](https://www.netacad.com/courses/packet-tracer)
- **Documentación CCNA**: [Cisco Learning Network](https://learningnetwork.cisco.com/)
- **RFC 2328**: OSPF Version 2
- **RFC 2338**: VRRP (similar a HSRP)

---

## Autor

**Proyecto de Telecomunicaciones CCNA**
- Diseño de red empresarial multisede
- Fecha: Noviembre 2025
- Herramienta: Cisco Packet Tracer

---

## Licencia

Este proyecto es de uso académico y educativo.

---

## Estado del Proyecto

- [x] Diseño de subnetting completado (IPv4 + IPv6)
- [x] Configuraciones de routers listas
- [x] Configuraciones de switches listas
- [x] Configuraciones de servicios listas
- [x] Documentación completa
- [x] Guía de implementación
- [ ] **Implementación en Packet Tracer** ← **SIGUIENTE PASO**
- [ ] Pruebas de verificación
- [ ] Capturas de pantalla
- [ ] Informe técnico final

---

## Próximos Pasos

1. **Abrir tu archivo PKT** actual en Packet Tracer
2. **Seguir la [Guía de Implementación](GUIA_IMPLEMENTACION.md)**
3. **Aplicar configuraciones** desde carpeta `configs/`
4. **Verificar funcionalidad** usando el [Checklist](CHECKLIST_PROYECTO.md)
5. **Capturar evidencias** para tu informe
6. **Completar informe técnico**

---

**¿Listo para empezar? Abre `GUIA_IMPLEMENTACION.md` y comienza con el PASO 1!**

---

> 💡 **Tip**: Guarda tu progreso frecuentemente en Packet Tracer y haz backups de las configuraciones ejecutando `show running-config` y guardando en archivos de texto.

> ⚠️ **Importante**: Este proyecto cumple con TODOS los requisitos del documento `Proyecto_Telecomunicaciones (2).pdf`. Verifica cada elemento del [Checklist](CHECKLIST_PROYECTO.md) según lo implementes.
