

# CompressPictures-by-canvas
## 前端压缩图片后上传到服务器。
#### 由于现在手机像素普遍较高，随手拍一张图片都6、7M，十几兆的图片也并不罕见。如果这些未处理的图片直接随数据上传向服务器，不但会占用更多的存储空间，而且用户也要等更久的时间，体验性会差很多，同时更长的传输时间，也加大了问题发生的概率，基于这些情况，压缩图片并上传的需求应运而生。
##### html5 中图片用 canvas 压缩(解决图片上传服务端的时候太大的问题，前端实现图片压缩)，用 exif 解决图片旋转问题（有些手机拍照上传的时候图片会被旋转，用 exif 识别图片是否被旋转，然后将其旋转回去）
##### 基本原理：
        1.FileRarder将文件转换为URL格式的字符串，这个URL可以直接被Image节点
        2.Image使用URL生成节点，此时可以获取原始图片的width,height等相关信息
        3.应用canvas重新绘制成比例较小的图片，从而实现图片压缩
        4.将压缩后的canvas上的图片转换成Blob,并且上传到服务器
##### 知识点介绍：
        1.FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，
          使用 File 或 Blob 对象指定要读取的文件或数据。
          总而言之：FileReader是一种异步文件读取机制，结合input:file可以很方便的读取本地文件。
        2.canvas:canvas用来转换图片，将原来的图片“另存为”新图片。这里核心方法是cavas的drawImage方法来压缩图片。

+ a
+ b
+ c 

1.
2.
3.


+ 123
    +abc
    +bcd
    +cde
+ 456
+ 789
`代码块`

```

```