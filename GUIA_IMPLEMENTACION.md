# Guía de Implementación Completa
## Proyecto: Red Empresarial Multisede con Voz, WiFi, Seguridad y Alta Disponibilidad

---

## PASO 1: TOPOLOGÍA FÍSICA EN PACKET TRACER

### Dispositivos Necesarios

#### Sede Central:
- **1 Router**: 2811 o 2900 series (R-Central)
- **2 Switches Capa 3**: 3560 (SW-Core1-Central, SW-Core2-Central)
- **2 Switches Capa 2**: 2960 (SW-Acceso1-Central, SW-Acceso2-Central)
- **4 Servidores**: DNS/HTTP, Syslog, DHCP (opcional), TFTP (opcional)
- **1 Access Point**: Access Point-PT
- **6 Teléfonos IP**: IP Phone 7960
- **2 Impresoras**: Printer
- **15-20 PCs**: PC-PT distribuidos en VLANs
- **1 Cloud/Router ISP**: Para simular internet

#### Sucursal A:
- **1 Router**: 2811 (R-SucursalA)
- **1 Switch**: 2960 (SW-SucursalA)
- **1 Access Point**: Access Point-PT
- **2 Teléfonos IP**: IP Phone 7960
- **1 Impresora**: Printer
- **8-10 PCs**: Distribuidos en VLANs

#### Sucursal B:
- **1 Router**: 2811 (R-SucursalB)
- **1 Switch**: 2960 (SW-SucursalB)
- **2 Access Points**: Access Point-PT (1 corporativo, 1 invitados)
- **2 Teléfonos IP**: IP Phone 7960
- **1 Impresora**: Printer
- **12-15 PCs**: Distribuidos en VLANs
- **3-5 Laptops**: Para WiFi invitados

### Conexiones de Red

#### Sede Central:
```
Internet (Cloud) ---[G0/2]--- R-Central ---[G0/0]--- SW-Core1-Central
                               |
                            [G0/1] WAN a Suc A
                               |
                            [S0/0/0] WAN a Suc B

SW-Core1-Central:
  - [G0/1-2] EtherChannel a R-Central
  - [G0/3-4] EtherChannel a SW-Acceso1-Central
  - [G0/5-6] EtherChannel a SW-Acceso2-Central
  - [F0/1] Servidor DNS/HTTP
  - [F0/2] Servidor Syslog
  - [F0/3] Servidor DHCP

SW-Acceso1-Central:
  - [G0/1-2] EtherChannel a SW-Core1
  - [F0/1-10] PCs VLAN 10
  - [F0/11-15] PCs + Teléfonos VLAN 20 + Voice VLAN 80
  - [F0/16-18] PCs VLAN 30
  - [F0/19-21] PCs VLAN 40
  - [F0/22-24] Impresoras VLAN 60
```

#### Sucursal A:
```
R-SucursalA ---[G0/1]--- WAN a Central
            ---[G0/0]--- SW-SucursalA

SW-SucursalA:
  - [G0/1] Trunk a R-SucursalA
  - [F0/1-5] PCs VLAN 10
  - [F0/6-10] PCs + Teléfonos VLAN 20 + Voice VLAN 60
  - [F0/11-15] PCs VLAN 80
  - [F0/20] Access Point VLAN 80
```

#### Sucursal B:
```
R-SucursalB ---[S0/0/0]--- WAN a Central
            ---[G0/0]--- SW-SucursalB

SW-SucursalB:
  - [G0/1] Trunk a R-SucursalB
  - [F0/1-12] PCs VLAN 40
  - [F0/13-18] PCs VLAN 30
  - [F0/19-22] PCs VLAN 80
  - [F0/23] Access Point WiFi Invitados (VLAN 70)
  - [F0/24] Access Point WiFi Corporativo (VLAN 80)
```

---

## PASO 2: APLICAR CONFIGURACIONES

### Orden Recomendado de Configuración:

1. **Routers** (Central → Suc A → Suc B)
2. **Switches Core** (Sede Central)
3. **Switches de Acceso** (Todas las sedes)
4. **Servidores** (DNS, HTTP, Syslog)
5. **Dispositivos finales** (PCs, teléfonos, impresoras, APs)

### Cómo Aplicar las Configuraciones:

