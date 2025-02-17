# Modbus

#Description

Complemento para leer y escribir en sus dispositivos ModbusTCP/IP y RTU



# Configuración del complemento

Después de descargar el complemento, primero debe activarlo, como cualquier complemento Jeedom :

![config](./images/ModbusActiv.png)

Luego, hay que iniciar la instalación de las dependencias (aunque aparezcan OK) :

![dependances](./images/ModbusDep.png)

Finalmente, inicie el demonio :

![demon](./images/ModbusDemon.png)

Rien n'est à modifier dans le champ « Port socket interne » de la section « Configuration ».

![socket](./images/ModbusConfig.png)

En esta misma pestaña, deberás elegir el valor de descanso entre actualizar tu equipo (por defecto 5 seg)

También puede optar por poner un Reintento para volver a ejecutar la solicitud en un comando/equipo que tendría un error (por defecto, Falso))
También puede elegir el número de intentos y la demora entre estos intentos.




# Uso del complemento


IMPORTANTE :

Para usar el complemento, debe conocer los parámetros de sus entradas / salidas de sus periféricos modbus (formato de datos, orden de bits, etc...)

Para los comandos, hay parámetros para seleccionar :

Detalles del parámetro :
- Valor negativo : para formatos de tipo LONG/INT, debe especificar si el valor de escritura/lectura será negativo
- Compensar : esto es si la compensación se tiene en cuenta o no en los números de registro en ciertos dispositivos Modbus
- Elija el tono del control deslizante : Esto es para elegir el paso del control deslizante en el caso de un comando de tipo Acción/Deslizador si desea enviar valores no enteros.




IMPORTANTE :

Dado el tiempo que lleva tener que configurar en ocasiones determinados equipos, es posible exportar los comandos de un equipo ya creado, para descargarlo localmente en .json.

Por lo tanto, puede importarlo fácilmente en otra caja en un nuevo equipo del mismo tipo (solo para cambiar lo que difiere en términos de su conexión)


En la página del equipo, abajo a la derecha, tienes este inserto : 

![dependances](./images/exportFunction.png)


Haga clic en Lista de pedidos para exportar; se abre una ventana con los comandos existentes en este equipo:

![dependances](./images/choiceCmds.png)

Puede seleccionarlos todos si es necesario usando el botón en la parte superior de la ventana. 
Cuando se elijan los comandos, haga clic en Validar.



Ahora verá los pedidos elegidos y listos para ser exportados en este inserto :

![dependances](./images/exportCmds.png)

Solo tienes que hacer clic en Descargar configuración de las órdenes que acaban de aparecer.



Para importar comandos al equipo : haga clic en la parte superior derecha del equipo en el botón Importar Json :

![dependances](./images/importFunction.png)












CONTROLES DE REPRODUCCIÓN :

Para bobinas y entradas discretas :  
  - Agrega un nuevo comando Modbus y nombra el comando. Elija un comando de tipo de información, en tipo binario o numérico.
  - Elija el código de función correspondiente : FC01 o FC02
  - Entonces es necesario elegir el registro inicial así como el número de bytes a leer (el número de registros)
  Cuando guarde, el comando creado se eliminará, para crear tantos comandos como el número de bytes especificado.
  Ex: Si elige un registro de inicio de 1 y un número de bytes de 4, los comandos se crearán : LeerBobina_1, LeerBobina_2, LeerBobina_3, LeerBobina_4
  - Por supuesto, puede cambiar el nombre de ReadCoils/Discretes a su gusto.



  Para registros de existencias y registros de entradas:
  - Agrega un nuevo comando Modbus y nombra el comando. Elige un comando de tipo de información, en Tipo numérico.
  - Elige el formato correspondiente : Flotante, Largo/Entero o Bits
  - Elija el código de función correspondiente : FC04 o FC03
  - El registro inicial así como el número de bytes : para flotantes, el número máximo de registros codificados es de 4 registros.



ESCRIBIR COMANDOS:

 En su equipo, por defecto habrá 3 comandos de tipo Acción/mensaje creados; Escritura de registro múltiple, escritura de bit y escritura de bobina múltiple


IMPORTANTE :


 Su principio de funcionamiento:



![cmdEcritures](./images/modbusCmdsEcritures.png)




  - Escritura de registro múltiple : en la configuración del comando se debe ingresar el registro de inicio, así como el orden de los bytes y palabra.
  Por defecto, el código de función es FC16. Por favor, deje esta configuración por defecto.

  Para cambiar los valores en los registros, use esta sintaxis:
  - valorparaenviar&nbofregistro, separados por | :   Ex:  120 y 1|214.5&4 Enviamos el entero 120 a un registro, partiendo del registro inicial configurado,
  entonces 214.5 en float en 4 registros siguientes al anterior.

  Para tipos flotantes, escriba el valor como se indica arriba, con un .


  - Escritura multibobina : en la configuración del comando, debe ingresar el registro de inicio
  Por defecto, el código de función es fc15. Por favor, deje esta configuración por defecto.

  Para cambiar los valores en los registros, use esta sintaxis:
  -  Ex : 01110111 Entonces esto enviará desde el registro de inicio configurado los valores True(1) o False(0) a los registros




  - Bit de escritura : en la configuración del comando se debe ingresar el registro de inicio, así como el orden de los bytes y palabra.
  Por defecto, el código de función es fc03, porque este comando le dará el valor del registro establecido en binario al comando info "infobitbinary".

  Por favor, deje esta configuración por defecto.

  En el comando info "infobitbinary", tendrá el valor binario del registro de parámetros en el comando Write Bit.
  Para cambiar el bit en el registro

  - valuetoend&PositionBit :   Ex:  1&4 Enviamos el valor 1 al bit de la posición 4 empezando por la derecha
  En el comando de información "infobitbinary", verá el valor 10000101, que corresponde al valor binario del registro de parámetros.
  Al escribir 1 y 6, ahora tendrá el valor : 10100101 en el registro configurado.



IMPORTANTE :


Algunos PLC no tienen la función fc06
Puede crear un comando de acción, en Tipo de mensaje, y elegir fc16
Compruebe el registro Fc16 no rastreado
En el tablero, debe usar esta sintaxis :
registro de salida ! value & nbregisters separados por un |

Ex: 7!122.5 y 2|10!22 y 2

Escribiremos del registro 7, el valor 122.5 en 2 registros y también del registro 10, el valor 22, en 2 registros



Para escribir en una bobina :

Ejemplo para registro 1 On:
- Agrega un nuevo comando Modbus y nombra el comando. Elige un comando de tipo Acción, en Tipo predeterminado.
- Elija Fc5 Escribir bobina simple
- Registro de salida : 1
- Número de bytes : 1
- Poner 1 en valor para enviar

Ejemplo de registro 1 Off:
- Agrega un nuevo comando Modbus y nombra el comando. Elige un comando de tipo Acción, en Tipo predeterminado.
- Elija Fc5 Escribir bobina simple
- Registro de salida : 1
- Número de bytes : 1
- Poner 0 en valor para enviar


Al actuar sobre estos comandos de acción en su tablero, enviará Verdadero o Falso a sus Coils.




Para escribir en un registro de retención :

- Agrega un nuevo comando Modbus y nombra el comando. Elige un comando de tipo Acción, en Tipo de control deslizante.
- También elija un valor mínimo y máximo para este control deslizante (recuerde tomar un valor mínimo para enviar un valor negativo)
- Elija BC6 Escribir registro único
- Elige el número de registros : 1
- Elija el paso del control deslizante (para decimales, escriba con un .   ex: 0.2)
