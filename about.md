---
layout: nil
title: About
permalink: /
---
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="description" content="Nicolò Martini ♥ unicorns">
		<meta name="author" content="Nicolò Martini">
		<meta name="viewport" content="width=device-width,initial-scale=1">
		<meta name="google-site-verification" content="-wDRU5qJ5MR2AtIa4t-JVRgU8hLs3Cyb4YmAKvKoT7I">
		<title>Nicolò Martini</title>
		<link rel="stylesheet" href="http://fonts.googleapis.com/css?family=Lato:100,300,400,700">
		<link rel="stylesheet" href="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.min.css">
		<link rel="stylesheet" href="main.css">
        <script>
          (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
          (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
          m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
          })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

          ga('create', 'UA-728709-3', 'martini.io');
          ga('send', 'pageview');

        </script>
	</head>
	<body class="credit">
		<div class="container">
			<h2 class="">Hi, I think coding is art.</h2>

			<div class="profile" itemscope itemtype="http://schema.org/Person">
				<a href="https://twitter.com/nicmartnic">
					<img src="http://www.gravatar.com/avatar/ef419cd042564d6d56fab6edfec7ad73.png?s=300" width="150" height="150" alt="Nicolò Martini" itemprop="image">
				</a>
				<div class="name" itemprop="name"><span>Nicolò</span><span>Martini</span></div>
			</div>
			<ul class="links">
				<li><a class="email" href="mailto:nicmartnic at gmail dot com">Email</a></li>
				<li><a href="http://twitter.com/nicmartnic">Twitter</a></li>
				<li><a href="https://github.com/nicmart">GitHub</a></li>
				<li><a href="/blog/"><b>Blog</b></a></li>
				<li><a href="http://stackoverflow.com/users/162087/nicolo-martini">StackOverflow</a></li>
				<li><a href="http://it.linkedin.com/in/nicmartnic">LinkedIN</a></li>
				<li><a href="https://github.com/nicmart/hi/raw/gh-pages/cv.pdf">CV</a></li>
			</ul>
		</div>
	</body>
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
	<script>
		$('.email').attr('href', 'mailto:' + 'nic' + 'olo' + '@' + 'martini' + '.' + 'io');

		$('.profile a').on('mouseover mouseout', function (e) {
			$(this).toggleClass('animated tada', e.type === 'mouseover');
		});

		setTimeout(function () {
			$('.credit').addClass('activate');
		}, 700);
	</script>
</html>
