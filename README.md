# Configuración de CoreDNS en AlmaLinux con Ansible

Este repositorio contiene playbooks de Ansible para instalar y configurar CoreDNS como servidor DNS en servidores que ejecutan AlmaLinux.

---

## 🔍 Descripción

Este proyecto automatiza la instalación y configuración de CoreDNS para gestionar solicitudes DNS dentro de una infraestructura. CoreDNS es un servidor DNS moderno, modular y eficiente, ideal para entornos de contenedores, redes internas o laboratorios.

### Funcionalidades de los Playbooks

- Instalación de CoreDNS.
- Configuración como servidor DNS local.
- Apertura de puertos requeridos en el firewall.
- Creación de registros DNS para servicios y nodos.

---

## ✅ Requisitos Previos

Antes de ejecutar los playbooks, asegúrate de tener:

- Un servidor con **AlmaLinux 9** configurado y funcionando.
- **Ansible** instalado en tu máquina local o servidor de administración.
- Acceso SSH al servidor remoto.

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
cd ansible-coredns-setup-almalinux
```

---

## 🚪 Ejecución del Playbook

### 1. Verificar Conectividad SSH

```bash
ssh -i /ruta/a/tu/key/actualizada user@<IP-del-servidor> -p 22
```

### 2. Configurar el Inventario de Ansible

Crea un archivo `inventory.ini`:

```ini
[coredns_servers]
10.17.3.11 ansible_user=core ansible_ssh_private_key_file=/ruta/a/tu/key/actualizada ansible_port=22
```

### 3. Ejecutar el Playbook

```bash
sudo ansible-playbook -i inventory.ini CoreDNS_setup.yml
```

### 4. Verificar la Configuración

```bash
sudo systemctl status coredns

# Probar resolución DNS
dig google.com
dig 10.17.3.11
```

### 5. Habilitar CoreDNS y Configurar el Firewall

```bash
sudo systemctl enable coredns
sudo systemctl start coredns
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
| infra-cluster    | 2   | 2048     | 10.17.3.11   | infra-cluster.cefaslocalserver.com| 32         | infra-cluster|
| loadbalancer1    | 2   | 2048     | 10.17.3.12   | loadbalancer1.cefaslocalserver.com| 32         | loadbalancer1|
| postgresql1      | 2   | 2048     | 10.17.3.13   | postgresql1.cefaslocalserver.com  | 32         | postgresql1  |
| helper           | 2   | 2048     | 10.17.3.14   | helper.cefaslocalserver.com       | 32         | helper_node  |

---

## 🔖 Licencia

Este proyecto está licenciado bajo la [Licencia MIT](https://opensource.org/licenses/MIT).

---

## 👨‍💼 Autor

**Victor Galvez**  
[https://github.com/vhgalvez](https://github.com/vhgalvez)

---

## 🔍 Registros DNS Especiales

### 1. Registro `k8s-api`

- **Qué es:** Un alias para el Virtual IP (VIP) usado por HAProxy + Keepalived para exponer el API Server de Kubernetes (puerto 6443).
- **Ejemplo de uso:** Permite que otros nodos del clúster (workers y masters) se conecten al API usando `https://k8s-api.cefaslocalserver.com:6443`.
- **IP real:** `10.17.5.10` (VIP flotante para el API Server).
- **Útil para:**
  - Comandos `kubectl` desde fuera del clúster.
  - Unión de nuevos nodos al clúster (`--server https://k8s-api.cefaslocalserver.com:6443`).
  - Automatizaciones como Ansible o Terraform que usen ese DNS.

### 2. Registro `ingress`

- **Qué es:** Es el VIP flotante que gestiona el tráfico HTTP(S) externo hacia Traefik, que es el Ingress Controller.
- **Ejemplo de uso:** Puedes definir dominios como `nginx.cefaslocalserver.com`, `grafana.cefaslocalserver.com`, etc., y que todos apunten internamente a `ingress.cefaslocalserver.com`.
- **IP real:** `10.17.5.30` (VIP flotante que apunta a los HAProxy que balancean hacia los Traefik pods dentro del clúster).
- **Útil para:**
  - Que todos los subdominios públicos o internos sean enrutados desde esa única IP al microservicio correspondiente vía Traefik.

### 3. Registro `k8s-api-lb`

- **Qué es:** El nodo físico/VM que actualmente posee la IP flotante del API Server (`10.17.5.10`).
- **Ejemplo de uso:** Si por alguna razón quieres conectar directamente con el nodo HAProxy sin pasar por el VIP, por ejemplo, para debug o health checks.
- **IP real:** `10.17.5.20` (IP fija del nodo HAProxy primario).
- **Útil para:**
  - Referencias directas al nodo balanceador "owner" o primario.
  - No es necesario en producción, pero puede ayudar en monitoreo, debug, scripts de failover.

### Conclusión de Registros DNS

| Registro DNS | Apunta a       | Uso principal                          |
|--------------|----------------|----------------------------------------|
| `k8s-api`    | VIP `10.17.5.10` | Unión de nodos y uso de `kubectl`.     |
| `ingress`    | VIP `10.17.5.30` | Tráfico web vía Traefik.              |
| `k8s-api-lb` | IP fija `10.17.5.20` | Nodo HAProxy primario (debug, monitoreo). |




```bash
sudo ansible-playbook -i inventory/hosts.ini regenerate_coredns.yml
```
