# Checklist del Proyecto de Telecomunicaciones

## ✅ COMPLETADO (Diseño de Subnetting)

- [x] VLANs completas (10, 20, 30, 40, 50, 60, 70, 80, 90, 99, 100)
- [x] Direccionamiento IPv4 con VLSM
- [x] Direccionamiento IPv6 en todas las sedes
- [x] Esquema DHCP definido
- [x] Gateways planificados
- [x] Broadcast calculado

---

## ❌ FALTA IMPLEMENTAR EN PACKET TRACER

### 1. TOPOLOGÍA Y EQUIPOS
- [ ] Sede Central con topología jerárquica (núcleo, distribución, acceso)
- [ ] 2 Sucursales remotas (A y B)
- [ ] Enlaces WAN entre sedes
- [ ] Switches multinivel correctos
- [ ] Routers en cada sede

### 2. ENRUTAMIENTO DINÁMICO (OSPF)
- [ ] OSPF configurado en todos los routers
- [ ] Verificación: `show ip route`
- [ ] Conectividad inter-sede funcional
- [ ] Traceroute entre sedes

### 3. TELEFONÍA IP ⚠️ CRÍTICO
- [ ] Mínimo 2 teléfonos IP por sede (6 total)
- [ ] VLAN 60 configurada para voz
- [ ] `switchport voice vlan 60` en puertos
- [ ] Llamada interna entre sedes funciona
- [ ] CallManager/CME configurado

### 4. WIRELESS ⚠️ CRÍTICO
- [ ] Mínimo 1 Access Point por sede (3 total)
- [ ] SSID "Invitados" en VLAN 70
- [ ] VLAN 70 aislada de red interna
- [ ] Dispositivos inalámbricos conectan

### 5. PERIFÉRICOS
- [ ] 1 impresora por sede en VLAN 90
- [ ] ACLs permiten acceso desde VLANs autorizadas

### 6. SEGURIDAD ⚠️ MUY IMPORTANTE

#### ACLs
- [ ] Bloquear VLAN 70 (Invitados) → red interna
- [ ] Permitir solo HTTP, DNS, DHCP desde invitados
- [ ] Controlar acceso a impresoras VLAN 90

#### Port Security (50% de switches mínimo)
- [ ] `switchport port-security` habilitado
- [ ] Límite de MACs configurado
- [ ] Acción por violación (shutdown/restrict)

#### Contraseñas y Banners
- [ ] `enable secret` en todos los dispositivos
- [ ] Console password configurado
- [ ] VTY password configurado
- [ ] `service password-encryption`
- [ ] `banner motd` en todos los dispositivos
- [ ] `description` en todas las interfaces
- [ ] `no cdp run` donde aplique

### 7. ALTA DISPONIBILIDAD Y REDUNDANCIA

#### STP (Spanning Tree Protocol)
- [ ] STP habilitado en todos los switches
- [ ] Raíz verificada: `show spanning-tree`
- [ ] Port Priority configurado

#### HSRP (Sede Central)
- [ ] Router primario configurado
- [ ] Router secundario configurado
- [ ] IP virtual asignada
- [ ] Prioridad y preempt configurados

#### EtherChannel
- [ ] EtherChannel entre switches troncales
- [ ] LACP o PAgP configurado
- [ ] Mínimo 2 enlaces agregados

### 8. SERVICIOS DE RED ⚠️ CRÍTICO

#### DHCP
- [ ] DHCP por VLAN configurado
- [ ] Pools con rangos correctos
- [ ] Gateway correcto en cada pool
- [ ] DNS apuntando al servidor

#### NAT
- [ ] PAT configurado en router perimetral
- [ ] Inside interfaces definidas
- [ ] Outside interface definida
- [ ] Funciona acceso simulado a internet

#### DNS
- [ ] Servidor DNS funcionando
- [ ] Registros A configurados
- [ ] Resolución desde clientes funciona

#### HTTP/HTTPS
- [ ] Servidor web configurado
- [ ] Página accesible desde navegadores
- [ ] Acceso desde diferentes VLANs

### 9. REGISTRO DE EVENTOS (SYSLOG)
- [ ] Servidor Syslog instalado en la red
- [ ] Routers envían logs: `logging host <IP>`
- [ ] Switches envían logs
- [ ] `logging trap informational` configurado
- [ ] Demostración: apagar interfaz y ver logs
- [ ] Bitácora de 10 eventos documentada

### 10. ADMINISTRACIÓN
- [ ] Hostnames descriptivos (ej: R-Central, SW-A-Acceso)
- [ ] VLAN nativa 99 en todos los trunks
- [ ] Nomenclatura estándar en diagramas
- [ ] Etiquetas claras en conexiones

### 11. OPCIONALES (Puntos Extra)
- [ ] Servidor SMTP configurado
- [ ] Servidor FTP configurado
- [ ] Servidor TFTP configurado
- [ ] SNMP para monitoreo (VLAN 100)
- [ ] QoS para priorizar voz: `mls qos`
- [ ] Automatización CLI (EEM, macros)

---

## 📋 VERIFICACIÓN FINAL

### Conectividad
- [ ] Ping entre VLANs permitidas
- [ ] Ping Sede Central ↔ Sucursal A
- [ ] Ping Sede Central ↔ Sucursal B
- [ ] Ping Sucursal A ↔ Sucursal B
- [ ] Traceroute muestra rutas OSPF

### Servicios Funcionando
- [ ] DHCP asigna IPs automáticamente
- [ ] DNS resuelve nombres
- [ ] HTTP/HTTPS accesible
- [ ] Telefonía IP funciona entre sedes
- [ ] WiFi conecta a VLAN 70

### Seguridad Activa
- [ ] Invitados bloqueados de red interna
- [ ] ACLs aplicadas y funcionando
- [ ] Port Security activo y probado
- [ ] Todas las contraseñas configuradas

### Redundancia Funcionando
- [ ] HSRP hace failover correctamente
- [ ] STP previene loops
- [ ] EtherChannel balancea tráfico

---

## 🎯 PRIORIDADES (Por orden)

1. **CRÍTICO**: Telefonía IP (llamadas entre sedes)
2. **CRÍTICO**: WiFi (Access Points + VLAN 70)
3. **CRÍTICO**: Servicios (DHCP, DNS, HTTP)
4. **MUY IMPORTANTE**: Seguridad (ACLs + Port Security)
5. **MUY IMPORTANTE**: OSPF (enrutamiento dinámico)
6. **IMPORTANTE**: Alta disponibilidad (HSRP + STP + EtherChannel)
7. **IMPORTANTE**: Syslog (registro de eventos)
8. **OPCIONAL**: Servicios adicionales (SMTP, FTP, QoS)

---

## 📝 COMANDOS CLAVE DE VERIFICACIÓN

```cisco
# Verificar VLANs
show vlan brief

# Verificar trunks
show interfaces trunk

# Verificar OSPF
show ip ospf neighbor
show ip route ospf

# Verificar HSRP
show standby brief

# Verificar EtherChannel
show etherchannel summary

# Verificar Port Security
show port-security interface

# Verificar STP
show spanning-tree

# Verificar DHCP
show ip dhcp binding

# Verificar NAT
show ip nat translations
```

---

**Fecha de última actualización**: 2025-11-16
**Estado del proyecto**: Diseño de subnetting completado, falta implementación en PKT
