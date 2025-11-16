# Comandos de Verificación
## Proyecto: Red Empresarial Multisede

---

## ROUTERS - Comandos de Verificación

### Interfaces
```cisco
! Ver estado de todas las interfaces
show ip interface brief
show ipv6 interface brief

! Ver configuración detallada de una interfaz
show interfaces GigabitEthernet0/0
show interfaces GigabitEthernet0/0.10

! Ver interfaces activas
show ip interface | include up

! Ver estadísticas de interfaces
show interfaces statistics
```

### OSPF
```cisco
! Ver vecinos OSPF
show ip ospf neighbor
show ipv6 ospf neighbor

! Ver tabla de rutas OSPF
show ip route ospf
show ipv6 route ospf

! Ver todas las rutas
show ip route
show ipv6 route

! Ver información detallada de OSPF
show ip ospf
show ip ospf interface
show ip ospf database

! Verificar configuración OSPF
show ip protocols
```

### HSRP (Alta Disponibilidad)
```cisco
! Ver estado HSRP
show standby
show standby brief

! Ver HSRP por interfaz
show standby GigabitEthernet0/0.10
```

### DHCP
```cisco
! Ver asignaciones DHCP
show ip dhcp binding

! Ver estadísticas DHCP
show ip dhcp server statistics

! Ver pools DHCP configurados
show ip dhcp pool
show ip dhcp pool VLAN10-Corporativo

! Ver conflictos DHCP
show ip dhcp conflict
```

### NAT
```cisco
! Ver traducciones NAT activas
show ip nat translations

! Ver estadísticas NAT
show ip nat statistics

! Ver configuración NAT
show ip nat configuration
```

### ACLs
```cisco
! Ver todas las ACLs
show access-lists
show ip access-lists

! Ver ACL específica
show access-lists BLOQUEAR-INVITADOS

! Ver ACLs aplicadas a interfaces
show ip interface GigabitEthernet0/0.70 | include access list
```

### Telefonía IP (CallManager Express)
```cisco
! Ver teléfonos registrados
show ephone registered
show ephone

! Ver directory numbers (extensiones)
show ephone-dn

! Ver servicios de telefonía
show telephony-service
show telephony-service ephone

! Ver llamadas activas
show voice call summary
```

### Logging y Syslog
```cisco
! Ver logs locales
show logging
show logging | include %

! Ver configuración de logging
show logging | include host

! Ver eventos recientes
show logging last 50
```

### Configuración General
```cisco
! Ver configuración en ejecución
show running-config
show running-config | section interface
show running-config | section router ospf
show running-config | section ip dhcp

! Ver configuración guardada
show startup-config

! Ver versión de IOS
show version

! Ver información del sistema
show clock
show users
```

---

## SWITCHES - Comandos de Verificación

### VLANs
```cisco
! Ver todas las VLANs
show vlan brief
show vlan

! Ver VLAN específica
show vlan id 10

! Ver interfaces por VLAN
show interfaces status
```

### Trunks
```cisco
! Ver puertos trunk
show interfaces trunk

! Ver encapsulación de trunk
show interfaces GigabitEthernet0/1 switchport

! Ver VLANs permitidas en trunks
show interfaces trunk | include allowed
```

### Spanning Tree (STP)
```cisco
! Ver estado de STP
show spanning-tree
show spanning-tree summary

! Ver STP por VLAN
show spanning-tree vlan 10

! Ver root bridge
show spanning-tree root

! Ver prioridades
show spanning-tree bridge

! Ver puertos STP
show spanning-tree interface FastEthernet0/1
show spanning-tree interface FastEthernet0/1 detail
```

### EtherChannel
```cisco
! Ver resumen de EtherChannel
show etherchannel summary

! Ver detalles de EtherChannel
show etherchannel 1 detail
show etherchannel port-channel

! Ver load-balancing
show etherchannel load-balance

! Ver protocolo (LACP/PAgP)
show lacp neighbor
show pagp neighbor
```

### Port Security
```cisco
! Ver port-security en todos los puertos
show port-security

! Ver por interfaz
show port-security interface FastEthernet0/1

! Ver direcciones MAC aprendidas
show port-security address

! Ver violaciones
show port-security interface FastEthernet0/1 | include Violation
```

