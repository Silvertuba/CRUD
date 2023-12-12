
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
