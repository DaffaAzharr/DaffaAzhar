# DaffaAzhar
Crud Barang 

1.index.php

<?php
$host = "localhost";
$username = "root";
$password = "";
$dbname = "your_database_name";

$conn = new mysqli($host, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>

2.tambahBarang &  SimpanBarang

<?php
include 'connect.php';

if(isset($_POST['submit'])) {
    $nama_barang = $_POST['nama_barang'];
    $harga = $_POST['harga'];
    $stok = $_POST['stok'];
    $foto = $_POST['foto'];

    $sql = "INSERT INTO DataBarang (nama_barang, harga, stok, foto) 
            VALUES ('$nama_barang', '$harga', '$stok', '$foto')";

    if ($conn->query($sql) === TRUE) {
        echo "New record created successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
}
?>
<form action="insert.php" method="POST">
    Nama Barang: <input type="text" name="nama_barang" required><br>
    Harga: <input type="number" name="harga" required><br>
    Stok: <input type="number" name="stok" required><br>
    Foto: <input type="text" name="foto" required><br>
    <input type="submit" name="submit" value="Simpan">
</form>



3.listBarang 

<?php
include 'connect.php';

$sql = "SELECT * FROM DataBarang";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        echo "ID: " . $row["id_barang"] . " - Nama: " . $row["nama_barang"] . " - Harga: " . $row["harga"] . " - Stok: " . $row["stok"] . " - Foto: " . $row["foto"] . "<br>";
    }
} else {
    echo "0 results";
}

$conn->close();
?>

4,editBarang 

<?php
include 'connect.php';
$id = $_GET['id'];

$sql = "SELECT * FROM DataBarang WHERE id_barang='$id'";
$result = $conn->query($sql);
$row = $result->fetch_assoc();
?>

<form action="update.php" method="POST">
    <input type="hidden" name="id_barang" value="<?php echo $row['id_barang']; ?>">
    Nama Barang: <input type="text" name="nama_barang" value="<?php echo $row['nama_barang']; ?>"><br>
    Harga: <input type="number" name="harga" value="<?php echo $row['harga']; ?>"><br>
    Stok: <input type="number" name="stok" value="<?php echo $row['stok']; ?>"><br>
    Foto: <input type="text" name="foto" value="<?php echo $row['foto']; ?>"><br>
    <input type="submit" name="update" value="Update">
</form>

5,updateBarang

<?php
include 'connect.php';

if(isset($_POST['update'])) {
    $id_barang = $_POST['id_barang'];
    $nama_barang = $_POST['nama_barang'];
    $harga = $_POST['harga'];
    $stok = $_POST['stok'];
    $foto = $_POST['foto'];

    $sql = "UPDATE DataBarang SET nama_barang='$nama_barang', harga='$harga', stok='$stok', foto='$foto' WHERE id_barang='$id_barang'";

    if ($conn->query($sql) === TRUE) {
        echo "Record updated successfully";
    } else {
        echo "Error updating record: " . $conn->error;
    }

    $conn->close();
}
?>

6,deleteBarang

<?php
include 'connect.php';

$id = $_GET['id'];

$sql = "DELETE FROM DataBarang WHERE id_barang='$id'";

if ($conn->query($sql) === TRUE) {
    echo "Record deleted successfully";
} else {
    echo "Error deleting record: " . $conn->error;
}

$conn->close();
?>