### Interfaces
```cisco
! Ver estado de interfaces
show ip interface brief
show interfaces status

! Ver configuración de switchport
show interfaces FastEthernet0/1 switchport

! Ver estadísticas de errores
show interfaces counters errors

! Ver interfaces down
show interfaces status | include down
```

### MAC Address Table
```cisco
! Ver tabla de MACs
show mac address-table
show mac address-table dynamic

! Ver MACs por VLAN
show mac address-table vlan 10

! Ver MACs por interfaz
show mac address-table interface FastEthernet0/1
```

### DHCP Snooping (si está configurado)
```cisco
! Ver binding database
show ip dhcp snooping binding

! Ver configuración
show ip dhcp snooping
```

### VLANs en Switch Capa 3 (SVIs)
```cisco
! Ver interfaces VLAN
show ip interface brief | include Vlan
show ipv6 interface brief | include Vlan

! Ver rutas en switch L3
show ip route
```

### SNMP
```cisco
! Ver configuración SNMP
show snmp
show snmp community
show snmp host
```

---

## PCs Y DISPOSITIVOS FINALES

### Windows/PC en Packet Tracer
```
! Ver configuración IP
ipconfig
ipconfig /all

! Renovar DHCP
ipconfig /release
ipconfig /renew

! Limpiar caché DNS
ipconfig /flushdns

! Pruebas de conectividad
ping [IP]
ping www.empresa.local
tracert [IP]
tracert www.empresa.local

! Verificar DNS
nslookg www.empresa.local

! Ver tabla ARP
arp -a

! Ver rutas
route print
```

---

## COMANDOS DE TROUBLESHOOTING

### Debug (¡Usar con cuidado!)
```cisco
! Debug OSPF
debug ip ospf adj
debug ip ospf events

! Debug DHCP
debug ip dhcp server events
debug ip dhcp server packet

! Debug NAT
debug ip nat

! Debug HSRP
debug standby events

! Debug ACLs
debug ip packet [ACL-number] detail

! IMPORTANTE: Siempre desactivar después
undebug all
no debug all
```

### Reiniciar Interfaces
```cisco
! Reiniciar interfaz
configure terminal
interface GigabitEthernet0/0
shutdown
no shutdown
exit
```

### Limpiar Configuraciones
```cisco
! Limpiar tabla NAT
clear ip nat translation *

! Limpiar tabla MAC
clear mac address-table dynamic

! Limpiar contadores de interfaces
clear counters
clear counters GigabitEthernet0/1

! Limpiar logs
clear logging

! Limpiar DHCP bindings
clear ip dhcp binding *
clear ip dhcp binding 10.0.0.100
```

### Recuperar Puerto de Port Security
```cisco
! Ver estado
show port-security interface FastEthernet0/1

! Si está en err-disabled
configure terminal
interface FastEthernet0/1
shutdown
no shutdown
exit

! O globalmente
errdisable recovery cause psecure-violation
errdisable recovery interval 300
```

---

## COMANDOS DE CONFIGURACIÓN RÁPIDA

### Guardar Configuración
```cisco
! Guardar
write memory
copy running-config startup-config

! Ver diferencias
show archive config differences
```

### Backup de Configuración
```cisco
! Copiar a TFTP
copy running-config tftp:
[Seguir prompts]

! Copiar desde TFTP
copy tftp: running-config
```

### Resetear Configuración (¡CUIDADO!)
```cisco
! Borrar configuración guardada
write erase
erase startup-config

! Reiniciar dispositivo
reload
```

---

## SECUENCIA DE VERIFICACIÓN COMPLETA

### 1. Verificar Conectividad Básica (Routers)
```cisco
show ip interface brief
show ipv6 interface brief
show ip route
show ipv6 route
ping [IP-destino]
```

### 2. Verificar OSPF
```cisco
show ip ospf neighbor
show ip route ospf
traceroute [IP-remota]
```

### 3. Verificar VLANs (Switches)
```cisco
show vlan brief
show interfaces trunk
show interfaces status
```

### 4. Verificar DHCP
```cisco
! En router
show ip dhcp binding

! En PC
ipconfig /all
ipconfig /release
ipconfig /renew
```

### 5. Verificar Servicios
```cisco
! DNS
nslookup www.empresa.local

! HTTP
[Abrir navegador] → http://www.empresa.local

! Syslog
show logging
```

### 6. Verificar Seguridad
```cisco
! ACLs
show access-lists
show ip interface [interface] | include access list

! Port Security
show port-security
show port-security address
```

