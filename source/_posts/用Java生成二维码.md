---
title: 用Java生成二维码
date: 2018-01-20 20:50:43
tags: 二维码
---
<h1>java实现二维码开发</h1>

java开发二维码的流程

1、导入核心jar包，在这里使用Google的zxing.jar，还有一个是日本开发qrcode就不用了。
```
	core-3.1.0.jar
```
2、编写二维码核心类
```java
	public static void zxingImg(String content,HttpServletResponse response) throws IOException{

	    int width2 = 200;   //图片高度和宽度
	    int height2 = 200; 

	    //二维码的图片格式 
	    String format = "gif"; 

	    Hashtable hints = new Hashtable(); 

	    //内容所使用编码 
	    hints.put(EncodeHintType.CHARACTER_SET, "UTF-8"); 

	    try {
		    BitMatrix bitMatrix = new MultiFormatWriter().encode(content,BarcodeFormat.QR_CODE, width2, height2, hints);

		    int width = bitMatrix.getWidth(); 
		    int height = bitMatrix.getHeight(); 

		    BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB); 

			    for (int x = 0; x < width; x++) { 
			        for (int y = 0; y < height; y++) { 
			            image.setRGB(x, y, bitMatrix.get(x, y) ? 0xFF000000 : 0xFFFFFFFF); //二维码图片为黑白两色
			    	} 
				}
		    //输出二维码图片流
		    ImageIO.write(image,"gif",response.getOutputStream());

		    //在本地生成二维码图片 
		    //File outputFile = new File("d:"+File.separator+"new.gif"); 
		    //MatrixToImageWriter.writeToFile(bitMatrix, format, outputFile);

			} catch (WriterException e) {
			    e.printStackTrace();
			} 
		}
```
3、使用二维码核心类

1）可以在Servlet中使用
```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

	    //设置编码
	    request.setCharacterEncoding("UTF-8");
	    response.setContentType("text/html;charset=UTF-8");

	    String content = new String(request.getParameter("content").getBytes("ISO-8859-1"),"UTF-8");
	    QrcodeUtil.QrcodeImg(content, response);
	}
```
2）使用SpringMVC(后续添加)

4、在网页中显示
```html
	<img src="QrcodeServlet?content=<%=content %>" />
```