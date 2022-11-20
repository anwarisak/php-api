## register_user
````
function register_user($conn){
    
    extract($_POST);
    $data = array();
    $error_array = array();
    $new_id = generate($conn);

    $file_name = $_FILES['image']['name'];
    $file_type = $_FILES['image']['type'];
    $file_size = $_FILES['image']['size'];
    
    $save_name = $new_id . ".jpg";

    
    //allow image
    $allowimages =["image/png", "image/jpg", "image/jpeg"];
    $max_size = 3 * 1024 * 1024;
    if(in_array($file_type, $allowimages)){
        if ($file_size > $max_size){
        //"  $error_array['file_size'] = "Image size must be less than ".$max_size;
        $error_array[] = "file size must less than ".$max_size;

        }

    }else{
        $error_array[] = "Image type not allowed";

    }
    if(count($error_array)<= 0){
        $query = "insert into user (id,username,password,image)values('$new_id','username', MD5('$password'),'$save_name')";
        $result = $conn->query($query);
    
    
        if($result){
    
            move_uploaded_file($_FILES ['image']['tmp_name'], "../uploads/".$save_name);
            $data = array("status" => true, "data" => "registered successfully");

    
        }else{
            $data = array("status" => false, "data"=> $conn->error);
                 
        }
    }else{
        $data = array("status" => false, "data"=> $error_array);

    }

    

    echo json_encode($data);
}

```