#### En cada Router/Switch:
1. Hacer clic en el dispositivo
2. Ir a pestaña **CLI**
3. Presionar Enter para entrar en consola
4. Copiar TODO el contenido del archivo .txt correspondiente
5. Pegar en la consola CLI
6. Esperar a que termine de aplicar (verás el prompt al final)
7. Verificar: `show running-config`

#### Archivos de Configuración por Dispositivo:
- **R-Central**: `configs/01_R-Central.txt`
- **R-SucursalA**: `configs/02_R-SucursalA.txt`
- **R-SucursalB**: `configs/03_R-SucursalB.txt`
- **SW-Core1-Central**: `configs/04_SW-Core1-Central.txt`
- **SW-Acceso1-Central**: `configs/05_SW-Acceso1-Central.txt`
- **SW-SucursalA**: `configs/06_SW-SucursalA.txt`
- **SW-SucursalB**: `configs/07_SW-SucursalB.txt`
- **Servidores**: `configs/08_Configuracion-Servidores.txt`

---

## PASO 3: CONFIGURACIÓN DE DISPOSITIVOS FINALES

### Configuración de PCs:

#### VLAN 10 - Corporativo (Ejemplo):
```
IP Configuration: DHCP
(Debería recibir automáticamente):
  IP: 10.0.0.x
  Mask: 255.255.254.0
  Gateway: 10.0.1.1
  DNS: 10.0.2.10
```

#### VLAN 20 - Finanzas (Ejemplo):
```
IP Configuration: DHCP
(Debería recibir automáticamente):
  IP: 10.0.5.x
  Mask: 255.255.255.192
  Gateway: 10.0.5.65
  DNS: 10.0.2.10
```

**IMPORTANTE**: Repetir para TODAS las VLANs en las 3 sedes.

### Configuración de Teléfonos IP:

1. Conectar teléfono a puerto con `switchport voice vlan` configurado
2. Encender teléfono
3. Configuración automática vía DHCP (Option 150)
4. Verificar que recibe IP de VLAN de voz
5. Verificar que se registra con CallManager (si aplica)

**Ubicaciones recomendadas:**
- **Central**: 2 teléfonos en VLAN 20 (Finanzas), extensiones 1001-1002
- **Sucursal A**: 2 teléfonos en VLAN 20, extensiones 2001-2002
- **Sucursal B**: 2 teléfonos en VLAN 30, extensiones 3001-3002

### Configuración de Access Points:

#### AP Sede Central (Corporativo):
1. Conectar a puerto FastEthernet0/20 de SW-Acceso1 (ya configurado para VLAN 80)
2. En el AP (GUI):
   - Port 1:
     - SSID: `Corporativo-Central`
     - Authentication: WPA2-PSK
     - Passphrase: `Empresa2024!`
     - VLAN: No cambiar (heredará VLAN 80 del switch)
   - IP: DHCP

#### AP Sucursal A (Corporativo):
1. Conectar a puerto FastEthernet0/20 de SW-SucursalA
2. SSID: `Corporativo-SucA`
3. WPA2-PSK: `Empresa2024!`

#### AP Sucursal B - Invitados (VLAN 70 - AISLADA):
1. Conectar a puerto FastEthernet0/23 de SW-SucursalB
2. SSID: `Invitados`
3. Authentication: Open (sin contraseña) o WPA2-PSK: `Invitados2024`
4. IMPORTANTE: Este AP está en VLAN 70 aislada por ACL

#### AP Sucursal B - Corporativo:
1. Conectar a puerto FastEthernet0/24 de SW-SucursalB
2. SSID: `Corporativo-SucB`
3. WPA2-PSK: `Empresa2024!`

### Configuración de Servidores:

Usar configuraciones del archivo `configs/08_Configuracion-Servidores.txt`

#### Servidor DNS/HTTP (10.0.2.10):
1. Conectar a SW-Core1 puerto F0/1 (VLAN 10)
2. Configurar IP estática:
   - IP: 10.0.2.10
   - Mask: 255.255.254.0
   - Gateway: 10.0.1.1
3. Pestaña **Services → DNS**:
   - Activar servicio
   - Agregar registros A según documento
4. Pestaña **Services → HTTP**:
   - Activar servicio
   - Crear página index.html
5. Pestaña **Services → HTTPS**:
   - Activar servicio

