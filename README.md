<?php
$servername = 'localhost';
$username = 'root';
$password = '';
$database = 'meNotes';
$conn = mysqli_connect($servername, $username, $password, $database);
if (!$conn)
    die('connection failed<br>' . mysqli_connect_error());

$insert = false;
$update = false;
$delete = false;

if ($_SERVER['REQUEST_METHOD'] == 'POST') {

    if (isset($_POST['snuEdit'])) {
        //update the record
        $snu = $_POST['snuEdit'];
        $title = $_POST['titleEdit'];
        $desc = $_POST['descEdit'];
        $sql = "UPDATE `notes_list` SET `title` = '$title', `description` = '$desc' WHERE `snu` = '$snu'; ";
        $result = mysqli_query($conn, $sql);
        $update = true;
    } elseif (isset($_POST['snuDel'])) {
        $snu = $_POST['snuDel'];
        $sql = "DELETE FROM `notes_list` WHERE `notes_list`.`snu` = $snu";
        $result = mysqli_query($conn, $sql);
        $delete = true;
    } else {
        $title = $_POST['title'];
        $desc = $_POST['desc'];
        $sql = "INSERT INTO `notes_list` (`snu`, `title`, `description`, `time stamp`) VALUES (null, '$title', '$desc', current_timestamp());";
        $result = mysqli_query($conn, $sql);
        if ($result)
            $insert = true;
    }

}
?>

<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>meNotes</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/js/bootstrap.bundle.min.js"></script>
</head>

<body style="background-color: rgb(0 0 0); color: white;">

    <!--Delete Modal -->
    <div style="color: black;" class="modal fade" id="deleteModal" tabindex="-1" aria-labelledby="exampleModalLabel"
        aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Delete this note.</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <form method="post" action="index.php?yes = true">
                    <input type="hidden" name="snuDel" id="snuDel">
                    <div class="modal-body">
                        Are you sure that you want to delete this note?
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">No</button>
                        <button type="submit" class="btn btn-primary" id="yes">Yes</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Edit Modal -->
    <div style="color: black;" class="modal fade" id="editModal" tabindex="-1" aria-labelledby="exampleModalLabel"
        aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Edit this note.</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <form method="post" action="index.php?save = true">
                    <div class="modal-body">
                        <input type="hidden" name="snuEdit" id="snuEdit">
                        <div class="mb-3">
                            <label for="exampleInputEmail1" class="form-label">note title</label>
                            <input type="text" class="form-control" name="titleEdit" id="titleEdit"
                                aria-describedby="emailHelp">
                        </div>
                        <div class="mb-3">
                            <label for="exampleInputPassword1" class="form-label">note description</label>
                            <input type="text" class="form-control" name="descEdit" id="descEdit">
                        </div>
                        <div class="mb-3">
                            <label for="exampleFormControlTextarea1" class="form-label">notes</label>
                            <textarea class="form-control" name="content" id="content" rows="3"></textarea>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                        <button type="submit" id="save" class="btn btn-primary">Save changes</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    <?php
    if ($insert)
        echo '<div class="alert alert-warning alert-dismissible fade show" role="alert">
            <strong>Holy guacamole!</strong> Your note has been added successfully.
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
          </div>';
    else if ($update)
        echo '<div class="alert alert-warning alert-dismissible fade show" role="alert">
            <strong>Holy guacamole!</strong> Your note has been updated successfully.
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
          </div>';
    else if ($delete)
        echo '<div class="alert alert-warning alert-dismissible fade show" role="alert">
            <strong>Holy guacamole!</strong> Note has been deleted successfully.
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
          </div>'
            ?>
                <nav class="navbar navbar-expand-lg bg-body-tertiary">
                    <div class="container-fluid">
                        <a class="navbar-brand" href="#">meNotes</a>
                        <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                            data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                            aria-label="Toggle navigation">
                            <span class="navbar-toggler-icon"></span>
                        </button>
                        <div class="collapse navbar-collapse" id="navbarSupportedContent">
                            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                                <li class="nav-item">
                                    <a class="nav-link active" aria-current="page" href="#">Home</a>
                                </li>
                                <li class="nav-item">
                                    <a class="nav-link" href="#">about</a>
                                </li>
                                <li class="nav-item">
                                    <a class="nav-link" href="#">contact us</a>
                                </li>
                            </ul>
                        </div>
                    </div>
                </nav>
                <div class="container mt-3">
                    <h2>add notes</h2>
                    <form method="post" action="index.php?add=true">
                        <input type="hidden" name="snu" id="snu">
                        <div class="mb-3">
                            <label for="exampleInputEmail1" class="form-label">note title</label>
                            <input type="text" class="form-control" name="title" aria-describedby="emailHelp" required>
                        </div>
                        <div class="mb-3">
                            <label for="exampleInputPassword1" class="form-label">note description</label>
                            <input type="text" class="form-control" name="desc" required>
                        </div>
                        <div class="mb-3">
                            <label for="exampleFormControlTextarea1" class="form-label">notes</label>
                            <textarea class="form-control" name="content" id="content" rows="3"></textarea>
                        </div>
                        <button type="submit" name="add" class="btn btn-primary"
                            style="background-color: rgb(16 185 129);">Add</button>
                    </form>
                </div>
                <br>

                <table style="color: white;" class="table" id="myTable">
                    <thead>
                        <tr>
                            <th scope="col">S.Nu</th>
                            <th scope="col">Title</th>
                            <th scope="col">Description</th>
                            <th scope="col">Options</th>
                        </tr>
                    </thead>
                    <tbody>
                <?php
    $snu = 1;
    $sql = 'SELECT * FROM `notes_list`';
    $result = mysqli_query($conn, $sql);
    while ($row = mysqli_fetch_assoc($result)) {

        echo ' <tr>
        <td scope="col">' . $snu . '</td>
        <td scope="col">' . $row['title'] . '</td>
        <td scope="col">' . $row['description'] . '</td>
        <td scope="col"><button style="background-color: rgb(45 212 191);" class="btn edit btn-sm btn-primary" data-bs-toggle="modal" data-bs-target="#editModal" id=' . $row['snu'] . '>Edit</button> <button data-bs-toggle="modal" data-bs-target="#deleteModal" id = "d' . $row['snu'] . '" class="delete btn btn-sm btn-primary" style="background-color: rgb(56 189 248);">Delete</button></td>
        </tr>';

        $snu++;
    }
    ?>
        </tbody>
    </table>

    <script>
        edits = document.getElementsByClassName('edit');
        Array.from(edits).forEach(element => {
            element.addEventListener('click', (e) => {
                tr = e.target.parentNode.parentNode;
                // console.log(tr);
                title = tr.getElementsByTagName('td')[1].innerHTML;
                desc = tr.getElementsByTagName('td')[2].innerHTML;
                titleEdit.value = title;
                descEdit.value = desc;
                snuEdit.value = e.target.id;
                // console.log(titleEdit.value,descEdit.value,snuEdit.value);
                // console.log(e.target.id);

            })
        })

        deletes = document.getElementsByClassName('delete');
        Array.from(deletes).forEach(element => {
            element.addEventListener('click', (e) => {
                snuDel.value = e.target.id.substr(1);
                // console.log(snuDel.value);
            })
        })

    </script>
</body>

</html>
