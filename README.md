# README.md

## Instalación de Ansible en Rocky Linux 9.4

Para instalar Ansible en tu sistema Rocky Linux 9.4, sigue estos pasos:

1. **Agregar el repositorio de EPEL** (Extra Packages for Enterprise Linux), ya que Ansible suele estar disponible en este repositorio:

    ```bash
    sudo dnf install epel-release -y
    ```

2. **Instalar Ansible**:

    ```bash
    sudo dnf install ansible -y
    ```

3. **Verificar la instalación**:

    ```bash
    ansible --version
    ```

4. **Clonar el repositorio**:

    ```bash
    sudo git clone https://github.com/vhgalvez/ansible-freeipa-dns-setup-rockylinux.git
    ```

Esto instalará Ansible y lo preparará para ejecutar playbooks en tu sistema.

## Ejecutar un Playbook de Ansible en un Servidor Remoto usando SSH

A continuación, se muestra una guía paso a paso para ejecutar un playbook de Ansible en tu servidor remoto usando SSH:

### Paso 1: Verificar la Conectividad con SSH

Primero, asegúrate de que puedes conectarte al servidor remoto con el siguiente comando SSH:

```bash
sudo ssh -i /root/.ssh/cluster_openshift/key_cluster_openshift/id_rsa_key_cluster_openshift core@10.17.3.11 -p 22
```

Si esta conexión funciona, tu clave SSH está configurada correctamente y podrás conectarte desde Ansible.

### Paso 2: Configurar el Inventario de Ansible

Crea un archivo de inventario para definir el servidor remoto. Puedes nombrarlo `inventory.ini` y añadir el siguiente contenido:

```ini
[freeipa_servers]
10.17.3.11 ansible_user=core ansible_ssh_private_key_file=/root/.ssh/cluster_openshift/key_cluster_openshift/id_rsa_key_cluster_openshift ansible_port=22
```

Este archivo indica a Ansible que se conecte al servidor `10.17.3.11` como usuario `core`, usando la clave SSH especificada.

### Paso 3: Crear el Playbook de Ansible

Guarda el playbook YAML que compartiste en un archivo, por ejemplo, `freeipa_setup.yml`.

### Paso 4: Ejecutar el Playbook de Ansible

Ejecuta el playbook con el siguiente comando, especificando el archivo de inventario:

```bash
sudo ansible-playbook -i inventory.ini freeipa_setup.yml
```

### Paso 5: Monitorear la Ejecución

Ansible mostrará el progreso de cada tarea en el playbook. Si alguna tarea falla, revisa los mensajes de error para diagnosticar el problema.

## Resumen

1. Conectar por SSH para verificar el acceso.
2. Crear el archivo de inventario (`inventory.ini`).
3. Guardar el playbook como `freeipa_setup.yml`.
4. Ejecutar el playbook con `ansible-playbook`.

Con estos pasos, Ansible debería aplicar todas las configuraciones especificadas en el playbook en tu servidor remoto.
