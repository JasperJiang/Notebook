# Model

python manage.py inspectdb 

这样就可以在命令行看到数据库的模型文件了 

python manage.py inspectdb &gt; app/models.py

### model field 类型

1、AutoField    

 一个自增的IntegerField，一般不直接使用，Django会自动给每张表添加一个自增的primary key。

2、BigIntegerField    

64位整数， -9223372036854775808 到 9223372036854775807。默认的显示widget 是 TextInput.

3、BinaryField （ Django 1.6 版本新增 ）    

存储二进制数据。不能使用 filter 函数获得 QuerySet

4、BooleanField   

True/False，默认的widget 是 CheckboxInput。    如果需要置空，则必须用 NullBooleanField 代替。    Django 1.6 修改：BooleanField 的默认值 由 False 改为 None，在 default 属性未设置的情况下。

5、CharField    

存储字符串。必须有 max\_length 参数指定长度。默认的form widget 是 TextInput    如果字符串巨长，推荐使用 TextField。

6、CommaSeparatedIntegerField    

一串由逗号分开的整数。必须有 max\_length 参数。

7、DateField    

日期，与python里的datetime.date 实例同。有以下几个可选的选项，均为bool类型：         

DateField.auto\_now: 每次执行 save 操作的时候自动记录当前时间，常作为最近一次修改的时间 使用。注意：总是在执行save 操作的时候执行，无法覆盖。        

 DateField.auto\_now\_add: 第一次创建的时候添加当前时间。常作为 创建时间 使用。注意：每次create 都会调用。    

默认的form widget 是 TextInput。    

注意：设置auto\_now 或者 auto\_now\_add 为 True 会导致当前自动拥有 editable=False 和 blank = True 设置。

8、DateTimeField    

日期+时间。与python里的 datetime.datetime 实例同。常用附加选项和DateField一样。    

默认 form widget 是一个 TextInput

9、DecimalField    

设置了精度的十进制数字。   

 A fixed-precision decimal number, represented in Python by a Decimal instance. Has two required arguments:    

DecimalField.max\_digits    

The maximum number of digits allowed in the number. Note that this number must be greater than or equal to decimal\_places.    DecimalField.decimal\_places?    

The number of decimal places to store with the number.    

For example, to store numbers up to 999 with a resolution of 2 decimal places, you’d use:   

models.DecimalField\(..., max\_digits=5, decimal\_places=2\)    

And to store numbers up to approximately one billion with a resolution of 10 decimal places:    

models.DecimalField\(..., max\_digits=19, decimal\_places=10\)    

The default form widget for this field is a TextInput.

10、EmailField 

在 CharField 基础上附加了 邮件地址合法性验证。不需要强制设定 max\_length 

注意：当前默认设置 max\_length 是 75，虽然已经不符合标准，但未了向前兼容，未修改。

11、FileField    

文件上传。不支持 primary\_key 和 unique 选项。否则会报 TypeError 异常。

必须设置 FileField.upload\_to 选项，这个是 本地文件系统路径，附加在 MEDIA\_ROOT 设置的后边，也就是 MEDIA\_ROOT 下的子目录相对路径。    

默认的form widget 是 FileInput。    使用 FileField 和 ImageField 需要以下步骤：        （1）修改 settting.py，设置 MEDIA\_ROOT（使用绝对路径），指定用户上传的文件保存在哪里。设置 MEDIA\_URL，作为 web地址 前缀，要保证 MEDIA\_ROOT 目录对运行 Django 的用户是可写的；        （2）在 model 中增加 FileField 或 ImageField，并指定 upload\_to 选项指定存在 MEDIA\_ROOT 的哪个子目录里；        （3）存在数据库里的是什么东西呢？是 File 或 Image相对于 MEDIA\_ROOT 的相对路径，你可以在 Django 里方便的使用这个地址，比如你的 ImageField 叫 tupian，你可以在 template 中用{{object.tupian.url}}。    举个例子：假设你的 MEDIA\_ROOT='/home/media'，upload\_to 设置为 'photos/%Y/%m/%d'，'%Y/%m/%d' 部分使用strftime\(\) 提供。如果你在 2013年10月10日上传了一个文件，那么它就存在 /home/media/photos/2013/10/10/ 下。    文件在 model实例 执行 save操作的同时保存，所以文件在model实例执行save之前，硬盘的上的文件名的是不可靠的。    注意：要验证用户上传的文件确实是自己需要的，以防止安全漏洞出现。    默认情况下，FileField 在数据库中表现为 varchar\(100\) 的一个列。你可以使用 max\_length 来改变这个大小。

