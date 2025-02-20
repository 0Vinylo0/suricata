# Análisis de Tráfico con Suricata

Suricata permite capturar, analizar y registrar el tráfico de red en tiempo real, facilitando la detección de amenazas y actividades sospechosas.

## Modos de Captura

Suricata puede operar en diferentes modos para analizar el tráfico:

### 1. Modo PCAP (Offline)
Permite analizar archivos de captura de tráfico (`.pcap`):

```sh
suricata -r captura.pcap -c /etc/suricata/suricata.yaml -l ./logs/
```

Este modo es útil para la investigación de incidentes y análisis forense.

### 2. Modo en Vivo (Live Capture)
Para capturar tráfico en tiempo real en una interfaz de red específica:

```sh
suricata -c /etc/suricata/suricata.yaml -i eth0
```

Esto permite a Suricata analizar y generar alertas en tiempo real según las reglas configuradas.

## Tipos de Registros

Suricata genera diferentes tipos de registros en el directorio `/var/log/suricata/`:

- **fast.log**: Alertas resumidas.
- **eve.json**: Eventos detallados en formato JSON.
- **stats.log**: Estadísticas de tráfico.
- **http.log**: Información sobre tráfico HTTP.
- **dns.log**: Consultas y respuestas DNS.

Para monitorear las alertas en tiempo real:

```sh
tail -f /var/log/suricata/fast.log
```

## Filtrado de Tráfico

Para capturar solo tráfico HTTP:

```sh
suricata -c /etc/suricata/suricata.yaml -i eth0 -v "tcp port 80"
```

También puedes utilizar expresiones de captura más complejas con `bpf`:

```sh
suricata -c /etc/suricata/suricata.yaml -i eth0 -v "src net 192.168.1.0/24"
```

## Análisis con Suricata-Update

Para obtener reglas actualizadas y mejorar la detección de amenazas:

```sh
sudo suricata-update
sudo systemctl restart suricata
```

## Integración con SIEM

Suricata se integra con herramientas SIEM como Elasticsearch y Kibana para visualización avanzada.

Ejemplo de configuración en `suricata.yaml` para salida JSON:

```yaml
outputs:
  - eve-log:
      enabled: yes
      types:
        - alert
        - http
        - dns
      filename: eve.json
```

Luego, los datos pueden enviarse a un SIEM para correlación y análisis visual.

## Comprobación de Rendimiento

Para evaluar el rendimiento de Suricata en la red, usa:

```sh
suricata -c /etc/suricata/suricata.yaml --dump-config
```

Para obtener estadísticas de captura de tráfico:

```sh
tail -f /var/log/suricata/stats.log
```

---

Con estas herramientas, puedes utilizar Suricata para monitorear y analizar el tráfico en profundidad, identificando posibles amenazas y optimizando la seguridad de la red.
