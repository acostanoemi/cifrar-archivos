![tests](https://github.com/acostanoemi/cifrar-archivos/actions/workflows/tests.yml/badge.svg)

# cifrar-archivos

Cifra y descifra cualquier archivo con una contraseña, desde la línea de comandos. El archivo cifrado es ilegible sin la clave. Hermano del [gestor de contraseñas](https://github.com/acostanoemi/gestor-contrasenas): mismo motor criptográfico (Scrypt + Fernet), aplicado a archivos.

## Para qué sirve

- Guardar algo sensible en la nube sin que el proveedor lo pueda leer.
- Mandar un archivo de forma segura (la clave se pasa aparte, por otro canal).
- Proteger documentos en el disco.

## Cómo funciona

1. De tu contraseña se deriva una clave con **Scrypt** (lento a propósito: encarece el fuerza bruta), usando una "sal" aleatoria.
2. Con esa clave se cifra el archivo con **Fernet** (cifrado + verificación de integridad: si tocan el archivo, se detecta).
3. El archivo `.cifrado` guarda la sal + los datos cifrados. Sin la contraseña, es ilegible.

## ⚠️ No hay recuperación

Si olvidás la contraseña, el archivo se perdió. No hay forma de recuperarlo: esa es la prueba de que está bien cifrado. Anotá la clave en un lugar seguro (un gestor de contraseñas, por ejemplo).

## Requisitos

- Python 3
- La librería `cryptography`: `sudo apt install python3-cryptography`

## Uso

```bash
python3 cifrar.py cifrar documento.pdf                 # -> documento.pdf.cifrado
python3 cifrar.py descifrar documento.pdf.cifrado      # -> documento.pdf
python3 cifrar.py cifrar foto.jpg -o secreto.bin       # nombre de salida propio
python3 cifrar.py cifrar datos.txt --borrar-original   # borra el original al terminar
```

Funciona con cualquier tipo de archivo (texto, imágenes, PDFs, lo que sea).

## Seguridad

- El archivo cifrado se guarda con permisos `600` (solo el dueño puede leerlo).
- La contraseña no se muestra en pantalla al tipearla.
- Nota: `--borrar-original` hace un borrado común; no destruye los datos del disco por completo (eso requiere herramientas de borrado seguro).

## Roadmap

- [x] v1: cifrar y descifrar archivos con contraseña
- [ ] v2: cifrar carpetas enteras (varios archivos de una)
- [ ] v3: leer archivos grandes por bloques (para no cargarlos enteros en memoria)
