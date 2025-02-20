# Ejemplos de Uso de Suricata

A continuación, se presentan varios ejemplos prácticos de cómo utilizar Suricata para la detección de amenazas y análisis de tráfico en diferentes escenarios.

## 1. Detección de Escaneo de Puertos

Para detectar intentos de escaneo de puertos en la red, puedes usar la siguiente regla personalizada:

```sh
alert tcp any any -> any any (msg:"Posible escaneo de puertos detectado"; flags:S; threshold:type both, track by_src, count 10, seconds 60; sid:100001; rev:1;)
```

Esta regla genera una alerta si una IP intenta conectarse a 10 puertos diferentes en un intervalo de 60 segundos.

## 2. Bloqueo de una IP Maliciosa

Si identificas una IP sospechosa, puedes bloquear su tráfico con la siguiente regla:

```sh
drop ip 203.0.113.45 any -> any any (msg:"IP maliciosa bloqueada"; sid:100002; rev:1;)
```

Esta regla previene que la IP `203.0.113.45` se comunique con la red.

## 3. Monitoreo de Tráfico HTTP

Para registrar solicitudes HTTP sospechosas, puedes emplear esta regla:

```sh
alert http any any -> any any (msg:"Posible acceso a sitio no autorizado"; content:"evil.com"; http_host; sid:100003; rev:1;)
```

Detecta cuando un usuario intenta acceder a `evil.com`.

## 4. Captura de Todo el Tráfico en un Puerto Específico

Para capturar todo el tráfico en el puerto 443 (HTTPS):

```sh
suricata -c /etc/suricata/suricata.yaml -i eth0 -v "port 443"
```

## 5. Uso de Suricata como IDS en Tiempo Real

Para monitorear el tráfico en una interfaz específica y registrar eventos en `eve.json`:

```sh
suricata -c /etc/suricata/suricata.yaml -i eth0 --af-packet
```

## 6. Análisis de un Archivo PCAP

Si tienes un archivo de captura de tráfico y deseas analizarlo:

```sh
suricata -r captura.pcap -c /etc/suricata/suricata.yaml -l ./logs/
```

## 7. Detección de Tráfico DNS Sospechoso

Si deseas detectar consultas DNS a dominios sospechosos:

```sh
alert dns any any -> any any (msg:"Consulta DNS sospechosa"; content:"malwaredomain.com"; nocase; sid:100004; rev:1;)
```

## 8. Filtrar Eventos en `eve.json`

Para mostrar solo eventos de alerta:

```sh
jq 'select(.event_type == "alert")' /var/log/suricata/eve.json
```

## 9. Actualización de Reglas y Reinicio del Servicio

Para mantener las reglas actualizadas y aplicar los cambios:

```sh
sudo suricata-update
sudo systemctl restart suricata
```

---

Estos ejemplos te ayudarán a aprovechar Suricata para la detección de amenazas y el monitoreo de tráfico en redes empresariales o domésticas.
