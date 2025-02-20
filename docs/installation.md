# Instalación de Suricata

Suricata se puede instalar en múltiples sistemas operativos. A continuación, se presentan los pasos para instalarlo en diferentes plataformas.

## Requisitos previos

Antes de instalar Suricata, asegúrate de tener los siguientes paquetes y dependencias:

- Un sistema basado en Linux (Debian, Ubuntu, CentOS, Fedora) o Windows.
- Acceso a un usuario con privilegios de administrador o `sudo`.
- Dependencias esenciales: `libpcre3`, `libpcre3-dev`, `libyaml-dev`, `libcap-ng-dev`, `libmagic-dev`, `libjansson-dev`, `libnetfilter-queue-dev`, `libnfnetlink-dev`.

## Instalación en Debian/Ubuntu

```sh
sudo apt update
sudo apt install -y suricata
```

Para verificar la instalación:

```sh
suricata --build-info
```

## Instalación en CentOS/Fedora

```sh
sudo dnf install -y suricata
```

O en CentOS con `yum`:

```sh
sudo yum install -y epel-release
sudo yum install -y suricata
```

## Instalación desde código fuente

Si necesitas la versión más reciente, puedes compilar Suricata manualmente:

```sh
sudo apt install -y build-essential libpcre3-dev libyaml-dev libcap-ng-dev libmagic-dev libjansson-dev libnetfilter-queue-dev libnfnetlink-dev
wget https://www.openinfosecfoundation.org/download/suricata-7.0.0.tar.gz
tar -xvzf suricata-7.0.0.tar.gz
cd suricata-7.0.0
./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
make
sudo make install
sudo ldconfig
```

Verifica la instalación ejecutando:

```sh
suricata --build-info
```

## Instalación en Windows

Suricata también se puede instalar en Windows con la versión precompilada disponible en su sitio web oficial: [https://suricata.io/download/](https://suricata.io/download/).

Una vez descargado el instalador, sigue los pasos del asistente de instalación.

## Configuración Post-instalación

Una vez instalado, puedes configurar Suricata editando su archivo de configuración ubicado en:

- **Linux:** `/etc/suricata/suricata.yaml`
- **Windows:** `C:\Program Files\Suricata\suricata.yaml`

Para probar la configuración:

```sh
suricata -T -c /etc/suricata/suricata.yaml
```

Si la configuración es válida, puedes iniciar Suricata con:

```sh
sudo systemctl start suricata
sudo systemctl enable suricata
```

## Actualización de Reglas

Para mantener las reglas actualizadas, utiliza `suricata-update`:

```sh
sudo suricata-update
sudo systemctl restart suricata
```

---

Con esto, Suricata estará instalado y listo para usarse. Para más configuraciones, revisa el archivo de configuración y ajusta según tus necesidades.
