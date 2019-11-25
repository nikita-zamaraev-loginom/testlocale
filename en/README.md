# testlocale

# Install Loginom Server

## MSI Installation

Options for the installer file name:

- LoginomTeam_6.xxmsi - installer for Team editorial
- LoginomStandard_6.xxmsi - installer for Standard edition
- LoginomEnterprise_6.xxmsi - installer for Enterprise edition

where 6.xx are numbers indicating the version and release of the program.

To work with the [Loginom Studio](../studio/README.md) program (web interface to Loginom Server), a web server is a prerequisite.

The following web server options are acceptable:

- **Apache** - standard installation, the web server is included in the distribution package;
- **Microsoft IIS** - is not part of the distribution, it must be [installed and configured](./iis.md) separately.

Installing Loginom Server using Apache or Microsoft IIS is different. When installed by default, Loginom Studio runs through the Apache web server.

### GUI

#### Installer launch

To install with non-standard parameters in the **Installation Type** dialog **,** click the **Custom** button.

#### Selecting components to install

- **Loginom Server is** required to install *Loginom server* .
- **Loginom Studio** installs the web interface.
- **The documentation** adds a platform help section to the web-based interface.
- **The web server** installs the Apache Httpd embedded web server. If you intend to use a third-party web server, the component must be disabled.

![](../../images/server_msi_features_default.png)

> **Note:** By default, all components are installed.

#### Loginom Server Settings

This dialog is available only when installing the *Loginom Server* component.

![](../../images/server_msi_server.png)

- **Server Port** - defines the port number for connecting Loginom Integrator and BatchLauncher.
- **WS port** - defines the port number for Loginom Studio connection via websocket (ws) protocol.
- **Use WebSocket secure** - includes the ability to set ssl parameters for websocket:
    -  **WSS port** - determines the port number for Loginom Studio connection using websocket secure (wss) protocol. 
    -  **SSL Certificate, SSL** **Key** - full paths to the *pem* certificate and SSL key files. 
- **Install Guardant x64 drivers** - starts the automatic installation of drivers for the electronic security key to work.

#### Apache Httpd Web Server Settings

The dialog is available only when installing the *Web server* component.

![](../../images/server_msi_httpd.png)

- **HTTP port** - defines the http number of the web server port.
- **Use WebSocket proxy** - enables proxying websocket connections.
- **Use HTTPS** - includes the ability to set ssl parameters for http:
    -  **HTTPS port** - defines the https port number of the web server. 
    -  **SSL Certificate, SSL** **Key** - full paths to the *pem* certificate and SSL key files. 

#### Loginom Studio Options

The dialog is available only when installing the Loginom Studio component without installing the Web server component.

![](../../images/server_msi_studio.png)

