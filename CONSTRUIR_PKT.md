# Guía para Construir el Proyecto en Packet Tracer
## Proyecto de Telecomunicaciones - Paso a Paso

---

## IMPORTANTE

Esta guía te ayudará a construir el proyecto **MANUALMENTE** en Cisco Packet Tracer.
No puedo generar el archivo .pkt directamente porque es un archivo binario propietario.

**Tiempo estimado**: 2-3 horas

---

## PARTE 1: CREAR DISPOSITIVOS

### Abre Cisco Packet Tracer y crea un nuevo proyecto

### SEDE CENTRAL

**Routers:**
1. Arrastra **Router 2811** (o 2900 series)
   - Renombrar: `R-Central`
   - Apagar el router (botón power)
   - Agregar módulos si es necesario:
     - HWIC-2T (para Serial)
     - Si ya tiene GigabitEthernet, bien

**Switches:**
1. Arrastra **Switch 3560** (Multilayer Switch)
   - Renombrar: `SW-Core1-Central`

2. Arrastra **Switch 2960**
   - Renombrar: `SW-Acceso1-Central`

**Servidores:**
1. Arrastra **Server** (x3)
   - `Servidor-DNS` (10.0.2.10)
   - `Servidor-Syslog` (10.0.2.20)
   - `Servidor-DHCP` (10.0.2.15) - Opcional

**PCs:**
1. Arrastra **PC** (x15-20)
   - Distribuir en diferentes VLANs

**Teléfonos:**
1. Arrastra **IP Phone 7960** (x4)
   - Para VLAN 80 (VoIP)

**Impresoras:**
1. Arrastra **Printer** (x1)
   - Para VLAN 60

**Access Point:**
1. Arrastra **Access Point-PT** (x1)
   - Para WiFi corporativo

**Cloud (Internet):**
1. Arrastra **Cloud-PT** (x1)
   - Simular internet

### SUCURSAL A

1. **Router 2811**: `R-SucursalA`
2. **Switch 2960**: `SW-SucursalA`
3. **PCs** (x8-10)
4. **IP Phones** (x2)
5. **Printer** (x1)
6. **Access Point** (x1)

### SUCURSAL B

1. **Router 2811**: `R-SucursalB`
2. **Switch 2960**: `SW-SucursalB`
3. **PCs** (x12-15)
4. **Laptops** (x3-5) para WiFi invitados
5. **IP Phones** (x2)
6. **Printer** (x1)
7. **Access Points** (x2) - uno corporativo, uno invitados

---

## PARTE 2: CONECTAR DISPOSITIVOS

### SEDE CENTRAL

**Conexiones Router ↔ Switch:**
```
R-Central [Gig0/0] ---- [Gig0/1] SW-Core1-Central
```

**Conexiones Switch Core ↔ Switch Acceso (EtherChannel):**
```
SW-Core1 [Gig0/3] ---- [Gig0/1] SW-Acceso1
SW-Core1 [Gig0/4] ---- [Gig0/2] SW-Acceso1
```

**Conexiones WAN:**
```
R-Central [Gig0/1] ---- [Gig0/1] R-SucursalA (Cable Copper Cross-Over)
R-Central [Serial0/0/0] ---- [Serial0/0/0] R-SucursalB (Cable Serial DCE)
```

**Internet:**
```
R-Central [Gig0/2] ---- [Ethernet6] Cloud (Cable Copper Straight-Through)
```

**Servidores → Switch Core:**
```
Servidor-DNS [FastEthernet] ---- [Fa0/1] SW-Core1
Servidor-Syslog [FastEthernet] ---- [Fa0/2] SW-Core1
Servidor-DHCP [FastEthernet] ---- [Fa0/3] SW-Core1
```

**PCs → Switch Acceso:**
```
Distribuir PCs en puertos Fa0/1-24 de SW-Acceso1
```

**Teléfonos IP:**
```
Conectar a puertos Fa0/11-15 de SW-Acceso1
(Estos puertos tendrán voice VLAN configurada)
```

**Impresora:**
```
Impresora [FastEthernet] ---- [Fa0/22] SW-Acceso1
```

**Access Point:**
```
AP [FastEthernet] ---- [Fa0/20] SW-Acceso1
```

### SUCURSAL A

```
R-SucursalA [Gig0/0] ---- [Gig0/1] SW-SucursalA
PCs ---- [Fa0/1-15] SW-SucursalA
Teléfonos ---- [Fa0/6-10] SW-SucursalA
AP ---- [Fa0/20] SW-SucursalA
Impresora ---- [Fa0/21] SW-SucursalA
```

