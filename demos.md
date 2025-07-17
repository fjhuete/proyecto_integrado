# Abrir sesión en terminal

```bash
screen -S demo
```

```bash
screen -x demo
```

# Carga de datos a la aplicación

## API

### Crear un dispositivo usando la API

```bash
curl -X POST \
  'https://proyecto.javihuete.site/api/dcim/devices/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Token 17578e115ef0e83cd0598b12b74fc6da3bc97f18' \
  -d '{
    "name": "Kyurem",
    "device_type": 101,
    "role": 4,
    "tenant": null,
    "platform": 2,
    "serial": "",
    "asset_tag": null,
    "site": 1,
    "location": 1,
    "rack": null,
    "position": null,
    "face": null,
    "status": "active",
    "airflow": null,
    "description": "",
    "comments": "",
    "tags": [],
    "custom_fields": {}
  }'
```

## Ansible

```bash
git clone https://github.com/netbox-community/netbox-zero-to-hero.git
cd netbox-zero-to-hero/ansible
python3 -m venv .
source bin/activate
python3 -m pip install --upgrade pip
pip3 install pynetbox
pip3 install ansible
pip3 install netaddr
pip3 install pytz
ansible-galaxy collection install netbox.netbox
export NETBOX_API='https://proyecto.javihuete.site'
export NETBOX_TOKEN=17578e115ef0e83cd0598b12b74fc6da3bc97f18
```

```yaml
---
ip_addresses:

  - assigned_object:
      device: Victini
      name: GigabitEthernet1
    address: 192.168.0.1/25
    status: Active

  - assigned_object:
      device: Reshiram
      name: Gig-E 1
    address: 192.168.0.2/25
    status: Active

  - assigned_object:
      device: Zekrom
      name: Gig-E 1
    address: 192.168.0.3/25
    status: Active

  - assigned_object:
      device: Keldeo
      name: Gig-E 1
    address: 192.168.0.4/25
    status: Active

  - assigned_object:
      device: Reshiram
      name: Gig-E 2
    address: 172.22.100.16/25
    status: Reserved

  - assigned_object:
      device: Kyurem
      name: Gig-E 1
    address: 192.168.0.5/25
    status: Reserved
```

```bash
ansible-playbook populate_netbox_ipam.yml
```

```yaml
---
ip_addresses:

  - assigned_object:
      device: Victini
      name: GigabitEthernet1
    address: 192.168.0.1/25
    status: Active

  - assigned_object:
      device: Reshiram
      name: Gig-E 1
    address: 192.168.0.2/25
    status: Active

  - assigned_object:
      device: Zekrom
      name: Gig-E 1
    address: 192.168.0.3/25
    status: Active

  - assigned_object:
      device: Keldeo
      name: Gig-E 1
    address: 192.168.0.4/25
    status: Active

  - assigned_object:
      device: Reshiram
      name: Gig-E 2
    address: 172.22.100.16/25
    status: Active

 - assigned_object:
      device: Kyurem
      name: Gig-E 1
    address: 192.168.0.5/25
    status: Active
```

# Configuración de Caddy a través de la API

## Ver la configuración actual

```bash
curl localhost:2019/config/ | jq
```

## Modificar la configuración

### Modificar la IP del backend

#### Agregar configuración desde un fichero JSON

```bash
cat caddyconfig.json
```

```bash
curl localhost:2019/load \
	-H "Content-Type: application/json" \
	-d @caddyconfig.json
```

#### Ver sólo la dirección IP de un backend

```bash
curl localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/ | jq

curl localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/0/dial/

curl localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/1/dial/
```

#### Modificar sólo la dirección IP de un backend

```bash
curl \
	localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/1/dial/ \
	-H "Content-Type: application/json" \
	-d '"172.22.201.219:80"'
```

#### Crear un ID para acceder directamente a esta configuración

```bash
curl \
	localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/0/@id \
	-H "Content-Type: application/json" \
	-d '"backend1"'

curl \
	localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/1/@id \
	-H "Content-Type: application/json" \
	-d '"backend2"'

curl localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams/ | jq
```

#### Modificar la IP de un backend usando el ID

```bash
curl \
	localhost:2019/id/backend2 \
	-H "Content-Type: application/json" \
	-d '{"@id":"backend2","dial":"172.22.200.202:80"}'
```

#### Ver los cambios

```bash
curl localhost:2019/id/backend2/
```

### Ventajas del uso de la API: automatizar los cambios en la configuración

```bash
cat lb_policy.sh

bash lb_policy.sh random
```

## Funcionamiento del health check

### Ver el estado de salud de los backends

```bash
curl "localhost:2019/metrics" | grep caddy_reverse_proxy_upstreams_healthy
```

### Revisar el estado de salud de los backends con un script

```bash
cat salud.sh

bash salud.sh
```
