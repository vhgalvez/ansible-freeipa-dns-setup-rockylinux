
# Configuración de CoreDNS en AlmaLinux con Ansible

Este repositorio contiene playbooks de Ansible para instalar y configurar CoreDNS como servidor DNS en servidores con AlmaLinux.

---

## 🔍 Descripción

Este proyecto automatiza la instalación y configuración de CoreDNS para gestionar solicitudes DNS en tu infraestructura. CoreDNS es un servidor DNS flexible y extensible, ideal para ambientes de contenedores y redes internas.

### Los playbooks incluyen:
- Instalación de CoreDNS
- Configuración como servidor DNS local
- Configuración del firewall para puertos necesarios
- Creación de registros DNS para servidores y servicios

---

## ✅ Requisitos Previos

- Servidor con **AlmaLinux 9** configurado
- Ansible instalado localmente o en un servidor de administración
- Acceso SSH habilitado al servidor remoto

---

## 🛠️ Instalación de Ansible en AlmaLinux 9

```bash
sudo dnf install epel-release -y
sudo dnf install ansible -y
ansible --version
```

---

## 👀 Clonar el Repositorio

```bash
git clone https://github.com/vhgalvez/ansible-coredns-setup-almalinux.git
```

---

## 🚪 Ejecutar un Playbook de Ansible en un Servidor Remoto

### Paso 1: Verificar la Conectividad SSH

```bash
ssh -i /ruta/a/tu/clave/privada user@<ip-del-servidor> -p 22
```

### Paso 2: Configurar el Inventario

Archivo `inventory.ini`:

```ini
[freeipa_servers]
10.17.3.11 ansible_user=core ansible_ssh_private_key_file=/ruta/a/tu/clave/privada ansible_port=22
```

### Paso 3: Crear el Playbook

Guardar el playbook como `coredns_setup.yml`.

### Paso 4: Ejecutar el Playbook

```bash
sudo ansible-playbook -i inventory.ini coredns_setup.yml
```

### Paso 5: Verificar la Configuración

```bash
sudo systemctl status coredns
dig google.com
dig 10.17.3.11
```

### Paso 6: Habilitar el Servicio DNS y Configurar el Firewall

```bash
sudo systemctl enable coredns
sudo systemctl start coredns
sudo systemctl status coredns
sudo firewall-cmd --list-all
```

---

## 📄 Resumen de Recursos para Máquinas Virtuales

| Nombre de VM     | CPU | RAM (MB) | IP           | Dominio                           | Disco (GB) | Hostname     |
|------------------|-----|----------|--------------|-----------------------------------|------------|--------------|
| master1          | 2   | 4096     | 10.17.4.21   | master1.cefaslocalserver.com      | 50         | master1      |
| master2          | 2   | 4096     | 10.17.4.22   | master2.cefaslocalserver.com      | 50         | master2      |
| master3          | 2   | 4096     | 10.17.4.23   | master3.cefaslocalserver.com      | 50         | master3      |
| worker1          | 2   | 4096     | 10.17.4.24   | worker1.cefaslocalserver.com      | 50         | worker1      |
| worker2          | 2   | 4096     | 10.17.4.25   | worker2.cefaslocalserver.com      | 50         | worker2      |
| worker3          | 2   | 4096     | 10.17.4.26   | worker3.cefaslocalserver.com      | 50         | worker3      |
| bootstrap        | 2   | 4096     | 10.17.4.27   | bootstrap.cefaslocalserver.com    | 50         | bootstrap    |
| freeipa1         | 2   | 2048     | 10.17.3.11   | freeipa1.cefaslocalserver.com     | 32         | freeipa1     |
| loadbalancer1    | 2   | 2048     | 10.17.3.12   | loadbalancer1.cefaslocalserver.com| 32         | loadbalancer1|
| postgresql1      | 2   | 2048     | 10.17.3.13   | postgresql1.cefaslocalserver.com  | 32         | postgresql1  |
| helper           | 2   | 2048     | 10.17.3.14   | helper.cefaslocalserver.com       | 32         | helper_node  |

---

## ✅ Resumen Final

1. Verificar conectividad SSH
2. Crear el archivo `inventory.ini`
3. Guardar el playbook como `coredns_setup.yml`
4. Ejecutar con `ansible-playbook`
5. Verificar servicio y firewall

---

## 📄 Licencia

Este proyecto está licenciado bajo la [Licencia MIT](https://opensource.org/licenses/MIT)

## 👨‍💼 Autor

**Victor Galvez**  
[https://github.com/vhgalvez](https://github.com/vhgalvez)


