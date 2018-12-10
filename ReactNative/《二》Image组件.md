Image是RN的图片常用的控件

### 图片的两种引用方式
source使用uri参数引用网络地址，使用require引用本地图片路径
引用本地图片路径
```javascript
<Image style={styles.img} source={require('../res/bg.jpg')}/>
```
- **./与../与/的区别**
>./代表当前目录，../代表父级目录，../代表根目录

加载网络图片
```javascript
<Image style={styles.img} source={{uri: 'http://img.artcm.cn/5c0b71fdfa5931373988331c.jpg?imageView2/1/w/712/h/400'}}/>
```

### resizeMode裁剪图片的方式
Android有多种裁剪图片的方式，如：center、centerCrop、fixXY、fitStart、fitEnd、fitCenter、matrix、centerInside

resizeMode有cover、contain、stretch、center、repeat

- cover：类似centerCrop的裁剪方式，等比例放大缩小，超出范围居中裁剪。
- contain：类似于fitCenter的裁剪方式，等比例放大缩小，保证图片显示完整，但是可能会有留白。
- stretch：类似于fitXY的裁剪方式，撑满整个控件大小，会导致图片变形。
- center：类似于center的裁剪方式，等比例放大缩小，居中不拉伸。
- repeat：平铺填满容器，能够完整显示，会有留白。