### SUCURSAL B

```
R-SucursalB [Gig0/0] ---- [Gig0/1] SW-SucursalB
PCs ---- [Fa0/1-22] SW-SucursalB
AP Invitados ---- [Fa0/23] SW-SucursalB
AP Corporativo ---- [Fa0/24] SW-SucursalB
Impresora ---- [Fa0/21] SW-SucursalB
```

---

## PARTE 3: APLICAR CONFIGURACIONES

### Configurar R-Central

1. Clic en **R-Central**
2. Ir a pestaña **CLI**
3. Presionar Enter
4. **Copiar TODO** el contenido de `configs_limpias/01_R-Central.txt`
5. **Pegar** en la consola
6. Esperar a que termine (verás el prompt `R-Central#`)

### Configurar R-SucursalA

1. Clic en **R-SucursalA**
2. CLI → Copiar/Pegar de `configs_limpias/02_R-SucursalA.txt`

### Configurar R-SucursalB

1. Clic en **R-SucursalB**
2. CLI → Copiar/Pegar de `configs_limpias/03_R-SucursalB.txt`

### Configurar SW-Core1-Central

1. Clic en **SW-Core1-Central**
2. CLI → Copiar/Pegar de `configs_limpias/04_SW-Core1-Central.txt`

### Configurar SW-Acceso1-Central

1. Clic en **SW-Acceso1-Central**
2. CLI → Copiar/Pegar de `configs_limpias/05_SW-Acceso1-Central.txt`

### Configurar SW-SucursalA

1. Clic en **SW-SucursalA**
2. CLI → Copiar/Pegar de `configs_limpias/06_SW-SucursalA.txt`

### Configurar SW-SucursalB

1. Clic en **SW-SucursalB**
2. CLI → Copiar/Pegar de `configs_limpias/07_SW-SucursalB.txt`

---

## PARTE 4: CONFIGURAR DISPOSITIVOS FINALES

### Servidor DNS/HTTP (10.0.2.10)

1. Clic en **Servidor-DNS**
2. Pestaña **Config**
3. **FastEthernet**:
   - IP Address: `10.0.2.10`
   - Subnet Mask: `255.255.254.0`
   - Default Gateway: `10.0.1.1`
   - DNS Server: `10.0.2.10`

4. Pestaña **Services**
5. **DNS**:
   - Activar: ON
   - Agregar registros:
     ```
     www.empresa.local → 10.0.2.10
     empresa.local → 10.0.2.10
     intranet.empresa.local → 10.0.2.10
     syslog.empresa.local → 10.0.2.20
     ```

6. **HTTP**:
   - Activar: ON
   - Editar index.html:
     ```html
     <html>
     <head><title>Intranet Corporativa</title></head>
     <body>
     <h1>Bienvenido a la Intranet</h1>
     <p>Red Empresarial Multisede</p>
     <ul>
       <li>Sede Central</li>
       <li>Sucursal A</li>
       <li>Sucursal B</li>
     </ul>
     </body>
     </html>
     ```

7. **HTTPS**: Activar ON

### Servidor Syslog (10.0.2.20)

1. Clic en **Servidor-Syslog**
2. **Config → FastEthernet**:
   - IP: `10.0.2.20`
   - Mask: `255.255.255.248`
   - Gateway: `10.0.5.209`
   - DNS: `10.0.2.10`

3. **Services → Syslog**:
   - Activar: ON

### PCs (Ejemplo: PC en VLAN 10)

1. Clic en **PC**
2. **Desktop → IP Configuration**:
   - Seleccionar: **DHCP**
   - Esperar a que reciba IP automáticamente
   - Debería recibir:
     - IP: 10.0.0.x
     - Gateway: 10.0.1.1
     - DNS: 10.0.2.10

**Repetir para TODOS los PCs** (configurar en DHCP)

### Teléfonos IP

1. Conectar teléfono a puerto con voice VLAN
2. Encender teléfono
3. Configuración automática vía DHCP
4. Verificar que recibe IP de VLAN 80

### Access Points

#### AP Sede Central (Corporativo):

1. Clic en **Access Point**
2. **Config → Port 1**:
   - SSID: `Corporativo-Central`
   - Authentication: **WPA2-PSK**
   - PSK Pass Phrase: `Empresa2024!`

3. **FastEthernet**:
   - IP Configuration: **DHCP**

#### AP Sucursal B - Invitados:

1. **Config → Port 1**:
   - SSID: `Invitados`
   - Authentication: **Open** (o WPA2-PSK con contraseña)

2. Conectar Laptops:
   - En laptop: **Desktop → PC Wireless**
   - Connect → SSID: `Invitados`
   - Debería recibir IP de VLAN 70

### Impresoras

1. Clic en **Printer**
2. **Config → FastEthernet**:
   - IP: `10.0.5.180`
   - Mask: `255.255.255.240`
   - Gateway: `10.0.5.177`

---

## PARTE 5: VERIFICAR CONECTIVIDAD

### Desde cualquier PC:

1. **Desktop → Command Prompt**

```
ping 10.0.1.1          (Gateway)
ping 10.0.2.10         (Servidor DNS)
ping 10.0.16.14        (Sucursal A)
ping 10.0.32.126       (Sucursal B)
ping www.empresa.local (DNS funcionando)
tracert 10.0.16.14     (Ver ruta OSPF)
```

### Desde R-Central CLI:

```
show ip ospf neighbor
show ip route ospf
show standby brief
show ip dhcp binding
```

### Desde SW-Core1:

```
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
```

### Probar HTTP:

1. En PC: **Desktop → Web Browser**
2. URL: `http://www.empresa.local`
3. Debería mostrar página web

### Probar Telefonía:

1. Levantar teléfono 1001
2. Marcar: 2001 (Sucursal A)
3. Debería conectar

### Probar WiFi Invitados AISLADO:

1. En laptop conectada a "Invitados"
2. **Command Prompt**:
   ```
   ping 10.0.2.10  (Debería FALLAR - bloqueado por ACL)
   ping 8.8.8.8    (Debería funcionar)
   ```

---

## PARTE 6: GUARDAR PROYECTO

1. **File → Save As**
2. Nombre: `Proyecto_Telecomunicaciones_[TuNombre].pkt`
3. Guardar

---

## TIPS IMPORTANTES

### Si algo no funciona:

1. **Interfaces apagadas**: Asegúrate que todas las interfaces estén `up/up`
   ```
   show ip interface brief
   ```

2. **VLANs no creadas**: Verifica en switches
   ```
   show vlan brief
   ```

3. **OSPF no forma vecindad**:
   ```
   show ip ospf neighbor
   debug ip ospf adj
   ```

4. **DHCP no asigna IPs**:
   - Verifica helper-address en router
   - Verifica pool DHCP configurado

5. **Port Security bloquea puerto**:
   ```
   show port-security interface fa0/1
   conf t
   interface fa0/1
   shutdown
   no shutdown
   ```

### Módulos necesarios en routers:

- **R-Central**: Necesita módulo serial (HWIC-2T)
- **R-SucursalB**: Necesita módulo serial

Para agregar:
1. Apagar router (botón power)
2. Arrastrar módulo HWIC-2T a slot
3. Encender router

### Cables correctos:

- **Router ↔ Switch**: Copper Straight-Through
- **Switch ↔ PC**: Copper Straight-Through
- **Router ↔ Router (WAN Gig)**: Copper Cross-Over
- **Router ↔ Router (Serial)**: Serial DCE

---

## CHECKLIST FINAL

- [ ] Todos los dispositivos tienen nombres correctos
- [ ] Todas las conexiones están correctas (triángulos verdes)
- [ ] Configuraciones aplicadas en todos los routers
- [ ] Configuraciones aplicadas en todos los switches
- [ ] Servidores configurados (DNS, HTTP, Syslog)
- [ ] PCs reciben IP por DHCP
- [ ] Ping entre sedes funciona
- [ ] OSPF muestra vecinos (`show ip ospf neighbor`)
- [ ] DNS resuelve nombres (`nslookup www.empresa.local`)
- [ ] HTTP muestra página web
- [ ] Teléfonos registrados y funcionan
- [ ] WiFi conecta correctamente
- [ ] WiFi Invitados BLOQUEADO de red interna
- [ ] Syslog recibe eventos
- [ ] HSRP muestra Active/Standby

---

## NOTAS FINALES

- **Tiempo total**: 2-3 horas aproximadamente
- **Guardar frecuentemente**: Cada 15-20 minutos
- **Probar paso a paso**: No esperes al final para probar
- **Documentar**: Toma capturas de pantalla mientras construyes

---

**¡Éxito con tu proyecto!**

Si algo falla, revisa los comandos de verificación en `COMANDOS_VERIFICACION.md`
