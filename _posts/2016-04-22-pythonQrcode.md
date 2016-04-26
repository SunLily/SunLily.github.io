---
layout: post
title: "使用Qrcode生成二维码"
header-img: "img/flower2.jpg"
---
 `Qrcode` 是python的一个库,用于生成QR码。使用Qrcode之前首先得安装python,然后配置好环境变量之后使用`pip install Qrcode`安装Qrcode,
 由于生成Qrcode图片需要依赖Python图片库,所以还得使用`pip install image`安装图片库,不然会报"ImportError: No module named Image" 错误.
 下载好之后就可以在终端使用qr命令生成二维码了
 {% highlight ruby %}
 qr "Just a test" >test.jpg
 qr --help
 {% endhighlight %}
<h2>生成简单的QR码</h2>
{%highlight ruby%}
import Image
import qrcode
qr = qrcode.QRcode(
    version = 2, #二维码尺寸大小
    error_correction = qrcode.constants.ERROR_CORRECT_L,#控制用于QR码纠错
    box_size = 10,#控制多少个像素
    border = 1 #控制边框的个子厚度
)
qr.add_data("http://sunlily.github.io/")
qr.make(fit=True)
img = qr.make_image()
img.save("test.jpg")
{% endhighlight %}
<h2>生成带logoQR码</h2>

{% highlight ruby %}
import Image
import os
import qrcode
def make_logo_qr(str, logo, save):
    #参数配置
    qr = qrcode.QRCode(
        version=4,
        box_size=8,
        border=2
    )
     #添加转换内容
        qr.add_data(str)
        qr.make(fit=True)
        #生成二维码
        img = qr.make_image()
        img = img.convert("RGBA")
     #添加logo
         if logo and os.path.exists(logo):
             icon = Image.open(logo)
             #获取二维码图片的大小
             img_w,img_h = img.size
             factor = 4
             size_w = int(img_w/factor)
             size_h = int(img_h/factor)
             #logo图片的大小不能超过二维码图片的1/4
             icon_w,icon_h = icon.size
             if icon_w > size_w:
                 icon_w=size_w
             if icon_h>size_h:
                 icon_h = size_h
             icon = icon.resize((icon_w,icon_h),Image.ANTIALIAS)
             #计算logo在二维码图中的位置
             w = int((img_w-icon_w)/2)
             h = int((img_h-icon_h)/2)
             icon=icon.convert("RGBA")
             img.paste(icon,(w,h),icon)
             #保存处理后的图片
             img.save(save)
{% endhighlight %}
{%highlight ruby%}
if __name__=="__main__":
    save_path = 'd://test.png'
    logo = 'img/logoQR.jpg' #logo图片
    str = "http://sunlily.github.io/"
    make_logo_qr(str,logo,save_path)
{% endhighlight %}