#### Servidor Syslog (10.0.2.20):
1. Conectar a SW-Core1 puerto F0/2 (VLAN 90)
2. Configurar IP estática:
   - IP: 10.0.2.20
   - Mask: 255.255.255.248
   - Gateway: 10.0.5.209
3. Pestaña **Services → Syslog**:
   - Activar servicio
4. Monitorear logs recibidos

#### Servidor DHCP (Opcional - si no se usa router):
1. IP: 10.0.2.15
2. Configurar pools según VLANs

### Configuración de Impresoras:

#### Impresora Sede Central (VLAN 60):
1. Conectar a SW-Acceso1 puertos F0/22-24
2. IP: 10.0.5.180
3. Mask: 255.255.255.240
4. Gateway: 10.0.5.177

#### Impresoras Sucursales:
Ver archivo de configuración de servidores para IPs específicas.

---

## PASO 4: VERIFICACIÓN Y PRUEBAS

### 4.1 Verificación de Conectividad Básica

#### En R-Central:
```
show ip interface brief
show ipv6 interface brief
show ip route
show ip ospf neighbor
show standby brief
show ip nat translations
```

#### En SW-Core1-Central:
```
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show standby brief
show ip interface brief
```

#### En SW-Acceso1-Central:
```
show vlan brief
show port-security
show port-security interface fastEthernet 0/1
show spanning-tree
```

### 4.2 Pruebas de Conectividad

#### Desde PC en VLAN 10 (Corporativo):
```
✓ ping 10.0.1.1         (Gateway - R-Central)
✓ ping 10.0.2.10        (Servidor DNS)
✓ ping 10.0.16.14       (R-SucursalA)
✓ ping 10.0.32.126      (R-SucursalB)
✓ ping www.empresa.local (Resolución DNS)
✓ tracert 10.0.16.14    (Ver ruta OSPF)
```

#### Desde PC en VLAN 20 (Finanzas):
```
✓ ping 10.0.5.65        (Gateway local)
✓ ping 10.0.5.180       (Impresora - debe funcionar por ACL)
✓ ping 10.0.0.100       (PC en VLAN 10 - debe funcionar)
```

#### Desde PC en VLAN 70 (Invitados WiFi - Sucursal B):
```
✓ ping 10.0.32.129      (Gateway local)
✓ ping 8.8.8.8          (DNS Google - debe funcionar)
✗ ping 10.0.0.100       (PC VLAN 10 - DEBE SER BLOQUEADO por ACL)
✗ ping 10.0.2.10        (Servidor interno - DEBE SER BLOQUEADO)
✓ Abrir navegador → http://www.google.com (debe funcionar vía NAT)
```

### 4.3 Pruebas de Telefonía IP

1. **Llamada Local (misma sede)**:
   - Desde teléfono 1001 llamar a 1002
   - Debería conectar

2. **Llamada Inter-sede**:
   - Desde teléfono 1001 (Central) llamar a 2001 (Suc A)
   - Debería conectar vía OSPF

3. **Verificación**:
   ```
   show ephone registered
   show telephony-service ephone
   ```

### 4.4 Pruebas de WiFi

#### WiFi Corporativo:
1. Conectar laptop a SSID "Corporativo-Central"
2. Ingresar contraseña: `Empresa2024!`
3. Debería recibir IP de VLAN 80 vía DHCP
4. Ping a servidores internos debe funcionar

#### WiFi Invitados:
1. Conectar laptop a SSID "Invitados" (Suc B)
2. Ingresar contraseña: `Invitados2024` (si aplica)
3. Debería recibir IP de rango 10.0.32.128/25
4. Ping a internet debe funcionar
5. Ping a red interna debe ser BLOQUEADO

### 4.5 Pruebas de Seguridad

#### Port Security:
1. Conectar PC a puerto con port-security
2. Cambiar MAC de PC
3. Verificar que puerto se bloquea
4. Ver en Syslog el evento de violación

#### ACLs:
1. Desde VLAN 70 intentar ping a VLAN 10
2. Debe ser bloqueado
3. Desde VLAN 70 intentar HTTP a servidor interno
4. Debe ser bloqueado
5. Desde VLAN 70 intentar HTTP a 8.8.8.8
6. Debe funcionar

### 4.6 Pruebas de Alta Disponibilidad

#### HSRP (Sede Central):
1. Verificar gateway virtual:
   ```
   show standby brief
   ```
2. Apagar interfaz del router activo
3. Verificar failover al router standby
4. PCs deben mantener conectividad