- **Web Application Directory**
- **Web application url**
- **Connection to the Loginom server** - contains parameters for generating [the Loginom Studio configuration file](../studio/config.md)
    -  **Host** - defines the value of the [host](../studio/config.md#host) parameter. If the value of the field is equal to the host value of the URL field of the *web application* , `host: null` specified in `server.json` . 
    -  **WS port** and **WSS port** - determine the values of the [wsport](../studio/config.md#wsport) and [wssport parameters,](../studio/config.md#wssport) respectively. 
    -  **Always use secure connection** - enables the [secure](../studio/config.md#secure) parameter. 
    -  **Use WebSocket proxy** - enables the [wsproxy option](../studio/config.md#wsproxy) . 
    -  **Proxy URI** - Defines the [path](../studio/config.md#path) parameter. 

### Command line

```cmd
msiexec /i "путь_к_msi_файлу" ключи_msi параметры_loginom
```

- `ключи_msi` - valid values can be found by running `msiexec /?` . Particularly useful may be:
    -  `/l* "%TEMP%\loginom.msi.log"` - enable installation logging. 
    -  `/qn` - silent installation without displaying a graphical interface. 

% spoiler% Loginom_parameters as `КЛЮЧ=значение` % spoiler%

Key | Default value | Description
--- | --- | ---
ADDLOCAL | `Server,Studio,Help,Webserver` | List of installed components
INSTALLDIR | `%ProgramFiles%\BaseGroup` | Installation directory
SERVER_WS_PORT | `8080` | Loginbs server loginom port
SERVER_WSS_PORT | `8443` | Websocket secure port of Loginom server
SERVER_PORT | `4580` | Loginom Server Port
SERVER_USE_SSL | `0` | Use encryption for websocket ( `0,1` )
SERVER_KEY_PATH |  | SSL key path
SERVER_CRT_PATH |  | SSL certificate path
WEBSRV_PORT | `80` | HTTP port of the web server
WEBSRV_PORT_SSL | `443` | HTTPS web server port
WEBSRV_USE_SSL | `0` | Use encryption (https) ( `0,1` )
WEBSRV_KEY_PATH |  | SSL key path
WEBSRV_CRT_PATH |  | SSL certificate path
WEBAPP_URI | `app/` | Loginom Studio web URI
WEBSRV_USE_WS_PROXY | `0` | Use websocket proxy ( `0,1` )
WEBSRV_PROXY_URI | `ws/` | URI websocket proxy

% / spoiler%

% spoiler% Examples% spoiler%

```cmd
:: "тихая" установка Loginom без веб-сервера
msiexec /i ".\LoginomEnterprise.msi" /qn ADDLOCAL=Server,Studio,Help

:: "тихая" установка с нестандартными портами
msiexec /i ".\LoginomEnterprise.msi" /qn WEBSRV_PORT=9080 SERVER_WS_PORT=9081 SERVER_PORT=9082

:: "тихая" установка с включением SSL для websocket
msiexec /i ".\LoginomEnterprise.msi" /qn SERVER_USE_SSL=1 SERVER_KEY_PATH="%ALLUSERSPROFILE%\ssl.key" SERVER_CRT_PATH="%ALLUSERSPROFILE%\ssl.crt"
```

% / spoiler%

## Licenses

To start the **LoginomServer** service, **you** need to configure licensing keys (see [License keys](../licenses/README.md) ).

When using the network key server, you need to create the [GnClient.ini](https://dev.guardant.ru/pages/viewpage.action?pageId=1277980) file in the directory `"C:\ProgramData\BaseGroup\Loginom 6\Server"`

## Service Launch

The LoginomServer and LoginomHttpd services do not start automatically after installation.

When installing the product, the Windows Start menu ( `Все программы\Loginom 6\Server` ) shortcuts **"Launch Loginom Server"** and **"Launch Web Server"** are added.

To start services using the shortcut, you need to select the option **"Run as administrator"** in its context menu. Upon successful launch, the `Служба "LoginomServer" успешно запущена` starts up successfully in the console window.

To start services from the command line, you must run the following commands as administrator:

- Starting the service "LoginomServer": `net start loginom`
- Starting the service "LoginomHttpd": `net start httpd_loginom`

## Firewall

The host with the Loginom server must allow connections:

- Incoming on TCP port `8080|8443` (websocket) for client hosts (browsers)
    -  when using websocket proxy - only for the web server host. 
- Incoming on TCP port `4580` for hosts with **Loginom Intergator** or **BatchLauncher** .
- When using Guardant network keys - on TCP / UDP ports `3186` and `3187` the network key server host.

The host with the web server must allow connections:

- On TCP port `80|443` (http (s)) for client hosts (browsers)

Make sure that installed antivirus and firewalls allow these connections.

% spoiler% Windows Firewal setup examples:% spoiler%

- Allow connections to the http port of the web server

```cmd
netsh advfirewall firewall add rule name="Allow HTTP" dir=in action=allow protocol=TCP localport=80
```

- Allow connections to Loginom Server websocket

```cmd
netsh advfirewall firewall add rule name="Allow HTTP" dir=in action=allow protocol=TCP localport=80
```

- Allow connections to Loginom Server for Integrator

```cmd
netsh advfirewall firewall add rule name="Allow Loginom Port" dir=in action=allow protocol=TCP localport=4850 remoteip=%lgi_host_ip%
```

- Allow connections to the network key server for Loginom Server

```cmd
netsh advfirewall firewall add rule name="Allow Guardant Net" dir=out action=allow protocol=TCP remoteport=3186-3187 remoteip=%guardant_net_host_ip%
```

% / spoiler%

## Accounts

When installed in Loginom Server, user accounts are created:

- `user` with an empty password - for working with Loginom scripts
- `admin` password `admin` - server for administration Loginom
- `service` with the password `service` - for connecting external applications (Integrator and BatchLauncher)

> **Note:** After installation is complete, you must change the password for the administrator account (admin).

## Health Check

- Launch Loginom Studio web client
    -  shortcut `Все программы\Loginom 6\Studio\Loginom Studio` from the Windows Start Menu 
    -  either in a browser (Chrome recommended) at the URL: `http://localhost/app/` . If the port has been changed in the web server settings, then the URL: `http://localhost:<Порт HTTP>/app/` 
- To log in with an account `user` with a blank password.
