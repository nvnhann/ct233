# ct233
[https://code.visualstudio.com/download](https://code.visualstudio.com/download)

```shell
sudo dpkg -i package_file.deb
```

```git
git init
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git add .           #Lưu ý có dấu chấm sau add
git commit -m "Initial import of Hello Heroku"
```

Liệt kê danh sách các ứng dụng mình đã tạo:

```heroku apps```

Xem thông tin chi tiết về ứng dụng example

```heroku apps:info --app example```

Xóa ứng dụng example

```heroku apps:destroy --app example```

Thêm một add-on để sử dụng dịch vụ database Postgres cung cấp miễn phí bởi Heroku

```heroku addons:create heroku-postgresql:hobby-dev
heroku pg:wait
```

Xem thông tin về database đã tạo ra cho ứng dụng

```heroku pg:info```

Xem chuỗi kết nối vào database DATABASE_URL từ ứng dụng

```heroku config```

=> Kết qủa DATABASE_URL: postgres://jozemgtezsssog:iSHEmlrZsA96srKxAAEr8W0ywY@ec2-54-75-242-208.eu-west-1.compute.amazonaws.com:5432/dfnugibvvam3o9 , với ý nghĩa như sau:

Username:Password: jozemgtezsssog:iSHEmlrZsA96srKxAAEr8W0ywY

Database server:cồng: ec2-54-75-242-208.eu-west-1.compute.amazonaws.com:5432

Database Name: dfnugibvvam3o9

Tài liệu chi tiết tại: [https://devcenter.heroku.com/articles/heroku-postgresql#provisioning-the-add-on](https://devcenter.heroku.com/articles/heroku-postgresql#provisioning-the-add-on)

```shell
git add  .   # Chú ý dấu chấm .
git commit  -m " Thêm tập tin createtable.php"
git push heroku main
```
```index.php```

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nguyen Van Nhan - B1809272</title>
</head>

<body>
    <a href="./create_db.php">Create database</a>
    <br />
    <a href="./add_student.php">Add student</a>
    <br />
    <a href="./list_students.php">List student</a>
</body>

</html>
```

```create_db.php```

```php
<?php
function pg_connection_string_from_database_from_url(){
    extract(parse_url($_ENV["DATABASE_URL"]));
    return "user=$user password=$pass host=$host dbname=" .substr($path, 1);
}

$db = pg_connect(pg_connection_string_from_database_from_url());

if(!$db){
    echo "Error: Unable to open database \n";
}else{
    echo "Opended database successfully\n";
}

$sql = "CREATE TABLE students (mssv text, hoten text);";

echo $sql;

$ret = pg_query($db, $sql);

if(!$ret){
    echo pg_last_error($db);
}else{
    echo "create table successfully!!\n";
}

pg_close($db);

?>
```

```add_student.php```

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Add student</title>
</head>

<body>
    <div style='margin: 3em auto; width: 30%'>
        <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post">
            <label for="username">MSSV</label> <br />
            <input type="text" name="mssv" id="mssv" placeholder='MSSV'><br />
            <label for="hoten">Ho ten</label> <br />
            <input type="text" name="hoten" id="hoten" placeholder='Ho ten'><br />
            <button type="submit">Submit</button>
        </form>
        <br />
        <a href="./list_students.php">List student</a>
        <br />
        <?php
        if (isset($_POST['mssv'])) {
            $mssv = $_POST['mssv'];
            $hoten = $_POST['hoten'];

            function pg_connection_string_from_database_from_url()
            {
                extract(parse_url($_ENV["DATABASE_URL"]));
                return "user=$user password=$pass host=$host dbname=" . substr($path, 1);
            }

            $db = pg_connect(pg_connection_string_from_database_from_url());

            if (!$db) {
                echo "Error: Unable to open database \n";
            } else {
                echo "Opended database successfully \n";
            }

            $sql = "INSERT INTO students (mssv, hoten) VALUES ('$mssv','$hoten'); ";
            echo $sql;
            $ret = pg_query($db, $sql);

            if (!$ret) {
                echo pg_last_error($db);
                echo "loi";
            } else {
                echo "Insert successfully \n";
            }
            pg_close($db);
        }
        ?>

    </div>

</body>

</html>
```

```list_students.php```

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>List student</title>
</head>

<body>
    <?php
    function pg_connection_string_from_database_from_url()
    {
        extract(parse_url($_ENV["DATABASE_URL"]));
        return "user=$user password=$pass host=$host dbname=" . substr($path, 1);
    }

    $db = pg_connect(pg_connection_string_from_database_from_url());

    if (!$db) {
        echo "Error: Unable to open database \n";
    } else {
        echo "Opended database successfully\n";
    }
    $sql = "SELECT * from students";
    echo "<br/>";
    echo $sql;

    $ret = pg_query($db, $sql);
    if (!$ret) {
        echo pg_last_error($db);
        exit();
    }


    ?>

    <table border="1" cellspacing="2" cellpadding="2">
        <tr>
            <td>MSSV</td>
            <td>Ho ten</td>
        </tr>

        <?php
        while ($row = pg_fetch_assoc($ret)) {
            echo "
                    <tr>
                        <td> " . $row['mssv'] . " </td>
                        <td>" . $row['hoten'] . " </td>
                    </tr>
                    
                    ";
        }
        pg_close($db);
        ?>

        <br />
        <a href="index.php">Index page</a>

    </table>
</body>

</html>

```


