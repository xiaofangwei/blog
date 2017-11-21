
<html lang="zh-CN">
<head>
<meta charset="uft-8">
<title>相册</title>
<link rel="stylesheet" href="css/demo-styles.css">
<link rel="stylesheet" href="css/styles.css">
</head>

<body>
<h1 style="margin: 40px; font: 32px Microsoft Yahei; text-align: center;">相册</h1>

<div id="gallery-container">
	<ul class="items--small">
		<li class="item"><a href="#"><img src="images/small-1.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-2.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-3.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-4.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-5.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-6.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-7.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-8.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-9.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-10.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-11.png" alt="" /></a></li>
		<li class="item"><a href="#"><img src="images/small-12.png" alt="" /></a></li>
	</ul>
	<ul class="items--big">
		<li class="item--big"> <a href="http://www.dowebok.com/"  target="_blank">
			<figure> <img src="images/big-1.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="http://www.dowebok.com/48.html"  target="_blank">
			<figure> <img src="images/big-2.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-3.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-4.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-5.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-6.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-7.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-8.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-9.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-10.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-11.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
		<li class="item--big"> <a href="#">
			<figure> <img src="images/big-12.jpg" alt="" />
				<figcaption class="img-caption"> 标题标题 </figcaption>
			</figure>
			</a> </li>
	</ul>
	<div class="controls">
		<span class="control icon-arrow-left" data-direction="previous"></span>
		<span class="control icon-arrow-right" data-direction="next"></span>
		<span class="grid icon-grid"></span>
		<span class="fs-toggle icon-fullscreen"></span>
	</div>
</div>
<div style="clear: both"></div>
<script src="js/jquery-1.8.3.min.js"></script>
<script src="js/plugins.js"></script>
<script src="js/scripts.js"></script>
<script>
$(function(){
	$('#gallery-container').sGallery({
		fullScreenEnabled: true
	});
});
</script>
</body>
</html>