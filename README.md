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

* [Configuración de Work Item Handler para Envío de Correos](#configuracion-de-Work-Item-Handler-para-Envio-de-Correos): Habilitar el WIH para uso de nodo de Email.


### Creación Usuarios y roles

Deberemos crear los roles "ejecutivoOficina","analistaCredito","ejecutivoStandard" y "ejecutivoPrime". Para lo cual realizaremos lo siguiente:

`sh $JBOSS_HOME/bin/add-user.sh -a -r ApplicationRealm -u oficina -p Redhat123_ -g user,ejecutivoOficina`

- "-a": Indica que es un usuario de aplicación y no de Management
- "-u": Nombre del usuario que se va a agregar
- "-p": Contraseña a asignar al usuario
- "-g": Roles que se asignarán al usuario.

**NOTA**: TODOS los usuarios de aplicación de BPMS deben tener un rol por defecto para poder acceder a la consola del business central. En este caso estamos limitando el rol a "user".

**NOTA**: Para efectos prácticos se recomienda además asignar estos nuevos roles de aplicación al usuario bpmsAdmin para hacer más simple la prueba del proceso.

### Creación de Organizational Unit y repositorio

* Accedemos al menú de administración

![Menú Administración](https://user-images.githubusercontent.com/20805557/33000328-18cb3ac2-cd86-11e7-948e-1735331af50b.png)

* Creamos nuestra OU (Organizational Unit > Manage Organizational Unit > Add)

![Creación de OU](https://user-images.githubusercontent.com/20805557/33000611-facbe3f8-cd87-11e7-84b6-a1ad0f73b850.png)

* Clonamos nuestro Repositorio (Repository > Clone Repository)

![Clonación Repositorio](https://user-images.githubusercontent.com/20805557/33000692-519fcc62-cd88-11e7-9733-e8224ff0bdb9.png)

### Configuración de Work Item Handler para Envío de Correos

* Accedemos a nuestro proyecto

![Acceder al Proyecto](https://user-images.githubusercontent.com/20805557/33000819-12988ca6-cd89-11e7-8304-40b01ed7c263.png)

**NOTA** Para revisar los artefactos del proyecto debemos estar en la ruta default >> apap >> poc

![Revisar arbol](https://user-images.githubusercontent.com/20805557/33000855-50a7a6ee-cd89-11e7-8810-294f5d734072.png)
https://github.com/RobsonWatt/POC-BPMS-https://github.com/RobsonWatt/POC-BPMS-RESThttps://github.com/RobsonWatt/POC-BPMS-REST
* Dentro de nuestro Project Editor seleccionamos la opción "Deployment descriptor"

![Deployment descriptor](https://user-images.githubusercontent.com/20805557/33000950-c0b5f10c-cd89-11e7-99e2-64272ca0580f.png)

* En la sección "Work Item Handlers" debemos agregar la configuración de correo
**NOTA** Para el envío de correo se ha creado una cuenta ficticia en gmail. Esta cuenta tiene habilitada la opción para el envío de correo desde otros dispositivos.

![Habilitar envio](https://user-images.githubusercontent.com/20805557/33001309-d67a0c88-cd8b-11e7-871e-b4fc3b59154f.png)

 La configuración del Work Item Hanlder de correo debe quedar de la siguiente forma:

 ![Habilitar envio](https://user-images.githubusercontent.com/20805557/33001405-78f3adf2-cd8c-11e7-8655-8d205190e536.png)

Name: Email
Identifier: new org.jbpm.process.workitem.email.EmailWorkItemHandler("smtp.gmail.com","587","workshopbpms@gmail.com","redhat2017","true")
Resolver Type: mvel

### Compilar Proyecto
Al compilar nuestro proyecto se generará la definición de los procesos de negocio del mismo, quedando listo para su ejecución. Para compilar el proyecto simplemente debemos acceder al Project Editor y seleccionar la opción "Build"

![Habilitar envio](https://user-images.githubusercontent.com/20805557/33001547-6011106c-cd8d-11e7-8b9d-b52d3ea402af.png)

**IMPORTANTE**
Si ya hemos desplegado anteriormente el proyecto y queremos volver a desplegarlo, debemos modificar la versión del mismo o bien eliminar el despliegue desde el menú Deploy > Process Deployment.

### Despliegue de Servicio REST
Finalmente deberemos compilar y desplegar el servicio REST en nuestro Jboss EAP que soporta nuestro Jboss BPMS. Este servicio Rest es el que se está invocando desde el flujo principal y es necesario para el correcto funcionamiento de la POC.

Para desplegar este servicio REST por favor seguir las indicaciones entregadas en el siguiente repositorio:

![https://github.com/RobsonWatt/POC-BPMS-REST](https://github.com/RobsonWatt/POC-BPMS-REST)

### Ejecución del flujo
Con nuestra POC preparada sólo nos resta probar su funcionamiento, para esto iniciaremos la definición de proceso "SolicitudCredito" desde el menú  Process Management > Process Definition

![Ejecucion proceso](https://user-images.githubusercontent.com/20805557/33001772-ceaf6b62-cd8e-11e7-946c-e7ad2a36c7bf.png)

Finalmente sólo debes seguir el flujo hasta que se termine la instancia.

**IMPORTANTE**
Para el correcto funcionamiento del proceso, en el formulario de crédito se debe ingresar a lo menos información para los campos "Nombre", "sexo" (M/F), "edad", "salario" y "correo electronico"; ya que estos campos están siendo considerados en las reglas de negocio creadas, su no asignación podría generar problemas en la ejecución de esta POC.
