Configuración de CoreDNS en AlmaLinux con Ansible
Este repositorio contiene playbooks de Ansible para instalar y configurar CoreDNS como servidor DNS en servidores con AlmaLinux.

Descripción
Este proyecto automatiza la instalación y configuración de CoreDNS para gestionar las solicitudes DNS en tu infraestructura. CoreDNS es un servidor DNS flexible y extensible que es ideal para ambientes de contenedores y redes internas.

Los playbooks incluyen:

Instalación de CoreDNS.

Configuración de CoreDNS como servidor DNS local.

Gestión de puertos y servicios en el firewall.

Configuración de registros DNS para tus servidores y servicios.

Requisitos Previos
Antes de ejecutar estos playbooks, asegúrate de que:

Tienes un servidor AlmaLinux 9 configurado.

Tienes Ansible instalado en tu máquina local o servidor de administración.

El servidor AlmaLinux tiene acceso SSH habilitado para que Ansible pueda conectarse y realizar configuraciones.

Instalación de Ansible en AlmaLinux 9
Para instalar Ansible en AlmaLinux 9, sigue estos pasos:

Instalar EPEL (Extra Packages for Enterprise Linux):

bash
Copiar
Editar
sudo dnf install epel-release -y
Instalar Ansible:

bash
Copiar
Editar
sudo dnf install ansible -y
Verificar que Ansible está correctamente instalado:

bash
Copiar
Editar
ansible --version
Clonar el repositorio de Ansible:

bash
Copiar
Editar
git clone https://github.com/vhgalvez/ansible-coredns-setup-almalinux.git
Ejecutar un Playbook de Ansible en un Servidor Remoto
Paso 1: Verificar la Conectividad SSH
Verifica que puedes conectarte al servidor AlmaLinux usando SSH:

bash
Copiar
Editar
sudo ssh -i /ruta/a/tu/clave/privada user@<ip-del-servidor> -p 22
Si la conexión es exitosa, puedes proceder con la configuración desde Ansible.

Paso 2: Configurar el Inventario de Ansible
Crea un archivo de inventario (inventory.ini) para definir el servidor remoto:

ini
Copiar
Editar
[freeipa_servers]
10.17.3.11 ansible_user=core ansible_ssh_private_key_file=/ruta/a/tu/clave/privada ansible_port=22
Paso 3: Crear el Playbook de Ansible
Guarda el playbook en un archivo llamado coredns_setup.yml.

Paso 4: Ejecutar el Playbook
Ejecuta el playbook de la siguiente manera:

bash
Copiar
Editar
sudo ansible-playbook -i inventory.ini coredns_setup.yml
Paso 5: Verificar la Configuración
Una vez que el playbook haya terminado, puedes verificar la configuración de CoreDNS con los siguientes comandos:

bash
Copiar
Editar
sudo systemctl status coredns
Verifica la resolución DNS:

bash
Copiar
Editar
dig google.com
dig 10.17.3.11
Paso 6: Verificar el Estado del Servicio DNS
Verifica el estado del servicio DNS en CoreDNS:

bash
Copiar
Editar
sudo systemctl enable coredns
sudo systemctl start coredns
sudo systemctl status coredns
Verifica que el firewall esté configurado correctamente para permitir el tráfico en el puerto 53:

bash
Copiar
Editar
sudo firewall-cmd --list-all
Resumen de Recursos para Máquinas Virtuales
Nombre de VM	CPU	Memoria (MB)	IP	Nombre de Dominio	Tamaño de Disco (GB)	Hostname
master1	2	4096	10.17.4.21	master1.cefaslocalserver.com	50	master1
master2	2	4096	10.17.4.22	master2.cefaslocalserver.com	50	master2
master3	2	4096	10.17.4.23	master3.cefaslocalserver.com	50	master3
worker1	2	4096	10.17.4.24	worker1.cefaslocalserver.com	50	worker1
worker2	2	4096	10.17.4.25	worker2.cefaslocalserver.com	50	worker2
worker3	2	4096	10.17.4.26	worker3.cefaslocalserver.com	50	worker3
bootstrap	2	4096	10.17.4.27	bootstrap.cefaslocalserver.com	50	bootstrap
freeipa1	2	2048	10.17.3.11	freeipa1.cefaslocalserver.com	32	freeipa1
loadbalancer1	2	2048	10.17.3.12	loadbalancer1.cefaslocalserver.com	32	loadbalancer1
postgresql1	2	2048	10.17.3.13	postgresql1.cefaslocalserver.com	32	postgresql1
helper	2	2048	10.17.3.14	helper.cefaslocalserver.com	32	helper_node
Resumen
Verificar la conectividad SSH.

Crear el archivo de inventario (inventory.ini).

Guardar el playbook como coredns_setup.yml.

Ejecutar el playbook con ansible-playbook.

Con estos pasos, Ansible debería aplicar todas las configuraciones especificadas en el playbook en tu servidor remoto.

Licencia
Este proyecto está licenciado bajo la Licencia MIT.

Autor
Victor Galvez https://github.com/vhgalvez

Este README.md ahora refleja el uso de AlmaLinux en lugar de Rocky Linux y proporciona instrucciones claras sobre la instalación y ejecución del playbook para configurar CoreDNS.