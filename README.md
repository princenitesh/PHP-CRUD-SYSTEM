# PHP-CRUD-SYSTEM
LEARN HOW TO WORK CRUD IN PHP 

//CODE STATRT FROM HERE >>>>>>>>>>START 
//FIRST CREATE A FILE NAME CONFIG.PHP FOR CONFIGRATION TO SQL DATABASE:--

//CONFIG.PHP

$databaseHost = "localhost";  // by defult when you using localhost server 
$databaseUsername = "root";  // by defult
$databasePassword = "" ; // by defult empty
$databaseName = "abcd";  // not abcd you type your database name in mysql

$mysqli = mysqli_connect($databaseHost , $databaseUsername ,$databasePassword , $databaseName);  //connection b/w config.php to your database 

//check your connection_

if($mysqli){
  echo "connected ! ";
}else{
echo " not connected ! " ;
}

//now create 2nd file name connection.php 

<?php

include 'config.php';

if (isset($_POST['submit'])) 
{
    $id = $_POST['id'];
    $name = $_POST['name'];
    $email = $_POST['email'];
    $contact = $_POST['mobial'];
     $filename = $_FILES["choosefile"]["name"];
    $tempname = $_FILES["choosefile"]["tmp_name"];   
        $folder = "upload/".$filename;
        $move=move_uploaded_file($tempname, $folder);
 if($move){
$result = mysqli_query($mysqli, "INSERT INTO `data`(`ID`, `name`, `email`, `contact`, `image`) VALUES ('$id','$name','$email','$contact','$filename')");

        if ($result) {
            echo "done";
        } else {
            echo "not done";
        }
    }
}
?>


//now you create 3rd file name index.php where your form is present in html format:-

<!DOCTYPE html>
<!--
Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
Click nbfs://nbhost/SystemFileSystem/Templates/Project/PHP/PHPProject.php to edit this template
-->
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
    <center>
    <h1>welcome to registation form</h1>
    </center>
    <center>
        <form method="post" action="connection.php" enctype="multipart/form-data">
            <input type="hidden" name="id">
            NAME:-
            <input type="text" name="name" requred><br><br>
             EMAIL:-
             <input type="email" name="email" requred><br><br><!-- comment -->
              CONTACT:-
            <input type="number" name="mobial" requred><br><br>
             IMAGE:-
            <input type="file" name="choosefile" requred><br><br>
            
            <input type="submit" name="submit">
        </form>
    </center>
        <?php
        // put your code here
        ?>
    </body>
</html>


//now create one another name data_fetch.php //where we fetch our data to database in table formate :----

<!DOCTYPE html>
<html lang="en">
<head> <meta charset="UTF-8"> 
<title>data_fetch</title>
 <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.css"> <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script> 
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.js"></script>
</head>
<body> 
<table class="table table-bordered" border="2"> 
<td colspan="5">
<center>
<h3>Previously Uploaded | <a href="crud.php" >Add Data</a>
</td>
<center>
	<tr>
<th>NAME</th>
<th>EMAIL</th>
<th>CONTACT</th>
<th>IMAGE</th>
<th>DELETE / UPDATE</th>
</tr>
</center>

<?php
include 'config.php';
//query

$query = "SELECT * FROM `data`";


if(isset($_GET['delete']))
{
$id = $_GET['delete'];
$mysqli->query("DELETE FROM `data` WHERE ID=$id");

}
$result = $mysqli->query($query);

	if ($result->num_rows > 0)
	{ 
		// OUTPUT DATA OF EACH ROW
		while($row = $result->fetch_assoc())
                { ?>

<tr>
<td><p><?php echo $row['name']; ?></p></td>
<td><P><?php echo $row['email'];?></p></td>
<td><?php echo $row['contact']; ?></td>
<td><img width="auto" src="upload/<?php echo $row['image']?>" width="450" height="200" />
</td> 
<td width="10%">
    <a href="datafetch.php?delete=<?php echo $row['ID'];?>"><button style="background-color: #737CA1;color: white;">Delete</button></a>
    <a href="update.php?update=<?php echo $row['ID'];?>"><button style="background-color: #4863A0;color: white;">Edit</button></a>
</td>
</tr> <?php } } ?> 
</tr>
</table>
</div>
</body>
</html>


               
