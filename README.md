<?php
 	//Connect to the database
	$mysql=new mysqli("localhost","root","","task_db");
	if(isset($_GET['page']))
	{
		$page=$_GET['page'];
	}
	else
	{
		$page=1;
	}
	if($page==''|| $page==1)
	{
		$page1=0;
	}
	else
	{
		$page1=($page*10)-10;
	}
	//following command is written for prev and next button
	$sql='SELECT * from users';
			$data=$mysql->query($sql);
			$record=$data->num_rows;
			$record_pages=ceil($record/10);
			$prev=$page-1;
			$next=$page+1;

?>
<!DOCTYPE html>
<html>
<head>
	<title>Home</title>
	<meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
   <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.0/css/all.css" integrity="sha384-lZN37f5QGtY3VHgisS14W3ExzMWZxybE1SJSEsQp9S+oqd12jhcu+A56Ebc1zFSJ" crossorigin="anonymous">
</head>
<body>
	<?php 
			echo "<br>";
			//fetching user data from table users
			$sql='SELECT * from users LIMIT '.$page1.',5';
			$result=$mysql->query($sql);
	?>
	<div class="container">
		<div class="row p-2">
			<div class="col-md-12 col-sm-12 col-lg-12 col-xl-12 bg-light">
				<div class="head-test bg-light">
					<h5>Users</h5>
					<hr>
				</div>
				<?php  
					$counter=1;
					while($row=$result->fetch_assoc())
					{
						//fetching user data from table friends
						$frienddata='select * from friends where uid='.$row['uid'];
						$friend=$mysql->query($frienddata);
						echo '<div class="card-deck">
								<div class="card">
									<div class="card-body">
										<img class="rounded" src="images/'.$row['avtar'].'" width="270" height="270"><br>
									<h5>'.$row['fname'].'&nbsp;&nbsp;'.$row['lname'].'<h5>
									<div>
							  	<h6>Friends</h6>
							    <ol>';
							  while($fdata=$friend->fetch_assoc())
							{
								//fetching user data from table friendsoffriends
								// $friendoffrienddata='select * from friendsoffriends where uid='.$row['uid']'or fid='.$fdata['fid'];
								// $friendofq=$mysql->query($friendoffrienddat);
								echo '<li>'.$fdata['fname'].'</li>';
							}
										echo '</ol></div>
									</div>
								</div>
							  </div>';
						$counter++;

					}
					
					echo '<ol class="pagination">';
					if($prev>=1)
					{
						echo '<li class="page-item"><a class="page-link" href="?page='.$prev.'">Prev</a></li>';
					}
					if($record_pages>=2)
					{
						for($r=1;$r<=$record_pages;$r++)
						{
							$active=$r==$page?'class="active"':'';
							echo '<li class="page-item"><a class="page-link" href="?page='.$r.'"'.$active.'>'.$r.'</a></li>';
						}
					}
					if($next<=$record_pages && $record_pages>=2)
					{
						echo '<li class="page-item"><a class="page-link" href="?page='.$next.'">Next</a></li>';
					}
					echo '</ol>';
						?>
			</div>
		</div>
	</div>
</body>
</html>

//creating table users
create table users(uid int primary key auto_increment, fname varchar(199),lname varchar(199), avtar varchar(199));

//creating table friends
create table friends(uid int primary key auto_increment, fname varchar(199),lname varchar(199), avtar varchar(199), uid int, foreign key(uid) reference users(uid));

//creating table friendsoffriends
create table friendsoffriends(uid int primary key auto_increment, fname varchar(199),lname varchar(199), avtar varchar(199), uid int,fid int, foreign key(uid) reference users(uid),foreign key(fid) reference friendss(fid));
