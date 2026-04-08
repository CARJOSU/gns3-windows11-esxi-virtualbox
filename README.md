# GNS3 + Virtualización en Windows 11

## Objetivo

Este documento muestra cómo configurar laboratorios de red usando GNS3 en Windows 11, integrando VirtualBox (hipervisor tipo 2) y VMware ESXi (hipervisor tipo 1).  
El objetivo es poder simular redes reales de manera eficiente y estable.

---

## 1. Arquitectura de Virtualización en Windows 11

### Aislamiento de núcleo y VBS

Windows 11 incluye funciones de seguridad que usan Hyper-V, como el aislamiento de núcleo y VBS.  
Estas funciones pueden interferir con otros hipervisores, haciendo que GNS3 VM no funcione correctamente.

**Problemas frecuentes:**
- Virtualización ocupada por Windows
- Conflictos con VirtualBox
- KVM no disponible

**Solución:**
1. Abrir Seguridad de Windows
2. Ir a “Seguridad del dispositivo”
3. Desactivar “Aislamiento de núcleo”
4. Reiniciar el sistema

### Activar VT-x / AMD-V

1. Entrar al BIOS/UEFI
2. Activar Intel VT-x o AMD-V según corresponda
3. Guardar y reiniciar

**Verificación:**  
Administrador de tareas → Rendimiento → Virtualización: Habilitada

---

## 2. GNS3 VM

### Estado de la máquina virtual

A continuación se muestra la ejecución correcta de la GNS3 VM en VirtualBox, incluyendo la IP asignada y acceso al servidor:

https://github.com/CARJOSU/gns3-windows11-esxi-virtualbox/blob/main/img/mermaid-diagram%20(1).png

**Figura 1.** Ejecución de la máquina virtual de GNS3 mostrando dirección IP y estado del servidor.

---

### KVM

GNS3 usa KVM para ejecutar máquinas virtuales de forma eficiente.

- Estado correcto: `KVM support available: True`  
- Si aparece False: bajo rendimiento y virtualización deshabilitada

### Imagen de KVM habilitado

![Topología de GNS3](img/img/mermaid-diagram (1).png)


### Recomendación de recursos

| Recurso | Sugerido |
|--------|-----------|
| RAM    | 4 – 8 GB  |
| CPU    | 2 – 4 núcleos |

Esto ayuda a que Windows funcione estable mientras corre GNS3 VM.

---

## 3. Integración con VirtualBox

### Red Host-Only

Permite que GNS3 en la PC se comunique con la GNS3 VM.

### Imagen de VirtualBox Host-Only

![VirtualBox Host-Only](img/virtualbox-hostonly.png)

### Modo Promiscuo

Permite capturar todo el tráfico de red, necesario para switches virtuales y VLANs.

**Configuración:**  
`Modo Promiscuo → Permitir todo`

---

## 4. Integración con VMware ESXi

### Arquitectura

- Cliente: GNS3 en tu PC  
- Servidor: ESXi ejecutando las máquinas virtuales

### Configuración del vSwitch

| Opción | Valor |
|--------|-------|
| Promiscuous Mode | Accept |
| MAC Address Changes | Accept |

### Diagrama de integración GNS3 + ESXi

![Diagrama ESXi](img/diagrama-integracion.png)

---

## 5. Troubleshooting

| Problema | Causa | Solución |
|----------|-------|---------|
| KVM en False | Virtualización no disponible | Activar VT-x / AMD-V |
| Sin conexión | Modo promiscuo desactivado | Permitir todo |
| Error puerto 3080 | Firewall | Abrir puertos 3080 y 5000-10000 |

---

## Comando importante

```bash
VBoxManage modifyvm "GNS3 VM" --nested-hw-virt on
