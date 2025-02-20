# Creación de Reglas en Suricata

Suricata utiliza reglas para detectar y responder a eventos de seguridad en la red. Estas reglas están basadas en patrones de tráfico y pueden configurarse para generar alertas o bloquear paquetes maliciosos.

## Ubicación de las Reglas

Las reglas de Suricata se encuentran en:

- **Linux:** `/etc/suricata/rules/`
- **Windows:** `C:\Program Files\Suricata\rules\`

Para definir reglas personalizadas, edita el archivo:

```sh
/etc/suricata/rules/custom.rules
```

## Estructura de una Regla

Una regla de Suricata sigue la siguiente estructura:

```sh
action proto src_ip src_port direction dst_ip dst_port (options)
```

Ejemplo de regla para detectar tráfico HTTP sospechoso:

```sh
alert http any any -> any any (msg:"Acceso a sitio sospechoso"; content:"badwebsite.com"; sid:100001; rev:1;)
```

### Explicación:
- `alert`: Acción que genera una alerta.
- `http`: Protocolo a monitorear.
- `any any -> any any`: Aplica la regla a cualquier IP y puerto.
- `msg`: Mensaje de la alerta.
- `content`: Patrón de contenido a detectar.
- `sid`: Identificador único de la regla.
- `rev`: Número de revisión de la regla.

## Tipos de Acciones

Las reglas de Suricata pueden tomar diferentes acciones:

- `alert`: Genera una alerta.
- `drop`: Bloquea el paquete y no lo envía.
- `reject`: Bloquea el paquete y envía una respuesta.
- `pass`: Omite el tráfico especificado.

Ejemplo de regla para bloquear tráfico de una IP específica:

```sh
drop ip 192.168.1.100 any -> any any (msg:"IP bloqueada"; sid:100002; rev:1;)
```

## Uso de Variables

Suricata permite definir variables para facilitar la gestión de reglas. Estas se configuran en `suricata.yaml`.

Ejemplo de variable para definir la red interna:

```yaml
vars:
  address-groups:
    HOME_NET: "[192.168.1.0/24]"
```

Regla utilizando la variable:

```sh
alert tcp $HOME_NET any -> any any (msg:"Tráfico sospechoso desde red interna"; content:"malicious"; sid:100003; rev:1;)
```

## Habilitar y Deshabilitar Reglas

Para activar reglas personalizadas, edita `suricata.yaml` y asegúrate de incluir:

```yaml
rule-files:
  - /etc/suricata/rules/custom.rules
```

Si deseas deshabilitar reglas específicas, usa:

```sh
sudo suricata-update disable-rule 100003
```

## Aplicar y Probar Reglas

Después de modificar o agregar reglas, prueba su validez con:

```sh
suricata -T -c /etc/suricata/suricata.yaml
```

Reinicia Suricata para aplicar los cambios:

```sh
sudo systemctl restart suricata
```

---

Con estas reglas personalizadas, Suricata puede detectar y mitigar amenazas en la red de manera efectiva.
