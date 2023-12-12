# Poner codigo en GitHub
- Ejemplo de como poner codigo en github tienes que usar ```seguido del lenguaje en mi caso php y para finalizar tienes que volver a usar las tres comillas:

```php
<?php
include "connection.php";

if (isset($_GET['id'])) {
    $id = (isset($_GET['id']) ? $_GET['id'] : '');
    $stmt = $conn->prepare("SELECT * FROM articulos WHERE id=:id");
    $stmt->bindValue(":id", $id);
    $stmt->execute();
    $result = $stmt->fetch(PDO::FETCH_LAZY);
    $nombre = $result['nombre'];
    $descripcion = $result['descripcion'];
    $costo = $result['costo'];
    $precio = $result['precio'];
    $cantidad = $result['cantidad'];
    $imagen = $result['imagen'];
}

if ($_POST) {
    $id = (isset($_POST['id']) ? $_POST['id'] : '');
    $nombre = (isset($_POST['nombre']) ? $_POST['nombre'] : '');
    $descripcion = (isset($_POST['descripcion']) ? $_POST['descripcion'] : '');
    $costo = (isset($_POST['costo']) ? $_POST['costo'] : '');
    $precio = (isset($_POST['precio']) ? $_POST['precio'] : '');
    $cantidad = (isset($_POST['cantidad']) ? $_POST['cantidad'] : '');
    
    $imagen_temporal = isset($_FILES['imagen']['tmp_name']) ? $_FILES['imagen']['tmp_name'] : "";
    $imagen_contenido = file_get_contents($imagen_temporal);

    $stmt = $conn->prepare("UPDATE articulos SET id=:id, nombre=:nombre, descripcion=:descripcion, costo=:costo, 
        precio=:precio, cantidad=:cantidad ,imagen=:imagen WHERE articulos.id=:id");
    $stmt->bindValue(":id", $id);
    $stmt->bindValue(":nombre", $nombre);
    $stmt->bindValue(":descripcion", $descripcion);
    $stmt->bindValue(":costo", $costo);
    $stmt->bindValue(":precio", $precio);
    $stmt->bindValue(":cantidad", $cantidad);
    $stmt->bindValue(":imagen", $imagen_contenido, PDO::PARAM_LOB);
    $stmt->execute();
    if ($stmt) {
?>
        <div class="alert alert-succes alert-dismissible fade show" role="altert">
            <strong>Correcto!</strong> Se actualizo el cliente.

            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
    <?php
        header('location: index_articulos.php');
    } else {
    ?>
        <div class="alert alert-danger alert-dismissible fade show" role="alert">
            <strong>Error!</strong> No se pudo actualizar el cliente.
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
<?php

    }
}
include "header.php";
?>

<!-- Formulario de actualizar articulos -->
<div class="row">
    <div class="col-md-10">
        <h2>Actualizar Articulos</h2>
    </div>
    <form action="" method="post" enctype="multipart/form-data">
        <input type="hidden" id="id" name="id" value="<?php echo $id; ?>">
        <div class="mb-3">
            <label for="nombre" class="form-label">Nombre</label>
            <input type="text" class="form-control" name="nombre" id="inputnombre" placeholder="Ingresa el nombre del producto" required value="<?php echo $nombre ?>">
        </div>
        <div class="mb-3">
            <label for="descripcion" class="form-label">Descripcion</label>
            <input type="descripcion" class="form-control" name="descripcion" id="inputdescripcion" placeholder="Es un producto lacteo" required value="<?php echo $descripcion ?>">
        </div>
        <div class="mb-3">
            <label for="costo" class="form-label">Costo</label>
            <input type="number" class="form-control" name="costo" id="inputcosto" placeholder="120.00" required value="<?php echo $costo ?>">
        </div>
        <div class="mb-3">
            <label for="precio" class="form-label">Precio</label>
            <input type="number" class="form-control" name="precio" id="inputprecio" placeholder="140.00" required value="<?php echo $precio ?>">
        </div>
        <div class="mb-3">
            <label for="cantidad" class="form-label">Cantidad</label>
            <input type="number" class="form-control" name="cantidad" id="inputcantidad" placeholder="50: pzs" required value="<?php echo $cantidad ?>">
        </div>
        <div class="mb-3">
            <label for="imagen" class="form-label">Imagen</label>
            <input type="file" class="form-control" name="imagen" id="inputimagen" accept="image/*" required>
        </div>
        <button type="submit" class="btn btn-primary">Modificar</button>
    </form>
</div>
<?php
include "footer.php";
?>
```