#### Spanning Tree:
1. Verificar root bridge:
   ```
   show spanning-tree
   ```
2. Desconectar enlace principal
3. Verificar reconvergencia
4. Conectividad debe mantenerse

#### EtherChannel:
1. Verificar bundle:
   ```
   show etherchannel summary
   ```
2. Desconectar un enlace del EtherChannel
3. Tráfico debe balancearse en enlaces restantes

### 4.7 Pruebas de Servicios

#### DNS:
1. Desde cualquier PC:
   ```
   nslookup www.empresa.local
   ```
   Debe resolver a 10.0.2.10

#### HTTP:
1. Abrir navegador en PC
2. Ir a: http://www.empresa.local
3. Debe mostrar página web corporativa

#### DHCP:
1. En PC, cambiar a DHCP
2. Hacer `ipconfig /release`
3. Hacer `ipconfig /renew`
4. Debe recibir IP del rango correspondiente

#### Syslog:
1. En servidor Syslog, ver eventos recibidos
2. Apagar una interfaz de router
3. Verificar que evento aparece en Syslog
4. Documentar mínimo 10 eventos diferentes

---

## PASO 5: DOCUMENTACIÓN FINAL

### 5.1 Bitácora de Eventos Syslog (Mínimo 10)

Documentar en tu informe:

1. **Evento**: Interface GigabitEthernet0/0 up
   - **Dispositivo**: R-Central
   - **Timestamp**: [capturar de Syslog]
   - **Severity**: Informational

2. **Evento**: OSPF neighbor 2.2.2.2 up
   - **Dispositivo**: R-Central
   - **Timestamp**: [capturar]
   - **Severity**: Informational

3. **Evento**: Port Security violation on Fa0/1
   - **Dispositivo**: SW-Acceso1-Central
   - **Timestamp**: [capturar]
   - **Severity**: Warning

4. **Evento**: Spanning Tree topology change
   - **Dispositivo**: SW-Core1-Central
   - **Timestamp**: [capturar]
   - **Severity**: Informational

5. **Evento**: HSRP state change to Active
   - **Dispositivo**: R-Central
   - **Timestamp**: [capturar]
   - **Severity**: Informational

6. **Evento**: Configuration saved
   - **Dispositivo**: R-SucursalA
   - **Timestamp**: [capturar]
   - **Severity**: Informational

7. **Evento**: ACL denied packet from 10.0.32.150
   - **Dispositivo**: R-SucursalB
   - **Timestamp**: [capturar]
   - **Severity**: Informational

8. **Evento**: EtherChannel bundle created
   - **Dispositivo**: SW-Core1-Central
   - **Timestamp**: [capturar]
   - **Severity**: Informational

9. **Evento**: Login success on VTY line
   - **Dispositivo**: R-Central
   - **Timestamp**: [capturar]
   - **Severity**: Informational

10. **Evento**: Interface Serial0/0/0 down
    - **Dispositivo**: R-SucursalB
    - **Timestamp**: [capturar]
    - **Severity**: Critical

### 5.2 Capturas de Pantalla Requeridas

Incluir en tu informe:

1. Topología completa en Packet Tracer (modo lógico y físico)
2. Tabla de rutas OSPF en R-Central (`show ip route`)
3. VLANs configuradas en switches (`show vlan brief`)
4. HSRP status mostrando Active/Standby
5. EtherChannel summary
6. Port Security configurado
7. Servidor Syslog mostrando eventos
8. Ping exitoso entre sedes
9. Traceroute mostrando ruta OSPF
10. Llamada de teléfono IP entre sedes
11. PC conectado a WiFi (DHCP recibido)
12. ACL bloqueando tráfico de invitados
13. Servidor HTTP mostrando página web
14. DNS resolviendo nombres

### 5.3 Tabla de Direccionamiento Final

Crear tabla con TODOS los dispositivos:

| Dispositivo | VLAN | IP | Máscara | Gateway | Observaciones |
|-------------|------|----|---------| --------|---------------|
| R-Central G0/0.10 | 10 | 10.0.1.254 | /23 | - | HSRP Primary |
| R-Central G0/1 | WAN | 10.255.1.1 | /30 | - | Enlace a Suc A |
| ... | ... | ... | ... | ... | ... |

### 5.4 Comandos de Verificación Resumen

