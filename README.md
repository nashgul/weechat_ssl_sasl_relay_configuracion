# Configuración de weechat con: SSL, SASL y RELAY server

Instalación de weechat en centOs 7 y debian, uso de SSL para conectarse a freenode, SASL para identificarse en freenode y configuración del servidor relay y los clientes

## Compilar weechat

Para poder usar SASL es necesario que weechat tenga el mecanismo de autenticación SASL 'ecdsa-nist256p-challenge', para eso es necesario compilarlo y que el paquete 'libgnutls28-dev' (gnutls-devel en centOs) esté instalado.

### Dependencias de weechat en Debian

Con el siguiente comando se deben instalar todas las dependencias necesarias para una correcta compilación:
    # apt-get install cmake pkg-config libncursesw5-dev libcurl4-gnutls-dev zlib1g-dev libgcrypt20-dev libgnutls28-dev gettext ca-certificates libaspell-dev python-dev libperl-dev ruby2.1-dev liblua5.2-dev tcl-dev guile-2.0-dev libv8-dev asciidoc source-highlight xsltproc docbook-xml docbook-xsl libcpputest-dev autotools-dev dh-autoreconf

### Dependencias de weechat para centOs 7
    # yum groupinstall "Development Tools"
    # yum install gnutls-devel ncurses-devel gettext-devel gnutls-devel aspell-devel python-devel perl-devel ruby-devel lua-devel tcl-devel guile-devel mod_perl-devel

### Descargar weechat

    $ git clone https://github.com/weechat/weechat.git

### Compilar e Instalar weechat

    $ ./autogen.sh
    $ mkdir build
    $ cd build
    $ ../configure
    $ make
    # make install

# Servidor RELAY
## Configurar weechat

- Fuente: http://erebor.teraflops.info/blog/2015-06-22/weechat/#.VjY4XGcvekA

### Establecer mecanismo SASL para freenode

- Generar la key:

    $ openssl ecparam -genkey -name prime256v1 >~/.weechat/ecdsa.pem

- Obtener la clave pública:

    $ openssl ec -noout -text -conv_form compressed -in ~/.weechat/ecdsa.pem | grep '^pub:' -A 3 | tail -n 3 | tr -d ' \n:' | xxd -r -p | base64

- Conectar al servidor, identificarse y establecer la clave pública.
    /connect freenode
    /msg nickserv identify tu_contraseña_de_freenode
    /msg nickserv set pubkey pegar_aquí_la_clave_obtenida

- Configurar las opciones de SASL
    /set irc.server.freenode.sasl_mechanism ecdsa-nist256p-challenge
    /set irc.server.freenode.sasl_username "your_nickname"
    /set irc.server.freenode.sasl_key "%h/ecdsa.pem"
