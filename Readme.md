# Demo 
<?php
$username = $password = $name = $confirm_password = "";
$username_err = $password_err = $name_err = $confirm_password_err = "";//for error declaration of verialble
if ($_SERVER['REQUEST_METHOD'] == "POST" )
{
    //CHECK IF USERNAME IS EMPTY
    if(empty(trim($_POST["username"]))){
        $username_err = "username cannot be blank";   
    }
    else{
        $sql = "SELECT id FROM user WHERE  username = ?";
        $stmt = mysqli_prepare($con, $sql); //binding veriable to sql
        if($stmt){
            mysql_stmt_bind_param($stmt, "s", $parm_username);
            //Set the value of parm username
            $parm_username = trim($_POST['username']);
            //try to nexecute the statement
            if(mysqli_stmt_execute($stmt)){
                mysqli_stmt_store_result($stmt);
            }
                if(mysqli_stmmt_num_rows($stmt) == 1){//checking username is taken or available or not
                    $username_err = "the username is already taken";
                     }
            else{
                $username = trim($_POST['username']);
               }

        }
        else{
            echo"something went wrong";
        }
    }
    }
    
    mysqli_stmt_close($stmt);

   //check for name
  //checking for empty
  if(empty(trim($_POST["name"]))){

    $name_err = "name cannot be blank";
  }
  elseif(strlen(trim($_POST['name'])) < 5){
    $name_err = "Name cannot be less than 5 charecter";
    }
   else{
    $name = trim($_POST['name']);
  }



  //check for password
  //checking for empty
  if(empty(trim($_POST["password"]))){
    $password_err = "password cannot be blank";
  }
   //checking the length of password
 elseif(strlen(trim($_POST['password'])) < 3){
    $password_err = "password cannot be less than 3 charecter";
 }
 else{
      $password = trim($_POST['password']);
 }
  //checking for confirm password although done from javascript.
 if(trim($_POST["password"] != trim($_POST['confirm_password']) )){
    $password_err = "password should be matching";
 }
 //if there are no error and insert into database
 if(empty($username_err) && empty($name_err) && empty($password_err) && empty ($confirm_password_err)){
    $sql = "INSERT INTO user (username, name, password VALUES (?,?,?)";
    $stmt = mysqli_prepare($con, $sql);
    if($stmt){
        mysql_stmt_bind_param($stmt, "sss",$parm_username, $parm_name, $parm_password);
        //set these parameter
        $parm_username = $username;
        $parm_name = $name;
        $parm_password = password_hash($password,PASSWORD_DEFAULT);//function for storing hash password in database.
        //try to execute querry
        if(mysqli_stmt_execute($stmt)){
            header("location:login.html");

        }
        else{
            echo"something went wrong..cannot Redirect!!!!!";

        }

        
    }
    mysqli_stmt_close($stmt);


    }
 mysqli_close($con);

?>
