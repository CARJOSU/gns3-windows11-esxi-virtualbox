# GNS3 + Virtualización en Windows 11

## Objetivo

El objetivo de este trabajo es investigar y poner en práctica la creación de laboratorios de red usando GNS3 en Windows 11, integrando tanto un hipervisor tipo 2 (VirtualBox) como uno tipo 1 (VMware ESXi).  
La idea es lograr un entorno lo más parecido posible a una red real.

---

##        1. Arquitectura de Virtualización en Windows 11

### Aislamiento de núcleo y VBS

Windows 11 tiene funciones de seguridad como el aislamiento de núcleo y VBS que usan Hyper-V internamente.

Esto puede causar problemas porque:
- Ocupa la virtualización (VT-x / AMD-V)
- Genera conflictos con VirtualBox
- Hace que GNS3 VM no funcione correctamente

#### Problema común

---


#### Solución

- Ir a Seguridad de Windows
- Entrar a "Seguridad del dispositivo"
- Desactivar "Aislamiento de núcleo"
- Reiniciar el equipo

---

### Activación de virtualización (VT-x / AMD-V)

Para que todo funcione bien:

1. Entrar al BIOS/UEFI
2. Activar:
   - Intel VT-x (Intel)
   - AMD-V (AMD)
3. Guardar cambios

#### Verificación

En el Administrador de tareas → Rendimiento:

Debe aparecer:




##           2. GNS3 VM

###  KVM

GNS3 utiliza KVM para ejecutar las máquinas virtuales con mejor rendimiento.

####  Estado correcto

---


Si aparece en False:
- La virtualización no está bien configurada
- El rendimiento baja bastante

---

###  Recursos recomendados

| Recurso | Configuración |
|--------|-------------|
| RAM | 4 – 8 GB |
| CPU | 2 – 4 núcleos |

Esto ayuda a que el sistema no se sature y funcione estable.

---

##      3. Integración con VirtualBox

###  Red Host-Only

Se usa para conectar:
- El GNS3 (en Windows)
- La GNS3 VM

#### Pasos:
- Crear adaptador Host-Only en VirtualBox
- Asignarlo a la GNS3 VM



###  Modo Promiscuo

Permite que la tarjeta de red vea todo el tráfico.

Es importante para:
- Switches virtuales
- VLANs
- Comunicación entre dispositivos

#### Configuración:


---


---

##  4. Integración con VMware ESXi

###  Arquitectura

- Cliente: GNS3 en la PC
- Servidor: ESXi con máquinas virtuales

Esto permite trabajar con topologías más grandes sin afectar tanto la computadora.

---

###  Configuración del vSwitch

| Opción | Valor |
|------|------|
| Promiscuous Mode | Accept |
| MAC Address Changes | Accept |

Esto es necesario para que la red funcione correctamente.

---

##  5. Troubleshooting

| Problema | Causa | Solución |
|---------|------|---------|
| KVM en False | Virtualización no disponible | Activar VT-x / AMD-V |
| No hay conexión | Promiscuo desactivado | Permitir todo |
| Error puerto 3080 | Firewall | Abrir puertos |



###  Comando usado

```bash
VBoxManage modifyvm "GNS3 VM" --nested-hw-virt on


---

##  Conclusión

La integración de GNS3 con hipervisores tipo 1 (VMware ESXi) y tipo 2 (VirtualBox) en Windows 11 permite crear laboratorios de red avanzados y realistas, ideales para aprender y probar configuraciones complejas.  

Es fundamental configurar correctamente la virtualización (activar VT-x/AMD-V y desactivar el aislamiento de núcleo) y ajustar las opciones de red, como el modo promiscuo, para evitar problemas de rendimiento y conectividad.  

Con estos ajustes, el entorno es estable y eficiente, facilitando el desarrollo de prácticas profesionales y experimentos en redes.


