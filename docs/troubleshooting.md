# Solución de Problemas en Suricata

Este documento cubre problemas comunes en Suricata y sus soluciones.

## 1. Verificar Configuración

Si Suricata no inicia correctamente, revisa la configuración con:

```sh
suricata -T -c /etc/suricata/suricata.yaml
```

Si hay errores, revisa la línea reportada y corrige el problema.

## 2. Logs de Errores

Si Suricata no funciona como se espera, revisa los logs:

```sh
tail -f /var/log/suricata/suricata.log
```

Para eventos en formato JSON:

```sh
tail -f /var/log/suricata/eve.json
```

## 3. Problema: Suricata No Captura Tráfico

### Solución 1: Verificar Interfaz Correcta

Ejecuta:

```sh
ip link show
```

Asegúrate de que Suricata esté escuchando en la interfaz correcta.

Para especificar la interfaz:

```sh
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

### Solución 2: Verificar Reglas Activas

Para comprobar si las reglas están cargadas:

```sh
sudo suricata --list-keywords
```

Si no hay reglas activas, revisa `suricata.yaml` y habilita los archivos de reglas.

## 4. Problema: Suricata Consume Demasiados Recursos

### Solución: Optimizar Uso de CPU y Memoria

Edita `suricata.yaml` y ajusta la sección de `threading`:

```yaml
threading:
  set-cpu-affinity: yes
  cpu-affinity:
    - management-cpu-set:
        cpu: [ 0 ]
    - worker-cpu-set:
        cpu: [ 1, 2, 3 ]
```

También puedes reducir la cantidad de eventos registrados en `eve.json`.

## 5. Problema: No Se Generan Alertas

### Solución: Comprobar Archivos de Reglas

Verifica que las reglas están activas:

```sh
cat /etc/suricata/rules/*.rules | grep -i alert
```

Si no hay alertas, intenta generar tráfico de prueba con:

```sh
curl http://testmyids.com
```

Debería activarse una alerta en `fast.log`.

## 6. Problema: Error con Suricata-Update

Si al actualizar las reglas aparece un error, intenta:

```sh
sudo suricata-update
sudo systemctl restart suricata
```

Si el problema persiste, revisa si el servicio tiene permisos para acceder a la red.

## 7. Reiniciar Suricata tras Cambios

Cada vez que hagas cambios en las reglas o configuración, reinicia el servicio:

```sh
sudo systemctl restart suricata
```

Para verificar el estado del servicio:

```sh
sudo systemctl status suricata
```

---

Siguiendo estas soluciones, puedes diagnosticar y corregir problemas comunes en Suricata.
