# EJERCICIO 4 - Expresiones regulares

## Escribe las expresiones regulares para los siguientes supuestos:

### 1. Directorios en /www/ cuyo nombre consista en tres dígitos.
^/www/[0-9]{3}/$


### 2. Ficheros: *.gif, *.jpeg, *.jpg, *.png
^.+\.(gif|jpe?g|png)$


### 3. Escribe una directiva para redireccionar todos los GIF a ficheros JPEG en otro servidor
RedirectMatch "(.*)\.gif$" "$1.jpg"

### 4. Números enteros y decimales
^[0-9]+(\.[0-9]+)?$


### 4. Números de teléfono en el formato Americano: 123-123-1234
^[0-9]{3}-[0-9]{3}-[0-9]{4}$


### 5. Palabras
^[A-Za-z]+$


### 6. Códigos hexadecimales de color de 24 o 32 bits
24 bits son 6 hexadecimales y 32 bits son 8 hexadecimales: 
^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{8})$


### 7. Palabras de 4 letras
^[A-Za-z]{4}$


### 8. Número entero sin signo
^[0-9]+$


### 9. Número entero con signo
^[+-]?[0-9]+$


### 10. Números reales
^[+-]?[0-9]*\.?[0-9]+$


### 11. Número reales con exponente
^[+-]?[0-9]*\.?[0-9]+([eE][+-]?[0-9]+)?$


### 12. Email
^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$


### 13. Números del 0 a 255
^(25[0-5]|2[0-4][0-9]|1?[0-9]?[0-9])$


