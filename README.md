<?php
$servername = 'localhost';
$username = 'root';
$password = '';
$database = 'meNotes';
$conn = mysqli_connect($servername, $username, $password, $database);
if (!$conn)
    die('connection failed<br>' . mysqli_connect_error());

$insert = false;
$upadte = false;
$edit = false;
$delete = false;

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    if (isset($_POST['snuEdit'])) {
        echo 'yes';
        exit();
    }
    $title = $_POST['title'];
    $desc = $_POST['desc'];
    $sql = "INSERT INTO `notes_list` (`snu`, `title`, `description`, `time stamp`) VALUES (NULL, '$title', '$desc', current_timestamp());";
    $result = mysqli_query($conn, $sql);
    if ($result)
        $insert = true;
}
?>

<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>meNotes</title>

    <link rel="stylesheet" href="//cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-KK94CHFLLe+nY2dmCWGMq91rCGa5gtU4mk92HdvYe+M/SXH301p5ILy+dN9+nJOZ" crossorigin="anonymous">

</head>

<body>

    <!-- Modal -->
    <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Edit this note.</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form method="post" action="index.php?update = true">
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
                        <button type="submit" class="btn btn-primary">Update</button>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div>
            </div>
        </div>
    </div>
    <?php
    if ($insert)
        echo '<div class="alert alert-warning alert-dismissible fade show" role="alert">
            <strong>Holy guacamole!</strong> kabhi khai hai guacamole? mat khana.
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
                    <!-- <form class="d-flex" role="search">
                        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
                        <button class="btn btn-outline-success" type="submit">Search</button>
                    </form> -->
                </div>
            </div>
        </nav>
        <div class="container mt-3">
            <h2>add notes</h2>
            <form method="post" action="index.php?add = true">
                <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label">note title</label>
                    <input type="text" class="form-control" name="title" aria-describedby="emailHelp">
                </div>
                <div class="mb-3">
                    <label for="exampleInputPassword1" class="form-label">note description</label>
                    <input type="text" class="form-control" name="desc">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlTextarea1" class="form-label">notes</label>
                    <textarea class="form-control" name="content" id="content" rows="3"></textarea>
                </div>
                <button type="submit" class="btn btn-primary">Add</button>
            </form>
        </div>
        <br>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-ENjdO4Dr2bkBIFxQpeoTz1HIcje39Wm4jDKdf19U8gI4ddQ3GYNS7NTKfAdVQSZe"
            crossorigin="anonymous"></script>
        <table class="table" id="myTable">
            <thead>
                <tr>
                    <th scope="col">S.Nu</th>
                    <th scope="col">Title</th>
                    <th scope="col">Description</th>
                    <th scope="col">Content</th>
                </tr>
            </thead>
            <tbody>
                <?php
    $snu = 0;
    $sql = 'SELECT * FROM `notes_list`';
    $result = mysqli_query($conn, $sql);
    while ($row = mysqli_fetch_assoc($result)) {
        $snu++;
        echo ' <tr>
                    <td scope="col">' . $snu . '</td>
                    <td scope="col">' . $row['title'] . '</td>
                    <td scope="col">' . $row['description'] . '</td>
                    <td scope="col"><button class="btn edit btn-sm btn-primary" id=' . $row['snu'] . '>Edit</button><button class="delete btn btn-sm btn-primary">Delete</button></td>
                  </tr>';
    }
    ?>
            <script src="https://code.jquery.com/jquery-3.6.4.js"
                integrity="sha256-a9jBBRygX1Bh5lt8GZjXDzyOB+bWve9EiO7tROUtj/E=" crossorigin="anonymous"></script>
            <script src="//cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
            <script>
                let table = new DataTable('#myTable');
            </script>
        </tbody>
    </table>
    <script>
        edits = document.getElementsByClassName('edit');
        Array.from(edits).forEach((element) => {
            element.addEventListener('click', (e) => {
                tr = e.target.parentNode.parentNode;
                title = tr.getElementsByTagName('td')[1].innerHTML;
                desc = tr.getElementsByTagName('td')[2].innerHTML;
                titleEdit.value = title;
                descEdit.value = desc;
                snuEdit.value = e.target.id;
                $('#editModal').modal('toggle');
                // console.log(e.target.id);

            })
        })
    </script>
</body>

</html>
