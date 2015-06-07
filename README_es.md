PYTHONRC
========
Guión de inicialización para el intérprete interactivo de Python. Su propósito
principal es mejorar la experiencia de usuario general al utilizar ese tipo de
entornos mediante la aplicación de algunos retoques a la consola estándar.

Funciona también con IPython y BPython, aunque su utilidad en ese tipo de
escenarios es discutible.

Probado en GNU/Linux con las versiones de Python 2.7 y 3.4.

Por favor, lea la sección de Instalación más abajo.


Características
---------------
- Compleción de las entradas del usuario
    + Introduce un mecanismo de compleción para las órdenes introducidas por el
    usuario en Python 2.
    + En Python 3, donde la consola estándar es mucho más amigable, se limita a
    suplantar la maquinaria de completado por defecto para mantener la
    consistencia con el comportamiento en Python 2 (y así mantener la
    posibilidad de adaptarla a las necesidades del usuario).

- Historial de órdenes
    + Crea un objeto **singleton** invocable (**callable**) llamado `history`
    y lo pone dentro del objeto `__builtins__,` para hacerlo fácilmente
    accesible, el cual permite el manejo del historial de órdenes (guardar
    algunas de las líneas introducidas a un fichero de su elección, listar las
    órdenes entradas hasta el momento, etc.) Pruebe a ejecutar simplemente
    `history()` en el intérprete para verlo en acción; inspecciones sus
    miembros (con `dir(history)` o `help(history.write)`) para más información.

- Símbolo de entrada de órdenes en color
    + Instaura un **prompt** colorido, si el terminal lo soporta.


Instalación
------------
- Debe definirse en el entorno (en GNU/Linux y MacOS X esto, generalmente, se
refiere al fichero `~/.bashrc`) la variable `PYTHONSTARTUP` conteniendo la ruta
a `pythonrc.py`

- También es altamente recomendable definir la variable `PYTHON_HISTORY_FILE`.
Recuerde que BPython (a diferencia del intérprete estándar o de IPython) ignora
esta variable, por lo que deberá configurarse éste por otros medios para poder
emplear el mismo fichero de historial con el mismo (en Linux, por ejemplo, el
fichero `~/.config/bpython/config` es un buen punto de partida, pero por favor
consulte la documentación de BPython).

### Configuraciones de ejemplo
- Extracto de `~/.bashrc`
```sh
# python
export PYTHONSTARTUP=~/.python/pythonrc.py
export PYTHON_HISTORY_FILE=~/.python/.python_history

## Podría querer "descomentar" también estas líneas si usa virtualenvwrapper
# export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3.4
# export WORKON_HOME=~/.python/virtualenvs
# source $(which virtualenvwrapper.sh)
```

- Extracto de `~/.config/bpython/config`
```
[general]
color_scheme = default
hist_file = ~/.python/.python_history
hist_lenght = 1000
```

Fallos / Advertencias / Mejoras futuras
---------------------------------------
- No se realiza introspección en los módulos para el último argumento en
órdenes de la forma `from <paquete> import <esto_no_se_completa>` (Esto, en
realidad, podría no ser tan malo, porque no ejecuta efectos colaterales como
p.ej. código de inicialización de los módulos).

- Dependiendo del sistema en el que está el usuario, la recopilación de la
lista de paquetes y módulos para la compleción de órdenes tales como
`import ...` y `from ... import ...` puede tomar un tiempo elevado, en especial
la primera vez que se invoca.

- Al completar cosas como el nombre de un método, el comportamiento por defecto
es incluir también el paréntesis de cierre junto con el de apertura, pero el
cursor siempre se coloca tras el paréntesis de cierre, en lugar de entre ambos.
Esto se debe a las limitaciones del módulo `readline` de Python.
Se puede desactivar la inclusión del paréntesis de cierre; si lo hace, podría
también estar interesado en modificar la variable `dict_keywords_postfix` (en
especial las cadenas que actúan de índices de ese diccionario).

- IPython tiene su propia **magia** `%history`. He hecho lo posible para no
interferir con ella, pero desconozco las consecuencias reales. Además, es
discutible si tiene sentido usar este código con IPython y/o BPython (aunque
tener unificado el historial para todos los entornos es agradable).
Se pueden definir algunos alias de bash como
```sh
alias ipython='PYTHONSTARTUP="" ipython'
alias bpython='PYTHONSTARTUP="" bpython'
```
para quedar más tranquilo.

- Podría haber usado el módulo `six` para mejorar la claridad. Ahora mismo se
emplean mis propios métodos sustitutivos para que funcione tanto en Python 2
como 3.

- Haría falta mejorar los comentarios y la documentación, especialmente la
parte relativa al manejo del historial.

- Probablemente muchos más. Siéntase libre de redactar informes de errores ;-)
