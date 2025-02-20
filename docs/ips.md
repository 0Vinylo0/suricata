# Configuración de Suricata como IPS con nftables

Este documento explica cómo configurar Suricata en modo IPS usando `nftables` para bloquear tráfico malicioso en tiempo real.

---

## Configuración de Suricata como IPS
### Editar `suricata.yaml`
Modifica el archivo de configuración `suricata.yaml`, que se encuentra en `/etc/suricata/`, para asegurarte de que Suricata está configurado en modo IPS. Busca la sección `af-packet` y habilita el modo IPS:

```yaml
af-packet:
  - interface: eth0
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
    bypass: yes
    copy-mode: ips
    copy-iface: eth1
```

Guarda los cambios y reinicia Suricata:
```bash
sudo systemctl restart suricata
```

---

## Configuración de nftables
Para que Suricata pueda inspeccionar y bloquear tráfico, configuraremos `nftables` para redirigir el tráfico a la cola de Suricata.

### Crear la tabla y reglas en nftables
Ejecuta los siguientes comandos para configurar `nftables`:

```bash
# Crear una tabla llamada 'filter'
nft add table inet filter

# Añadir una cadena para el firewall
nft add chain inet filter firewall '{ type filter hook forward priority 0; policy accept; }'

# Añadir una cadena para Suricata IPS
nft add chain inet filter suricata '{ type filter hook forward priority 10; }'

# Enviar tráfico a Suricata para su inspección
nft add rule inet filter suricata queue
```

Para que estas reglas persistan tras un reinicio, guarda la configuración:
```bash
nft list ruleset > /etc/nftables.conf
```

Reinicia `nftables` para aplicar los cambios:
```bash
sudo systemctl restart nftables
```

---

## Verificación
Para comprobar que Suricata está inspeccionando el tráfico, usa:
```bash
tail -f /var/log/suricata/fast.log
```
Si Suricata está bloqueando tráfico correctamente, deberías ver registros en este archivo.

También puedes probar con `ping` y `curl` desde otro equipo para verificar que las reglas están funcionando.

---

## Recursos adicionales
- [Documentación oficial de Suricata](https://suricata.io/documentation/)
- [Guía de nftables en Linux](https://wiki.nftables.org/)
- [Foro de discusión sobre Suricata](https://forum.suricata.io/)