### 7. Verificar Alta Disponibilidad
```cisco
! HSRP
show standby brief

! STP
show spanning-tree summary

! EtherChannel
show etherchannel summary
```

### 8. Verificar Telefonía
```cisco
show ephone registered
show telephony-service

! Hacer llamada de prueba entre extensiones
```

---

## PRUEBAS COMPLETAS DEL PROYECTO

### Checklist de Verificación

```
[ ] 1. Ping desde PC VLAN 10 a gateway (10.0.1.1)
[ ] 2. Ping desde PC VLAN 10 a servidor DNS (10.0.2.10)
[ ] 3. Ping desde Sede Central a Sucursal A
[ ] 4. Ping desde Sede Central a Sucursal B
[ ] 5. Traceroute muestra ruta OSPF correcta
[ ] 6. DHCP asigna IPs automáticamente
[ ] 7. DNS resuelve www.empresa.local
[ ] 8. HTTP accede a servidor web
[ ] 9. Telefonos IP registrados (show ephone registered)
[ ] 10. Llamada exitosa entre sedes
[ ] 11. WiFi conecta y recibe IP por DHCP
[ ] 12. WiFi Invitados NO puede hacer ping a red interna (debe fallar)
[ ] 13. HSRP muestra Active/Standby correctamente
[ ] 14. Failover HSRP funciona (apagar router activo)
[ ] 15. STP sin loops (show spanning-tree)
[ ] 16. EtherChannel con todos los enlaces up
[ ] 17. Port Security bloquea MACs no autorizadas
[ ] 18. Syslog recibe eventos (show logging en servidor)
[ ] 19. NAT traduce IPs correctamente
[ ] 20. IPv6 funciona (ping6 entre sedes)
```

---

## CAPTURA DE EVIDENCIAS PARA INFORME

### Comandos para Capturas de Pantalla

```cisco
! 1. Topología completa
[Captura en modo Physical y Logical en Packet Tracer]

! 2. Tabla de rutas OSPF
show ip route ospf

! 3. Vecinos OSPF
show ip ospf neighbor

! 4. VLANs en switches
show vlan brief

! 5. HSRP status
show standby brief

! 6. EtherChannel
show etherchannel summary

! 7. Port Security
show port-security interface FastEthernet0/1

! 8. Servidor Syslog
[Pestaña Syslog en servidor mostrando eventos]

! 9. Ping exitoso entre sedes
ping 10.0.16.14

! 10. Traceroute
traceroute 10.0.32.126

! 11. DHCP binding
show ip dhcp binding

! 12. ACL bloqueando tráfico
show access-lists BLOQUEAR-INVITADOS

! 13. Teléfonos registrados
show ephone registered

! 14. DNS funcionando
[PC: nslookup www.empresa.local]

! 15. HTTP funcionando
[Navegador mostrando página web]
```

---

## EVENTOS SYSLOG PARA DOCUMENTAR (Mínimo 10)

1. Interface up/down
2. OSPF neighbor up/down
3. Port Security violation
4. Spanning Tree topology change
5. HSRP state change
6. Configuration saved
7. ACL denied packet
8. EtherChannel bundle created/deleted
9. Login success/failure
10. DHCP binding created

---

## NOTAS IMPORTANTES

### Comandos que NO funcionan en Packet Tracer
Algunos comandos avanzados no están soportados en PT:
- `debug` completo (limitado)
- Algunos comandos `show` avanzados
- TFTP real (simulado)
- Algunos protocolos específicos

### Mejores Prácticas
1. Siempre usar `show running-config` antes de cambios importantes
2. Guardar con `write memory` después de cada cambio exitoso
3. Verificar con `show` antes de aplicar `debug`
4. Desactivar `debug` inmediatamente después: `undebug all`
5. Documentar todos los cambios

### Convenciones de este Proyecto
- **Enable Secret**: Cisco123!
- **Console**: Console123
- **VTY**: VTY123
- **Router-IDs**: 1.1.1.1 (Central), 2.2.2.2 (Suc A), 3.3.3.3 (Suc B)
- **VLAN 99**: Nativa en todos los trunks
- **VLAN 70**: AISLADA (Invitados WiFi)

---

**Fecha**: 2025-11-16
**Proyecto**: Red Empresarial Multisede - CCNA
