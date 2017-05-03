# get-any-URL-meta-information
This code show how to get URL meta tags information by open graph way but from scratch with a little bit of design 


How to use 
----

- Open the files from your local host then put in the url text any website url you want to get it's meta information 
- The Style for the form and the table of content you'll find it in the uploaded files 
=======

<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>URL SEEKER</title>
		<link rel="stylesheet" href="bootstrap.css">
		<link rel="stylesheet" href="style.css">
	</head>
	<body>
		
		<div class='container'>
			<h1 class="text-center">URL INFORMATION</h1>
			<div class='row'>
				<div class='col-md-12'>
					<form action="" method="POST">
						<div class="input-group">
					      <input type="url" name='url' class="form-control" placeholder="Enter URL Here" required>
					      <span class="input-group-btn">
					        <button class="btn btn-success btn-lg" name='go' type="submit">Search !</button>
					      </span>
					    </div>
					</form>

					<?php

					function URL($url)
					{

						// Built-in Class To Parse HTML Pages
					    $parse = new DomDocument;

					    $page = file_get_contents($url);

					    // Load The Page And Take Care OF Symbols And These Stuff
					    @$parse->loadHTML(mb_convert_encoding($page, 'HTML-ENTITIES', 'UTF-8'));
					     
					    // Give Path To Our Page Contents To Get Through it 
					    $path = new DOMXPath($parse);

					    // Pattern To Get The Open Graph (og) Properties
					    $metaTags = $path->query('//*/meta');

					    // Make Array To Our Properties Which We Got 
					    $info = array();

					    foreach($metaTags as $meta){

					    	// Get The Properties From Our Query
					    	$fetch = $meta->getAttribute('property');

					        // Get Properties Without OG IF Exists 
					        $property = str_replace('og:', '', $fetch);

					        // Get Content
					        $content = $meta->getAttribute('content');
					        $info[$property] = $content;
					    }

					    // Return Results 
					    return $info;
					}

					// Initialize My Inputs in 
					$urlText = isset($_POST['url']) ? $_POST['url'] : false;
					$button  = isset($_POST['go'])  ? $_POST['go']  : false;
					$false   = "<span class='notSupported'> Not Supported !</span>";

					// IF Submit Button Clicked Then Show Me The Results
					if(isset($urlText) && isset($button) && !empty($urlText)) {

						// Call The Function 
						$info = URL($urlText); ?>

						<div class="table-responsive">
			                <table class="myTable table table-bordered text-center">
			                    <tr>
			                    	<td>Site Name</td>
			                        <td>Title</td>
			                        <td>Description</td>
			                        <td>Type</td>
			                        <td>Image</td>
			                        
			                    </tr>

			                    <tr>
			                    	<!-- Show Some OF Meta Tags -->
			                    	<td><?php echo isset($info['site_name']) ? $info['site_name'] :$false ?></td>

			                        <td><?php echo isset($info['title']) ? $info['title'] : $false ?></td>

			                        <td><?php echo isset($info['description']) ? $info['description'] : $false ?></td>

			                        <td><?php echo isset($info['type']) ? $info['type'] : $false ?></td>

			                        <td><?php echo isset($info['image']) ? "<img src = " . $info['image'] .">" : $false ?></td>
			                    </tr>
			                </table>
			            </div>
					<?php
					}
					?>
				</div>
			</div>
		</div>
	</body>
</html>


