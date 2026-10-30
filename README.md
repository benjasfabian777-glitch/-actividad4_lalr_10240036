# Actividad 4: Construcción de un analizador sintáctico LALR con Bison y Flex

**Nombre:** Benjamín Fabián Gabino
**Matrícula:** ESCRIBE_AQUI_TU_MATRICULA
**Grupo:** ISC151

## Objetivo
Construir e integrar un analizador léxico y sintáctico LALR utilizando Flex y GNU Bison bajo el estándar C11 para validar sentencias de asignación y expresiones aritméticas. El analizador gestiona niveles de precedencia, operadores unarios, asociatividad por la derecha para la potencia, recuperación de errores sintácticos mediante sincronización y registro de un catálogo acotado de identificadores.

## Requisitos
* Entorno WSL Ubuntu
* GNU Bison 3.6 o posterior
* Flex
* GCC con soporte para el estándar C11
* GNU Make
* Git
* Utilidades `zip` y `unzip`

## Comandos de compilación y prueba
* Limpiar binarios y archivos generados:
  `make clean`
* Compilar el analizador:
  `make`
* Ejecutar la batería automatizada de pruebas:
  `make test`

## Códigos de salida
* `0`: Aceptación sin errores léxicos ni sintácticos.
* `1`: Error léxico, sintáctico o límite de capacidad del catálogo excedido.
* `2`: Uso incorrecto de argumentos en línea de comandos o archivo de entrada no disponible.

## Decisiones de diseño
* **Precedencia de operadores:** Definida de menor a mayor jerarquía en `parser_lalr.y`: suma y resta (`SUMA`, `RESTA`), seguidas de multiplicación y división (`MULTIPLICA`, `DIVIDE`), potencia (`POTENCIA`) y el menos unario en el nivel más alto para evaluar signos negativos antes que operaciones binarias.
* **Asociatividad de `^`:** Configurada con `%right POTENCIA` (líneas 40 y 62 de `parser_lalr.y`) para que expresiones encadenadas como `a ^ b ^ c` se agrupen de derecha a izquierda (`a ^ (b ^ c)`).
* **Recuperación de errores con `;`:** Implementada mediante la producción `error PUNTO_COMA` (línea 50 de `parser_lalr.y`), permitiendo descartar tokens inválidos hasta hallar el delimitador `;`, contabilizar la recuperación y continuar analizando las sentencias posteriores.
* **Alcance del catálogo de identificadores:** Almacenamiento en memoria estática de capacidad fija que registra cada identificador válido (`ID_VALIDO`), conservando la primera línea en la que aparece y el conteo acumulado de sus apariciones; si se supera la capacidad máxima definida, se reporta error de capacidad y el programa finaliza con código `1`.


## URL del repositorio
https://github.com/benjasfabian777-glitch/-actividad4_lalr_10240036.git
