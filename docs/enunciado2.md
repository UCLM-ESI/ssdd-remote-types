# Laboratorio Sistemas Distribuidos 24/25 - Entregable 2

Éste documento es el enunciado del segundo entregable de la práctica de Sistemas Distribuidos
del curso 2024/2025 en convocatoria ordinaria.

## Introducción

Tras haber desarrollado el servicio del [[entregable 1]], queremos integrarlo con una tecnología
de cola de mensajes. Por ello, será necesario implementar un **cliente** para el servicio
ya desarrollado que sea capaz de recibir eventos con instrucciones sobre las operaciones
a realizar sobre los objetos de nuestro servicio y enviar las respuestas a dichas operaciones
a través de otro canal de eventos diferente.

## Requisitos

- La tecnología de cola de mensajes utilizada deberá ser **Apache Kafka**.
- El cliente del servicio deberá consumir mensajes de un _topic_ que deberá ser configurable.
- El cliente del servicio deberá realizar la operación definida en cada evento recibido
    sobre el servidor, y enviar la respuesta a través de otro _topic_, que tendrá que
    ser también configurable.
- El alumno deberá demostrar que si se lanzan varias instancias del cliente desarrollado
    no se realizará la misma operación varias veces. Puede hacerse uso del mecanismo de replicación
    con grupos de consumidores (_consumer group_).
- Se deberán respetar los formatos de mensajes definidos en éste enunciado.
- Si un evento recibido no respeta el formato, deberá intentar reportarse por el canal de resultados
    si al menos conocemos el identificador de operación. Si no lo sabemos, se descartará el evento.
- Todos los mensajes de respuesta deberán cumplir el formato definido más adelante.

## Formato de mensajes

Cada evento recibido por el cliente estará definido en formato [JSON][1]. Estará conformado por un
_array_ de operaciones. A continuación, se define el formato de cada petición individual.

### Mensajes de petición de operación

Una petición de operación será un _objeto_ JSON con las siguientes claves:

- **Claves obligatorias**: `"id"`, `"object_identifier"`, `"object_type"`, `"operation"`
- **Claves opcionales**: `"args"`

Las claves contendrán un valor conforme a la siguiente especificación:

- `"id"`: será un identificador único de la operación a realizar. Se utilizará para poder enviar
    el mensaje de respuesta asociado a ésta petición.
- `"object_identifier"` será un "string", equivalente al identificador utilizado para distinguir los
    diferentes objetos en el entregable 1.
- `"object_type"`: será un string indicando el tipo de objeto: `"RSet"`, `"RList"` o `"RDict"`.
- `"operation"`: un string que coincidirá con el nombre del método que se quiera ejecutar sobre el objeto.
- `"args"`: un _object_ JSON donde las claves y valores corresponderán con los definidos para cada operación
    en el Slice.

### Mensajes de respuesta

Al igual que el evento de petición contiene un _array_ de peticiones de operación, la respuesta contendrá un
_array_ de las respuestas obtenidas del servidor. Cada respuesta individual contendrá un _object_ con el formato
definido a continuación.

- **Claves obligatorias**: `"id"`, `"status"`
- **Claves opcionales**: `"result"`, `"error"`

Las claves contendrá un valor respecto a la siguiente especificación:

- `"id"`: será el identificador de la operación realizada, se debe correlacionar con el valor recibido previamente.
- `"status"`: contendrá un string indicando `"ok"` en caso de haberse realizado la operación sin errores o `"error"`
    en caso de haberse producido algún error.
- `"result"`: sólo aparecerá en caso de que la operación se haya realizado correctamente y contedrá el valor
    devuelto por la operación. Si la operación no devuelve nada o ha ocurrido un error, no aparecerá ésta clave.
- `"error"`: sólo aparecerá en caso de que no se haya podido realizar la operación, indicando el error que se ha
    producido. Deberá contener el nombre de la clase causante de la excepción.

## Nota sobre la operación `iter`

Dada la naturaleza de la operación `iter`, no es viable realizar una implementación de la misma a través de Apache
Kafka. Por tanto, si en algún momento se recibiera una solicitud de invocar el método `iter`, primero se intentará
resolver si el objeto es válido o no, y si lo fuera, en lugar de realizar la invocación remota al método `iter`,
se devolverá un error indicando `"OperationNotSupported"`


## Entregable

La práctica se deberá realizar y almacenar en un repositorio de Git privado. La entega consistirá en enviar
la URL a dicho repositorio, por lo que el alumno deberá asegurarse de que los profesores tengan acceso a
dicho repositorio.

El repositorio deberá contener al menos lo siguiente:

- `README.md` con una explicación mínima de cómo configurar y ejecutar el servicio, así como sus dependencias
    si las hubiera.

- El fichero o ficheros de configuración necesarios para la ejecución.
- Algún sistema de control de dependencias de Python: fichero `pyproject.toml`, `setup.cfg`, `setup.py`, Pipenv,
    Poetry, `requirements.txt`... Su uso debe estar también explicado en el fichero `README.md`

### Fecha de entrega

La fecha de entrega será el día 5 de Enero de 2025.


[1]: https://es.wikipedia.org/wiki/JSON
