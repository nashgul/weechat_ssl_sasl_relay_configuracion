# Configuración de weechat con: SSL, SASL y RELAY server

Instalación de weechat en centOs 7 y debian, uso de SSL para conectarse a freenode, SASL para identificarse en freenode y  configuración del servidor relay y los clientes

## Compilar weechat

Para poder usar SASL es necesario que weechat tenga el mecanismo de autenticación SASL 'ecdsa-nist256p-challenge', para eso es necesario compilarlo y que el paquete 'libgnutls28-dev' (gnutls-devel en centOs).

### Dependencias de weechat en Debian

Con el siguiente comando se deben instalar todas las dependencias necesarias para una correcta compilación:
- # apt-get install cmake pkg-config libncursesw5-dev libcurl4-gnutls-dev zlib1g-dev libgcrypt20-dev libgnutls28-dev gettext ca-certificates libaspell-dev python-dev libperl-dev ruby2.1-dev liblua5.2-dev tcl-dev guile-2.0-dev libv8-dev asciidoc source-highlight xsltproc docbook-xml docbook-xsl libcpputest-dev autotools-dev dh-autoreconf

### Dependencias de weechat para centOs 7
- # yum groupinstall "Development Tools"
- # yum install gnutls-devel ncurses-devel gettext-devel gnutls-devel aspell-devel python-devel perl-devel ruby-devel lua-devel tcl-devel guile-devel mod_perl-devel

### Descargar weechat

- $ git clone https://github.com/weechat/weechat.git

### Instalar weechat

- $ ./autogen.sh
- $ mkdir build
- $ cd build
- $ ../configure
- $ make
- # make install


