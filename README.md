# Configuración de weechat con: SSL, SASL y RELAY server

Instalación de weechat en centOs 7 y debian, uso de SSL para conectarse a freenode, SASL para identificarse en freenode y configuración del servidor relay y los clientes

## Compilar weechat

Para poder usar SASL es necesario que weechat tenga el mecanismo de autenticación SASL 'ecdsa-nist256p-challenge', para eso es necesario compilarlo y que el paquete 'libgnutls28-dev' (gnutls-devel en centOs) esté instalado.

### Dependencias de weechat en Debian

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

## Instalación en Archlinux

    # pacman -S weechat

# Servidor RELAY

## Configurar weechat

- Fuente: http://erebor.teraflops.info/blog/2015-06-22/weechat/#.VjY4XGcvekA

### Crear servidor 'freenode'

- /server add freenode chat.freenode.net/6697 -ssl

### Crear el servidor 'hispano'

- /server add hispano ssl.chathispano.com/6697 -ssl

#### Nota: En caso de usar centOs, igual hay que hacer lo siguiente:

- /set weechat.network.gnutls_ca_file "/etc/ssl/certs/ca-bundle.crt"

### Configurar weechat con tus credenciales (esto ya lo sabes)

### Establecer mecanismo SASL para freenode

#### Generar la key:

    $ openssl ecparam -genkey -name prime256v1 >~/.weechat/ecdsa.pem

#### Obtener la clave pública:

    $ openssl ec -noout -text -conv_form compressed -in ~/.weechat/ecdsa.pem | grep '^pub:' -A 3 | tail -n 3 | tr -d ' \n:' | xxd -r -p | base64

#### Conectar al servidor, identificarse y establecer la clave pública.

- /connect freenode

- /msg nickserv identify tu_contraseña_de_freenode

- /msg nickserv set pubkey pegar_aquí_la_clave_pública_obtenida

#### Configurar las opciones de SASL

- /set irc.server.freenode.sasl_mechanism ecdsa-nist256p-challenge

- /set irc.server.freenode.sasl_username "tu_nick_de_freenode"

- /set irc.server.freenode.sasl_key "%h/ecdsa.pem"

#### Reconectar a freenode

- /reconnect freenode

Si conecta puedes seguir, pero guarda antes los cambios:

- /save

## Descargar los certificados de startssl.com 

- Tengo un tutorial aquí: https://github.com/nashgul/startssl_free_cert

## Crear el pem

    $ cat sub.class1.server.ca.pem >> ssl.crt

    $ cat ssl.key >> ssl.crt

    $ chmod 400 ssl.crt
    
    $ cp ssl.crt /home/tu_usuario/.weechat/ssl/relay.pem

## Configurar el servidor de relay
    
    /relay sslcertkey
    
    /relay add ssl.weechat 9001
    
    /relay add ssl.irc 8001
    
    /set relay.network.password ********

# Instalar los clientes de relay

## Hacer una instalación típica de weechat

- En los clientes de realy no es necesario que tengan el mecanismo de SASL.

### Debian

    # apt-get update && apt-get install weechat

### Archlinux

    # pacman -S weechat

### centOs

    # yum install weechat

## Configurar weechat (en los clientes)

> /server add freenode ip_o_nombre_de_tu_servidor_de_relay/8001 -ssl -password=freenode:password_introducido_asl_crear_el_relay

> /server add hispano ip_o_nombre_de_tu_servidor_de_relay/8001 -ssl -password=hispano:password_introducido_asl_crear_el_relay

### Si hay problemas con el tamaño de la clave:

- /set irc.server.freenode.ssl_dhkey_size 1024

Bueno, si he cometido errores os podéis quejar con toda la razón en: sinp13@gmail.com 
