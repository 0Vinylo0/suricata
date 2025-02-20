# Configuración de Suricata

Suricata utiliza un archivo de configuración principal llamado `suricata.yaml`, que define su comportamiento y parámetros de ejecución. En este documento se explica cómo configurarlo adecuadamente.

## Ubicación del Archivo de Configuración

- **Linux:** `/etc/suricata/suricata.yaml`
- **Windows:** `C:\Program Files\Suricata\suricata.yaml`

Para verificar la configuración antes de iniciar Suricata, ejecuta:

```sh
suricata -T -c /etc/suricata/suricata.yaml
```

## Parámetros Clave de Configuración

### 1. Interfaz de Red

Define la interfaz de red en la que Suricata monitoreará el tráfico:

```yaml
vars:
  address-groups:
    HOME_NET: "[192.168.1.0/24]"
```

Para capturar tráfico en una interfaz específica, ajusta:

```yaml
afp:
  interface: eth0
```

### 2. Modo de Ejecución

Suricata puede ejecutarse en modo IDS (detección) o IPS (prevención). Para IDS:

```yaml
outputs:
  - fast:
      enabled: yes
```

Para habilitar el modo IPS:

```sh
sudo suricata -c /etc/suricata/suricata.yaml -q 0
```

### 3. Configuración de Registros

Suricata genera varios tipos de registros, incluidos alertas y tráfico capturado. Se configuran en:

```yaml
outputs:
  - eve-log:
      enabled: yes
      types:
        - alert
        - http
        - dns
        - tls
```

Los registros se almacenan por defecto en `/var/log/suricata/`.

### 4. Habilitar GeoIP

Para habilitar la geolocalización de IPs, instala `geoipupdate` y configura:

```yaml
geoip:
  enabled: yes
  db-dir: /usr/share/GeoIP/
```

### 5. Uso de Hilos y CPU

Para optimizar el rendimiento en sistemas con múltiples núcleos:

```yaml
threading:
  set-cpu-affinity: yes
  cpu-affinity:
    - management-cpu-set:
        cpu: [ 0 ]
    - worker-cpu-set:
        cpu: [ 1, 2, 3 ]
```

### 6. Activar TLS Inspection

Si deseas inspeccionar tráfico cifrado TLS:

```yaml
tls:
  enabled: yes
  certs-dir: /etc/suricata/certs/
```

### 7. Configuración de Reglas

Para usar reglas personalizadas, edita la sección:

```yaml
rule-files:
  - /etc/suricata/rules/custom.rules
```

Para actualizar reglas automáticamente:

```sh
sudo suricata-update
sudo systemctl restart suricata
```

## Reiniciar Suricata tras Cambios

Cada vez que se realicen cambios en la configuración, es recomendable reiniciar el servicio:

```sh
sudo systemctl restart suricata
```

---

Con esta configuración, Suricata estará optimizado y listo para operar de manera eficiente en tu red.
