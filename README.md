<!DOCTYPE html>
<html>
<head>
  <title>EMP Management</title>
</head>
<body>
  <h1>EMP Management</h1>
  
  <form method="POST">
    <h2>Add EMP</h2>
    <label>First name:</label>
    <input type="text" name="first_name"><br>
    <label>Last name:</label>
    <input type="text" name="last_name"><br>
    <label>Phone:</label>
    <input type="text" name="phone"><br>
    <input type="submit" name="add_EMP" value="Add EMP">
</form>
  
  <?php
  // Create a connection to the database
  $servername = "localhost";
  $username = "rajesh";
  $password = "r@jesh";
  $dbname = "servers";
  $conn = new mysqli($servername, $username, $password, $dbname);
  
  // Check for errors in connection
  if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
  }
  
  // Handle form submissions
  if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Check if the form was submitted to add a new EMP
    if (isset($_POST["add_EMP"])) {
      // Get the data from the form
      $first_name = $_POST["first_name"];
      $last_name = $_POST["last_name"];
      $phone = $_POST["phone"];
      
      // Insert the data into the database
      $sql = "INSERT INTO EMP (first_name, last_name, phone) VALUES ('$first_name', '$last_name', '$phone')";
      if ($conn->query($sql) === TRUE) {
        echo "New EMP added successfully";
      } else {
        echo "Error adding EMP: " . $conn->error;
      }
    }
    
    // Check if the form was submitted to edit an existing EMP
    if (isset($_POST["edit_EMP"])) {
      // Get the data from the form
      $id = $_POST["id"];
      $first_name = $_POST["first_name"];
      $last_name = $_POST["last_name"];
      $phone = $_POST["phone"];
      
      // Update the data in the database
      $sql = "UPDATE EMP SET first_name='$first_name', last_name='$last_name', phone='$phone' WHERE id=$id";
      if ($conn->query($sql) === TRUE) {
        echo "EMP updated successfully";
      } else {
        echo "Error updating EMP: " . $conn->error;
      }
    }
    
    // Check if the form was submitted to delete an EMP
    if (isset($_POST["delete_EMP"])) {
      // Get the ID of the EMP to delete
      $id = $_POST["id"];
      
      // Delete the EMP from the database
      $sql = "DELETE FROM EMP WHERE id=$id";
      if ($conn->query($sql) === TRUE) {
        echo "EMP deleted successfully";
      } else {
        echo "Error deleting EMP: " . $conn->error;
      }
    }
  }
  
  // Retrieve data from the database
  $sql = "SELECT id, first_name, last_name, phone FROM EMP";
  $result = $conn->query($sql);
  
  if ($result->num_rows > 0) {
    // Output data of each row
    while($row = $result->fetch_assoc()) {
      echo "<form method='POST'>";
      echo "<input type='hidden' name='id' value='" . $row["id"] . "'>";
      echo "First name: <input type='text' name='first_name' value='" . $row["first_name"] . "'><br>";
      echo "Last name: <input type='text' name='last_name' value='" . $row["last_name"] . "'><br>";
      echo "Phone: <input type='text' name='phone' value='" . $row["phone"] . "'><br>";
      echo "<input type='submit' name='edit_EMP' value='Edit'>";
      echo "<input type='submit' name='delete_EMP' value='Delete'>";
      echo "</form>";
    }
  } else {
    echo "failed";
  }
  
?>
  
</body>
</html>
