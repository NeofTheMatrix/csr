# CSR
Conexión Shell Rápida (Para conectarse rápidamente a servidores usando solo el hostname cuando no está disponible el DNS)


### NOMBRE

csr — Conexión Shell Rápida (Para conectarse rápidamente a servidores usando solo el hostname cuando no está disponible el DNS)


### DESCRIPCIÓN

CSR permite conectarse a host remotos mediante protocolos seguros como SSH usando un inventario preconfigurado. Esto le permite a los administradores de sistemas UNIX ubicar y conectarse rápidamente a servidores UNIX virtuales o físicos mediante el protocolo SSH, directamente desde su terminal de linux, sin necesidad de usar herramientas gráficas.  

CSR puede ser útil cuando no tenga disponible un servicio DNS para conectarse a todos los servidores. Solo debe configurar el inventario con la información de todos los host de destino.

También posee otras utilidades para el caso de servidores Oracle. Como ubicar la Zona global o servidores primarios donde se encuentra el servidor virtual que especifique y conectarse a ellos. De esta manera podrá encontrarlos facilmente si no recuerda cuales son.

Configuración Inicial:
----------------------

1- Coloque todos los archivos de CSR en su carpeta ~/bin para que pueda usar el comando csr desde cualquier ubicación.

2- Use el siguiente comando para configurar el usuario por defecto: 
```shell
    csr --setUser [ Nombre del usuario ]
```



### SYNOPSIS

Conexión:
```shell
        csr [-Opciones] [Nombre del servidor] [Usuario (Opcional)]
```
Configuración:
```shell
        csr [-Opciones] [Argumentos]
```


### USO BÁSICO

La idea de CSR es simplicar la conexión ssh a los servidores administrados. Por lo que una vez preconfigurado el usuario por defecto y el inventario de servidores, podrá usar el siguiente comando para conectarse rapidamente a ellos:
```shell
        csr [ Nombre del servidor ]

        csr hostname
```
Para usar un usuario diferente al configurado por defecto, solo debe indicarlo como un segundo argumento:
```shell
        csr [ Nombre del servidor ] [ Nombre del usuario ]

        csr hostname username
```

### OPCIONES

-h     --help Muestra el documento de ayuda de csr.

Uso:

                csr -h
                csr -?
                csr --help

Conexión:

-p     --primary Esta opción le permite conectarse al servidor primario en el que se encuentra un servidor virtual especificado. Esto se determina según la información del inventario.

Uso: 

                csr -p [ Nombre del servidor virtual ]
                csr --primary [ Nombre del servidor virtual ]


-g     --global Esta opción le permite conectarse al servidor global en el que se encuentra un servidor de zona virtual especificado. Esto se determina según la información del inventario.

Uso: 
                
                csr -g [ Nombre del servidor zona virtual ]
                csr --global [ Nombre del servidor zona vitual ]


Configuración:

-sU    --setUser Puede usar esta opción para configurar el usuario por defecto que será usado para conectarse a todos los hostremotos del inventario:

Uso:

                csr -sU [ Nombre del usuario ]
                csr --setUser [ Nombre del usuario ]


-gU    --getUser Use esta opción para saber el usuario por defecto que se encuentra configurado.

Uso:
                
                csr -gU
                csr --getUser


-in    --insert Use esta opción para insertar un nuevo elemento al inventario de host remotos de csr:

Uso:

    csr -in [ Tipo de host ] [ Nombre del nuevo host ] [ dirección ip del nuevo host ] [ Nombre de host padre ]
    csr --insert [ Tipo de host ] [ Nombre del nuevo host ] [ dirección ip del nuevo host ] [ Nombre de host padre ]


Tipos de host:<br>
                p: Primary host, para identificar el host primario de servidores cluster.<br>
                g: Global zone, para indentificar zonas globales de servidores Oracle.<br>
                v: Zonas virtuales, para indentifcar host virtuales o zonas virtuales de
                   servidores Oracle.<br>
                o: Otros tipo de host

Ejemplos:

Agregar un primary host:

                    csr -in p hostnameprimary 192.168.1.101

Nota: Es importante que use el argumento "p" para insertar un host de tipo Primary. Esto le permitirá asignarle host de zonas globales posteriormente.

Agregar un global host:

                    csr -in g hostnameglobal 192.168.1.102 hostnameprimary

Nota: Es necesario que indique el nombre del host primario como cuarto argumento. CSR buscará el nombre host primario indicado (hostnameprimary) e insertará el nombre de la zona global (hostnameglobal) asignándolo a dicho host. Esto permitirá que la opción -p funcione correctamente. Si csr no consigue un host de tipo "p" con el nombre "hostnameprimary" indicado, NO agrerá el nuevo elemento al inventario.

Es importante que use el argumento "g" para insertar un host de tipo Zona global. Esto le permitirá asignarle host virtuales posteriormente.

Agregar una zona virtual:

                    csr -in v hostnamevirtual 192.168.1.103 hostnameglobal

Nota: Es necesario que indique el nombre de la zona global como cuarto argumento. CSR buscará el nombre de la zona global indicada (hostnameglobal) e insertará el nombre de la zona virtual (hostnamevirtual) asignándolo a dicho host. Esto permitirá que la opción -g funcione correctamente. Si csr no consigue un host de tipo "g" con el nombre "hostnameglobal" indicado, NO agrerá el nuevo elemento al inventario.

Agregar un host de cualquier otro tipo:

                    csr -in o hostname 192.168.1.201



### ACERCA DE CSR

   Conexión Shell Rápida.<br>
   Versión 0.1<br>
   Desarrollado por: Anderson José Campos Rosales.<br>
   Licencia MIT


#### ADVERTENCIAS
    
Por medidas de seguridad, CSR no fue creado para almacenar contraseñas de servidores. Queda bajo su responsabilidad las consecuencias de modificar cualquier versión de CSR para que también cumpla con esa función.