```bash
# En todos los routers
show ip interface brief
show ip route ospf
show ip ospf neighbor
show ip dhcp binding
show ip nat translations
show standby brief

# En todos los switches
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show port-security
show port-security address

# En PCs
ipconfig /all
ping [gateway]
ping [servidor]
tracert [destino remoto]
nslookup www.empresa.local
```

---

## PASO 6: VERIFICACIÓN FINAL CONTRA CHECKLIST

Usar el archivo `CHECKLIST_PROYECTO.md` para verificar que TODO esté completo:

- [ ] ✓ Todas las VLANs creadas y funcionando
- [ ] ✓ OSPF configurado y vecinos establecidos
- [ ] ✓ DHCP asignando IPs automáticamente
- [ ] ✓ DNS resolviendo nombres
- [ ] ✓ HTTP/HTTPS accesible
- [ ] ✓ NAT funcionando (acceso a internet simulado)
- [ ] ✓ Telefonía IP: 6 teléfonos funcionando con llamadas entre sedes
- [ ] ✓ WiFi: 3+ Access Points configurados
- [ ] ✓ WiFi Invitados AISLADO de red interna
- [ ] ✓ ACLs bloqueando tráfico según políticas
- [ ] ✓ Port Security en 50%+ de puertos
- [ ] ✓ Contraseñas en TODOS los dispositivos
- [ ] ✓ Banners configurados
- [ ] ✓ HSRP funcionando con failover
- [ ] ✓ STP previniendo loops
- [ ] ✓ EtherChannel con múltiples enlaces
- [ ] ✓ Syslog recibiendo eventos
- [ ] ✓ 10+ eventos documentados
- [ ] ✓ IPv6 configurado y funcionando
- [ ] ✓ Impresoras accesibles según ACLs
- [ ] ✓ SNMP configurado (opcional)

---

## ARCHIVOS DE RESPALDO

Una vez completado:

1. Guardar archivo PKT: `Proyecto_Telecomunicaciones_COMPLETO.pkt`
2. Exportar todas las configuraciones:
   - En cada dispositivo: `show running-config`
   - Copiar a archivo de texto para respaldo
3. Capturar todas las pantallas de verificación
4. Completar informe técnico con bitácora Syslog

---

## TROUBLESHOOTING COMÚN

### Problema: OSPF no forma vecindad
**Solución**:
- Verificar que interfaces estén up/up
- Verificar que área OSPF sea la misma
- Verificar que no haya mismatch de subnet
- `show ip ospf neighbor`
- `debug ip ospf adj` (en caso extremo)

### Problema: DHCP no asigna IPs
**Solución**:
- Verificar pool configurado en router
- Verificar `ip helper-address` en sub-interface
- Verificar que VLAN esté creada en switches
- Verificar trunk permite la VLAN
- `show ip dhcp binding`
- `debug ip dhcp server events`

### Problema: Teléfonos IP no se registran
**Solución**:
- Verificar que puerto tenga `switchport voice vlan`
- Verificar DHCP Option 150 configurado
- Verificar que VLAN de voz esté creada
- Verificar conectividad a CallManager

### Problema: WiFi invitados puede acceder a red interna
**Solución**:
- Verificar ACL en interfaz VLAN 70
- Verificar orden de reglas en ACL (deny antes de permit)
- `show access-lists`
- `show ip interface [interface]`

### Problema: Port Security bloquea puerto constantemente
**Solución**:
- Aumentar `maximum` si es puerto de teléfono+PC
- Para teléfono+PC usar `maximum 3`
- Verificar aging time
- `show port-security interface [interface]`
- `switchport port-security aging time 10`

### Problema: EtherChannel no se forma
**Solución**:
- Verificar que ambos lados usen mismo modo (active-active o desirable-desirable)
- Verificar que interfaces tengan misma configuración
- Verificar que todas interfaces estén en mismo VLAN o trunk
- `show etherchannel summary`
- `show interfaces etherchannel`

---

## CONTACTO Y SOPORTE

Si encuentras problemas durante la implementación:

1. Verifica archivo de configuración correspondiente en carpeta `configs/`
2. Revisa esta guía paso a paso
3. Consulta checklist en `CHECKLIST_PROYECTO.md`
4. Revisa troubleshooting común arriba

---

**¡Éxito con tu proyecto!**

**Última actualización**: 2025-11-16
