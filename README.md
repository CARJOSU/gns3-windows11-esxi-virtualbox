# 🧪 GNS3 + Virtualización en Windows 11

## 📌 Objetivo

El objetivo de este trabajo es investigar y poner en práctica la creación de laboratorios de red usando GNS3 en Windows 11, integrando tanto un hipervisor tipo 2 (VirtualBox) como uno tipo 1 (VMware ESXi).  
La idea es lograr un entorno lo más parecido posible a una red real.

---

## 🖥️ 1. Arquitectura de Virtualización en Windows 11

### 🔐 Aislamiento de núcleo y VBS

Windows 11 tiene funciones de seguridad como el aislamiento de núcleo y VBS que usan Hyper-V internamente.

Esto puede causar problemas porque:
- Ocupa la virtualización (VT-x / AMD-V)
- Genera conflictos con VirtualBox
- Hace que GNS3 VM no funcione correctamente

#### ⚠️ Problema común
