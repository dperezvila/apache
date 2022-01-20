# Práctica Servidor WEB

## Servidor apache

~~~
asir_apache:
    image: httpd:latest
    networks:
       apanet:
        ipv4_address: 192.168.22.4
    ports:
      - 8081:80
    volumes:
      - apache-data:/usr/local/apache2/htdocs
~~~
Le asignamos una ip fija

## Servidor DNS

~~~
asir_bind9:
    image: internetsystemsconsortium/bind9:9.16
    networks:
       apanet:
        ipv4_address: 192.168.22.5
    ports:
      - 53:53
    volumes:
      - conf:/etc/bind
      - options:/var/cache/bind
      - secondaryzones:/var/lib/bind
      - logfiles:/var/log
~~~
Le asignamos una ip fija, que será la ip del dns en el cliente

## Cliente

~~~
asir_cliente:
    image: kasmweb/desktop:1.10.0-rolling
    networks:
      - apanet
    dns:
      - 192.168.22.5
    ports:
      - 6901:6901
    stdin_open: true  # docker run -i
    tty: true         # docker run -t
~~~
Le asignamos la el DNS 192.168.22.5 al cliente, que es la ip de nuestro DNS

### Redes

~~~
networks:
  apanet:
    external: true
~~~

### Volumenes
volumes:
  apache-data:
    external: true
  conf:
    external: true
  options:
  secondaryzones:
  logfiles:
~~~
