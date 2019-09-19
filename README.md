# CompressPictures-by-canvas-
前端压缩图片后上传到服务器
前端压缩图片后上传到服务器	## 前端压缩图片后上传到服务器。
#### 由于现在手机像素普遍较高，随手拍一张图片都6、7M，十几兆的图片也并不罕见。如果这些未处理的图片直接随数据上传向服务器，不但会占用更多的存储空间，而且用户也要等更久的时间，体验性会差很多，同时更长的传输时间，也加大了问题发生的概率，基于这些情况，压缩图片并上传的需求应运而生
##### 基本原理：
        1.FileRarder将文件转换为URL格式的字符串，这个URL可以直接被Image节点
        2.Image使用URL生成节点，此时可以获取原始图片的width,height等相关信息
        3.使用exif解决图片旋转问题（有些手机拍照上传的时候图片会被旋转，用 exif 识别图片是否被旋转，然后将其旋转回去）
        4.应用canvas重新绘制成比例较小的图片，从而实现图片压缩
        5.将压缩后的canvas上的图片转换成Blob,并且上传到服务器
##### 知识点介绍：
        1.FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，
          使用 File 或 Blob 对象指定要读取的文件或数据。
          总而言之：FileReader是一种异步文件读取机制，结合input:file可以很方便的读取本地文件。
        2.canvas:canvas用来转换图片，将原来的图片“另存为”新图片。这里核心方法是cavas的drawImage方法来压缩图片。
        3.Zepto是一个轻量级的针对现代高级浏览器的JavaScript 库， 它与jquery 有着类似的api。
          Zepto.js在移动端被运用的更加广泛；更注重在移动端的使用,Zepto.js可以说是阉割版本的jQuery。
        4.EXIF(EXchangeable Image File Format)是专门为数码相机的照片设定的可交换图像文件格式，
          通过在图片下载URL后附加exif指示符（区分大小写）获取。
          只能读取使用设备（如相机、手机、摄像头等）拍摄的照片进行测试，这样的照片才有 EXIF 数据。
拍照方向、相机设备型号、拍摄时间、ISO 感光度、GPS 地理位置等数据。

##### 重要代码块分析：
       1.传统上传图片标签
```
         <input type="file" id="imgupload"   multiple="multiple" accept="image/*" name="pics[]">
```
       2.压缩图片需要的一些元素和对象
```
         var reader = new FileReader(), 
         img = new Image();//创建一个img对象
         var file = null;//选择的文件对象
         var canvas = document.createElement('canvas');  // 缩放图片需要的canvas
         var context = canvas.getContext('2d');
 ```
        3.判断拍照设备持有方向调整照片角度

``` 
          var orientation = exif ? exif.Orientation : 1;
            switch (orientation) {
                case 3:
                    angle = 180;
                    break;
                case 6:
                    angle = 90;
                    break;
                case 8:
                    angle = 270;
                    break;
            }
    
```
         4.将图片用canvas重画宽高缩小一倍(压缩重点)
```
           function imgScale(img,callback) {
                    var cvs = document.createElement('canvas');
                    var cvxTx = cvs.getContext('2d');
                    var imgWidth = img.width / 2;
                    var imgHeight;
                    var ww, hh, dataURL;
                    var formatSize = function(w, h) {
                        imgHeight = imgWidth * h / w;
                        cvs.width = imgWidth;
                        cvs.height = imgHeight;
                    };
                    cvxTx.drawImage(img, 0, 0, ww, hh);
                    dataURL = cvs.toDataURL('image/jpeg', 0.7);
                    callback && callback(dataURL);
                }
            })();
```
          5.转成blob
```
             
            function dataURLtoBlob(dataurl) {
                var arr = dataurl.split(',');
              //注意base64的最后面中括号和引号是不转译的
                var _arr = arr[1].substring(0,arr[1].length-2);
                var mime = arr[0].match(/:(.*?);/)[1],
                bstr =atob(_arr),
                n = bstr.length,
                u8arr = new Uint8Array(n);
                while (n--) {
                    u8arr[n] = bstr.charCodeAt(n);
                }
                return new Blob([u8arr], {
                    type: mime
                });
            }
```

