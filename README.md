> # CRUD ARTICULOS 
>

**Connection:**

Este código PHP intenta conectarse a una base de datos MySQL utilizando PDO. Si la conexión es exitosa, la instancia de PDO ($conn) se puede utilizar para realizar consultas y operaciones en la base de datos. Si hay algún problema durante la conexión, se captura la excepción y se imprime un mensaje de error.
```php
<?php
$servername = "192.168.2.68";
$username = "rootIP";
$password = "1234";

try {
  $conn = new PDO("mysql:host=$servername;dbname=crudp", $username, $password);
  // set the PDO error mode to exception
  $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  // echo "Connected successfully";
} catch(PDOException $e) {
  echo "Connection failed: " . $e->getMessage();
}
?>
```

**Create**

Este código proporciona una funcionalidad completa para agregar nuevos registros a una base de datos a través de un formulario web, asegurando una gestión segura y eficiente de los datos. La presentación del formulario en un modal y la redirección posterior mejoran la experiencia general del usuario.

```php
<?php
include "connection.php";
if ($_POST) {
    $nombre = isset($_POST['nombre']) ? $_POST['nombre'] : "";
    $descripcion = isset($_POST['descripcion']) ? $_POST['descripcion'] : "";
    $costo = isset($_POST['costo']) ? $_POST['costo'] : "";
    $precio = isset($_POST['precio']) ? $_POST['precio'] : "";
    $cantidad = isset($_POST['cantidad']) ? $_POST['cantidad'] : "";

    $imagen_temporal = isset($_FILES['imagen']['tmp_name']) ? $_FILES['imagen']['tmp_name'] : "";
    $imagen_contenido = file_get_contents($imagen_temporal);

    $stmt = $conn->prepare("INSERT INTO articulos (nombre, descripcion, costo, precio, cantidad, imagen) VALUES (:nombre, :descripcion, :costo, :precio, :cantidad, :imagen)");
    $stmt->bindValue(":nombre", $nombre);
    $stmt->bindValue(":descripcion", $descripcion);
    $stmt->bindValue(":costo", $costo);
    $stmt->bindValue(":precio", $precio);
    $stmt->bindValue(":cantidad", $cantidad);
    $stmt->bindValue(":imagen", $imagen_contenido, PDO::PARAM_LOB);
    $stmt->execute();

    echo '<script type="text/javascript">';
    echo 'window.location.href = "index_articulos.php";';
    echo '</script>';
}
?>

<!-- Modal para crear articulo -->
<div class="modal" id="create_articulos" tabindex="-1" aria-labelledby="modallabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Nuevo articulo</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <form action="" method="post" enctype="multipart/form-data">
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input type="text" class="form-control" name="nombre" id="inputnombre" placeholder="Ingresa el nombre del producto" required>
                    </div>
                    <div class="mb-3">
                        <label for="descripcion" class="form-label">Descripcion</label>
                        <input type="text" class="form-control" name="descripcion" id="inputdescripcion" placeholder="Es un producto lacteo" required>
                    </div>
                    <div class="mb-3">
                        <label for="costo" class="form-label">Costo</label>
                        <input type="number" class="form-control" name="costo" id="inputcosto" placeholder="120.00" required>
                    </div>
                    <div class="mb-3">
                        <label for="precio" class="form-label">Precio</label>
                        <input type="number" class="form-control" name="precio" id="inputprecio" placeholder="140.00" required>
                    </div>
                    <div class="mb-3">
                        <label for="cantidad" class="form-label">Cantidad</label>
                        <input type="number" class="form-control" name="cantidad" id="inputcantidad" placeholder="50: piezas" required>
                    </div>
                    <div class="mb-3">
                        <label for="imagen" class="form-label">Imagen</label>
                        <input type="file" class="form-control" name="imagen" id="inputimagen" accept="image/*" required>
                    </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                <button type="submit" class="btn btn-primary">Guardar</button>
            </div>
        </div>
        </form>
    </div>
</div>
```

**Delete**

Este código PHP borra un registro de la base de datos MySQL utilizando el valor del parámetro 'id' proporcionado a través de la URL. La eliminación se realiza de manera segura mediante el uso de consultas preparadas y, después de la eliminación exitosa, el usuario es redirigido a la página principal de artículos.
```php
<?php
include "connection.php";
if (isset($_GET['id'])) {
    $id=(isset($_GET['id']) ? $_GET['id'] : "");
    $stmt = $conn->prepare("DELETE FROM articulos WHERE id=:id");
    $stmt->bindParam(":id",$id);
    $stmt->execute();
    header('location: index_articulos.php');
}
?>
```
**Footer**

Este fragmento de código cierra la estructura básica de una página HTML y carga el archivo JavaScript de Bootstrap para aprovechar las funcionalidades proporcionadas por este framework en la parte de interactividad y diseño.
```php
</div> <!--Cierra la clase container-->
    <script src="Bootstrap/js/bootstrap.min.js"></script>
  </body>
</html>
```

**Header**

Este código establece la estructura inicial de una página HTML, enlaza hojas de estilo de Bootstrap para mejorar el diseño y la apariencia, y comienza el área de contenido visible dentro de un contenedor.
```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="Bootstrap/css/bootstrap.min.css" />
    <link rel="stylesheet" href="Bootstrap/icons/bootstrap-icons.min.css">
    <title>Sistema web</title>
  </head>
  <body>
    <div class="container">
```
