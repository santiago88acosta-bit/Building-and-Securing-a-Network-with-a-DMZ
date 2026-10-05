
# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Explica brevemente qué se buscaba lograr con este laboratorio.

Configurar una Zona DMZ utilizando un router Cisco ISR para alojar un servidor web, permitiendo acceso controlado desde la red externa y la red interna, mientras se aísla la red interna de la DMZ.

### 2. Topología implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

- Cantidad de redes: 3
- Dispositivos usados: 7
- Breve descripción de la función de cada zona (LAN, DMZ, Externa): 

La red interna es para conectar dispositivos y sistemas para permitir que la información fluya de forma rápida, colaborativa y segura entre los empleados.

La red externa es paraconectar la organización con el mundo exterior, permitiendo el acceso a internet, la comunicación con sucursales remotas y el intercambio seguro de datos con clientes y proveedores

Y la zona DMZ es para actuar como un escudo protector o zona de amortiguamiento entre la red interna privada e internet.



### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambió).

| Dispositivo             | IP              | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |192.168.1.10      |255.255.255.0      |192.168.1.1        |
| Server_DMZ              |192.168.2.10      |255.255.255.0      |192.168.2.1        |
| PC_External             |192.168.3.10      |255.255.255.0      |192.168.3.1        |
| Router_FW Gi0/0 (LAN)   |192.168.1.1       |255.255.255.0      |                   |
| Router_FW Gi0/1 (DMZ)   |192.168.2.1       |255.255.255.0      |                   |
| Router_FW Gi0/2 (Ext)   |192.168.3.1       |255.255.255.0      |                   |


### 4. Configuración aplicada (resumen)

> Resume los comandos o pasos más relevantes que ejecutaste. Usa texto + fragmentos de código cuando sea necesario.

- Interfaces configuradas con `ip address`

GigabitEthernet0/0 este va conectado a la ip LAN interna, GigabitEthernet0/1 este a la del DMZ, GigabitEthernet0/2 y este a la red externa 

- NAT:

interface GigabitEthernet0/1
ip nat inside
Le indica al router que la interfaz GigabitEthernet0/1 pertenece a la red interna

interface GigabitEthernet0/2
ip nat outside
Le indica al router que la interfaz GigabitEthernet0/2 está conectada a la red externa

ip nat inside source static 192.168.2.10 192.168.3.1
Este NAT estático, su función es crear un enlace permanente y exclusivo de uno a uno entre la dirección IP privada interna y una dirección IP pública externa

```bash
ip nat inside source static 192.168.2.10 192.168.3.1
```
- ACLs:

access-list 101 permit tcp any host 192.168.3.1 eq 80
Esta permite solamente el tráfico HTTP (puerto 80) desde cualquier origen (any) hacia la IP pública de tu servidor web DMZ (192.168.3.1).

access-list 102 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 102 permit ip any any
Esta crea una ACL que deniega completamente cualquier intento de comunicación que se origine desde la red DMZ

```bash
access-list 101 permit tcp any host 192.168.3.1 eq 80
access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```



### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

- `ping` desde PC_Internal al router: ✅

![alt text](../image.png) Al hacer ping desde la pc de la red interna hacia el router [ping 192.168.1.1] se pudo establecer la coneccion con este.

- Acceso web desde PC_External: ✅

![alt text](../image-1.png) Se puede ver que la pc de la red externa tiene acceso hacia el servidor web

- Bloqueo de acceso desde DMZ a LAN: ✅

![alt text](../image-2.png) El servidor DMZ no encontro acceso a LAN, por lo cual eso indica que tiene el puerto bloqueado hacia esa red.

### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?

Aprendi acerca de usar los comandos de la terminal del CLI. Recomendaria usar los ALCs mas a menudo porque facilitan bastante la seguridad y el ritmo de la propia red.


### 7. Capturas de evidencia

> Adjunta aquí (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
[text](../Evidencias.pdf)