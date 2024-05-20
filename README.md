# post

<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Direktori tempat file akan disimpan
    $targetDir = "uploads/";
    
    // Pastikan direktori tujuan ada
    if (!is_dir($targetDir)) {
        mkdir($targetDir, 0777, true);
    }

    // Target file path
    $targetFile = $targetDir . basename($_FILES["file"]["name"]);
    $uploadOk = 1;

    // Memeriksa apakah file sebenarnya adalah gambar atau bukan
    if (isset($_POST["submit"])) {
        $check = getimagesize($_FILES["file"]["tmp_name"]);
        if ($check !== false) {
            echo "File is an image - " . $check["mime"] . ".<br>";
            $uploadOk = 1;
        } else {
            echo "File is not an image.<br>";
            $uploadOk = 0;
        }
    }

    // Memeriksa apakah file sudah ada
    if (file_exists($targetFile)) {
        echo "Sorry, file already exists.<br>";
        $uploadOk = 0;
    }

    // Memeriksa ukuran file (dalam byte, contoh di bawah ini membatasi ukuran maksimal 500KB)
    if ($_FILES["file"]["size"] > 500000) {
        echo "Sorry, your file is too large.<br>";
        $uploadOk = 0;
    }

    // Memeriksa tipe file (opsional, bisa disesuaikan dengan kebutuhan)
    $fileType = strtolower(pathinfo($targetFile, PATHINFO_EXTENSION));
    $allowedTypes = array("jpg", "png", "jpeg", "gif");
    if (!in_array($fileType, $allowedTypes)) {
        echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.<br>";
        $uploadOk = 0;
    }

    // Memeriksa apakah uploadOk sudah diatur ke 0 oleh suatu kesalahan
    if ($uploadOk == 0) {
        echo "Sorry, your file was not uploaded.<br>";
    } else {
        if (move_uploaded_file($_FILES["file"]["tmp_name"], $targetFile)) {
            echo "The file ". htmlspecialchars(basename($_FILES["file"]["name"])) . " has been uploaded.<br>";
        } else {
            echo "Sorry, there was an error uploading your file.<br>";
        }
    }
} else {
    echo "No file uploaded.";
}
?>
