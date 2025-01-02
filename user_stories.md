**Nota:** Los roles iniciales son administrador y usuario (no administrador), la idea es definir politicas y/o permisos que desde un administrador se puedan otorgar a un usuario normal, por ejemplo, un usuario normal únicamente tiene permisos para consulta de datos en tiempo real, sensores registrados y demás, pero puede existir el caso en el que un administrador le otogue permisos de registro (y por tanto de modificación) de sensores en el sistema, pude también darse el caso en el que un usuario además tenga permisos para edición y registro de minas.

### Módulo de Registro y Busqueda de Sensores

#### Historias de Usuario
- Como usuario quiero contar con la posibilidad de registrar sensores que se encuentren permitidos para su uso en el contexto (de caracter semi-industrial).
- Como administrador quiero que en el proceso de registro de un sensor, o más adelante (en la busqueda de uno ya registrado) se pueda indicar, en el caso de que esté o vaya ser usado, la mina a la que corresponde.
- Como administrador quiero tener la posibilidad de definir los rangos en los que un sensor puede indicar alguno de los tres estados de NORMAL, ADVERTENCIA o PELIGRO.
- Como administrador necesito un mecanismo que me permita saber qué sensores se encuentran registrados, cuales de esos se encuentran en uso y cuales no (indicando la mina asociada en el caso de estár en uso).

### Módulo de Registro de Minas

#### Historias de Usuario

- Como administrador del sistema necesito registrar minas e información relevante sobre estas, de tal forma que si voy a registrar o implementar sensores pueda asociarlos a una mina, de igual forma en el momento que se emita una alerta pueda diferencia y ubicar la mina (de acuerdo a sus datos) automatizando la generación de alertas.

### Módulo de Visualización 3D

#### Historias de Usuario

- Como usuario necesito tener la visual en 3D de la distribución de nodos sensores (unidad que centraliza diferentes sensores) de tal forma que en tiempo real pueda ver los variables sensadas para un nodo sensor seleccionado, además de poder inicialmente ver qué sensores se encuentran en ese nodo sensor.
- En la representación en tres dimensiones, la lectura de los sensores, pudiendo diferencia e identificar cuándo una lectura está en un rango NORMAL, de ADVERTENCIA y de PELIGRO.

### Módulo de monitoreo en tiempo real

#### Detalle ténico

Este módulo es el encargado de alimentar el frontend de lo que será el módulo de visualización 3D

### Módulo de Gestión de Normas y Certificados

#### Historias de Usuario

- Como administrador quiero tener la posibilidad de registrar y/o establecer rangos de medición para los cuales se determinan las variables cualitativas de alerta y/o estado de NORMAL, ADVERTENCIA y PELIGRO en un sensor, ya sea por lo que me diga la norma asociada al sensor o una rangos personalizados.
- Teniendo en cuenta que a partir de la norma se puede tener una o más certificaciones (uno o varios organismos pueden certificar una norma), como administrador necesito tener la posibilidad de asociar uno o más certificados a una norma.

### Módulo de interoperabilidad

#### Historias de Usuario

- Como empleado/funcionario/encargado de la Agencia Nacional Minera quiero tener acceso a los datos en tiempo real de una o varias minas determinadas, esto para consumir a nivel de API y poder realizar analitica de datos.
