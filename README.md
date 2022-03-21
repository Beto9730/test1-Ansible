# test1-Ansible
Laboratorio usando variables con Ansible

Para permitir más flexibilidad en playbooks y roles, Ansible tiene la capacidad de trabajar con variables. Estas variables se pueden usar para recorrer una serie de valores determinados, acceder a informacion diversa, como el hostname o la ip de un sistema, e incluso reemplazar ciertas cadenas en una plantilla por valores específicos.

Las variables se pueden proporcionar a través del inventario, de un archivo de variables, o se pueden incluir por linea de comandos al llamar un playbook.

Dicho esto , empezemos...

1. Variables y bucles

- En general, los bucles son uno de los casos de uso mas comunes de variables, copiando un conjunto de archivos, es posible escribir una tarea para cada archivo o simplemente recorrerlos:
```
  - name: Copia ficheros
    copy: 
      src: /etc/tmp/{{ item }}
      dest: /tmp/{{ item }}
    with_items:
      - prueba1.txt
      - prueba2.txt

*Las variables se pueden usar en los argumentos del módulo y se referencian entre llaves {{ variable }}*
```
2. Variables dentro de condicionales

- Las variables se pueden usar dentro de condiciones, lo que garantiza que ciertas tareas solo se ejecuten cuando, la variable solicitada se establece en un valor determinado
```
- name: Install Apache on RHEL
    yum:  
      name: httpd
    when: ansible_os_family == "RedHat"

*En este caso, la tarea solo se ejecuta en máquinas Red Hat*
```

3. Variables desde la línea de comandos

- Otra manera de definir variables en Ansible es llamar a los playbooks con la opción --extra-vars o -e en su formato abreviado:
```
$ ansible-playbook --extra-vars "test_var=alberto"

La referencia a esta variable, es otra vez, entre llaves {{ variable }} 

environment: {{ test_var }}
```

4. Variables en el inventario

- En ocasiones, puede cobrar sentido definir variables directamente en un fichero de inventario previamente definido:
```
[clients]
helium intevent_var=helium_123
neon invent_var=bar

El inventario trata más o menos cualquier argumento que no sea específico de Ansible como variable de host.
```
- También es posible establecer variables para hostgroups completos:
```
[clients]
helium
neon
 
[clients:vars]
invent_var=group-foo
```
5. Precedencia de Variables

-En general, Ansible da prioridad a las variables que se definieron más recientemente, de manera más activa y con un alcance más explícito. Las variables en la carpeta predeterminada dentro de un rol se anulan fácilmente. Cualquier cosa en el directorio vars del rol anula las versiones anteriores de esa variable en el espacio de nombres. Las variables de host y/o inventario anulan los valores predeterminados de funciones, pero las inclusiones explícitas, como el directorio vars o una include_varstarea, anulan las variables de inventario.