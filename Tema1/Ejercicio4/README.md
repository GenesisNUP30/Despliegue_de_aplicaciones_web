# EJERCICIO 4 - Expresiones regulares

## Escribe las expresiones regulares para los siguientes supuestos:

### 1. Directorios en /www/ cuyo nombre consista en tres dígitos.
^/www/[0-9]{3}/$


### 2. Ficheros: *.gif, *.jpeg, *.jpg, *.png
.+\.(gif | jpe?g | png )


### 3. Escribe una directiva para redireccionar todos los GIF a ficheros JPEG en otro servidor
RedirectMatch "(.*)\.gif$" "$1.jpg"

### 4. Números enteros y decimales


Números de teléfono en el formato Americano: 123-123-1234


Palabras


Códigos hexadecimales de color de 24 o 32 bits


Palabras de 4 letras


Número entero sin signo


Número entero con signo


Números reales

