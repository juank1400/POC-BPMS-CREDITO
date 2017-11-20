# Red Hat Jboss BPMS POC - Flujo simple de crédito

## Resumen

Esta POC pretende mostrar las capacidades generales de Jboss BPMS, se ha considerado:

1.- Proceso general y llamado a subproceso reutilizable<br/>
2.- Invocación de reglas de negocio para la segmentación de cliente y cálculo de crédito<br/>
3.- Se considera la interacción de 4 roles<br/>
4.- Invocación de Rest<br/>
5.- Envío de correo usando nodo email del producto.<br/>
6.- Customización simple de formularios.<br/>


## Preparación del ambiente

**IMPORTANTE:** Se da como supuesto que Jboss BPMS ya se encuentra instalado con acceso a la consola de business central.

Para la configuración base de la poc debemos realizar las siguientes acciones:

* [Creación de usuarios/roles](#creacion-usuarios-y-roles): Creación de usuarios y roles para soportar el correcto funcionamiento del Flujo

* [Creación de Organizational Unit y repositorio](#creacion-de-organizational-unit-y-repostorio): Organizaremos nuestra poc dentro de una OU sobre la cual clonaremos este repositorio


### Creación Usuarios y roles

Deberemos crear los roles "ejecutivoOficina","analistaCredito","ejecutivoStandard" y "ejecutivoPrime". Para lo cual realizaremos lo siguiente:

`sh $JBOSS_HOME/bin/add-user.sh -a -r ApplicationRealm -u oficina -p Redhat123_ -g user,ejecutivoOficina`

- "-a": Indica que es un usuario de aplicación y no de Management
- "-u": Nombre del usuario que se va a agregar
- "-p": Contraseña a asignar al usuario
- "-g": Roles que se asignarán al usuario.

**NOTA**: TODOS los usuarios de aplicación de BPMS deben tener un rol por defecto para poder acceder a la consola del business central. En este caso estamos limitando el rol a "user".

### Creación de Organizational Unit y repositorio

* Accedemos al menú de administración
![Menú Administración](https://drive.google.com/file/d/1o-Nmm4m5N4XWbvgQeKNJz0MmjNnvi28q/view)
