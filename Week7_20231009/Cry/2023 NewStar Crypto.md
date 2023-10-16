## 2023 NewStar Crypto

[TOC]

### 知识点补充：

#### UUencode/XXencode

```
UUencode/XXencode

UUencode的加密方式和base64很相似。但他的编码表有很多是特殊字符:”!”#￥%&‘（）*+=’” 等等。
XXencode的加密方式也和base64相似。跟base64打印字符相比，就是UUencode多一个“-” 字符，少一个”/” 字符。
举个例子
UUencode（1234567）= (,3(S-#4V-PH`
特征：看着特别奇怪
XXencode（1234567）= 6AH6nB1IqBkc+
特征：与base64相似
```

#### 维吉尼亚密码

```markdown
**维吉尼亚密码只对字母加密，不对符号加密**，解密完成后要把符号加到对应位置
```

#### 两个解码函数：

```
print(libnum.n2s(m).decode())	//需要导入libnum库
print(long_to_bytes(m))		//需要导入Crypto.Util.number
```

#### AES加密

```
AES（高级加密标准，Advanced Encryption Standard）是一种对称加密算法，被广泛用于保护敏感数据的机密性。它是目前最常用的加密算法之一。

AES 使用相同的密钥进行加密和解密操作，因此被归类为对称加密算法。它支持三种不同的密钥长度：128 位、192 位和 256 位。加密和解密的过程是通过一系列的轮函数（round function）来完成的。

AES 的加密过程如下：
1. 密钥扩展（Key Expansion）：根据初始密钥，生成一系列轮密钥（round keys）。
2. 初始轮（Initial Round）：将明文与第一轮密钥进行异或运算。
3. 轮函数（Round Function）：执行多轮的轮函数操作，每轮包括字节替换（SubBytes）、行移位（ShiftRows）、列混淆（MixColumns）和轮密钥加（AddRoundKey）。
4. 最后一轮（Final Round）：在最后一轮中，省略列混淆操作。
5. 密文生成：经过多轮轮函数操作后，得到最终的加密结果，即密文。

AES 的解密过程与加密过程相似，但在轮函数的执行顺序上有所不同。解密过程如下：
1. 密钥扩展：根据初始密钥，生成一系列轮密钥。
2. 初始轮：将密文与最后一轮的轮密钥进行异或运算。
3. 轮函数逆运算：执行多轮的轮函数逆运算，逆序进行字节替换逆运算（InvSubBytes）、行移位逆运算（InvShiftRows）、列混淆逆运算（InvMixColumns）和轮密钥加逆运算（AddRoundKey）。
4. 最后一轮：在最后一轮中，省略列混淆逆运算。
5. 明文生成：经过多轮轮函数逆运算后，得到最终的解密结果，即明文。

AES 的安全性和性能都得到广泛认可，它在各种应用中被用于加密文件、保护通信和数据传输等领域。然而，正确使用和管理密钥对于保证 AES 的安全性至关重要。
```

#### 仿射密码

```markdown
仿射密码 (Affine cipher) 是一种基于线性变换的简单替换密码。它使用了以下的加密算法：

加密算法：
选择两个整数 a 和 b，其中 a 是一个密钥，且 a 必须与 26 互素（即 a 和 26 的最大公约数必须为 1）。
对于明文中的每个字母 x（假设为小写字母），应用以下变换：
将字母 x 转换为对应的数值，即 a=0, b=1, c=2, ..., z=25。
对数值应用仿射变换：y = (a * x + b) mod 26，其中 mod 表示取模运算。
将变换后的数值转换回字母形式，即 y=0 对应 a, y=1 对应 b, ..., y=25 对应 z。

解密算法：
计算 a 的乘法逆元 a'，即 a * a' ≡ 1 (mod 26)。
对于密文中的每个字母 y（假设为小写字母），应用以下变换：
将字母 y 转换为对应的数值。
对数值应用仿射变换的逆变换：x = (a' * (y - b)) mod 26，其中 mod 表示取模运算。
将变换后的数值转换回字母形式。
```



### WP

#### brainfuck

flag{Oiiaioooooiai#b7c0b1866fe58e12}

++++++++[>>++>++++>++++++>++++++++>++++++++++>++++++++++++>++++++++++++++>++++++++++++++++>++++++++++++++++++>++++++++++++++++++++>++++++++++++++++++++++>++++++++++++++++++++++++>++++++++++++++++++++++++++>++++++++++++++++++++++++++++>++++++++++++++++++++++++++++++<<<<<<<<<<<<<<<<-]>>>>>>>++++++.>----.<-----.>-----.>-----.<<<-.>>++..<.>.++++++.....------.<.>.<<<<<+++.>>>>+.<<<+++++++.>>>+.<<<-------.>>>-.<<<+.+++++++.--..>>>>---.-.<<<<-.+++.>>>>.<<<<-------.+.>>>>>++.

Captfencoder brainfuck解码即可



#### Caesar's Secret

flag{ca3s4r's_c1pher_i5_v4ry_3azy}

kqfl{hf3x4w'x_h1umjw_n5_a4wd_3fed}

凯撒密码移位5位



#### Fence

flag{reordering_the_plaintext#686f8c03}

fa{ereigtepanet6680}lgrodrn_h_litx#8fc3

栅栏密码分组为2



#### Vigenère

flag{la_c1fr4_del_5ign0r_giovan_batt1st4_b3ll5s0}

pqcq{qc_m1kt4_njn_5slp0b_lkyacx_gcdy1ud4_g3nv5x0}

根据前四个字母是kfck猜测密钥是kfc，解密即可

猜测过程：

f--p 移位为k  l--q 移位为f  a--c 移位为c  g--q 移位为k

需要注意的是 **维吉尼亚密码只对字母加密，不对符号加密**，解密完成后要把符号加到对应位置



#### babyencode

part 1 of flag: 

```
ZmxhZ3tkYXp6bGluZ19lbmNvZGluZyM0ZTBhZDQ=
```

part 2 of flag:

```
 MYYGGYJQHBSDCZJRMQYGMMJQMMYGGN3BMZSTIMRSMZSWCNY=
```

part 3 of flag: 

```
=8S4U,3DR8SDY,C`S-F5F-C(S,S<R-C`Q9F8S87T`
```



part1: base64	flag{dazzling_encoding#4e0ad4

part2: base32	f0ca08d1e1d0f10c0c7afe422fea7

part3: UUencode	c55192c992036ef623372601ff3a}



知识点补充：

```
UUencode/XXencode

UUencode的加密方式和base64很相似。但他的编码表有很多是特殊字符:”!”#￥%&‘（）*+=’” 等等。
XXencode的加密方式也和base64相似。跟base64打印字符相比，就是UUencode多一个“-” 字符，少一个”/” 字符。
举个例子
UUencode（1234567）= (,3(S-#4V-PH`
特征：看着特别奇怪
XXencode（1234567）= 6AH6nB1IqBkc+
特征：与base64相似
```



#### babyRSA

```python
from Crypto.Util.number import *
from flag import flag

def gen_prime(n):
    res = 1

​    for i in range(15):
​        res *= getPrime(n)

​    return res


if __name__ == '__main__':
    n = gen_prime(32)
    e = 65537
    m = bytes_to_long(flag)
    c = pow(m,e,n)
    print(n)
    print(c)

# 17290066070594979571009663381214201320459569851358502368651245514213538229969915658064992558167323586895088933922835353804055772638980251328261

# 14322038433761655404678393568158537849783589481463521075694802654611048898878605144663750410655734675423328256213114422929994037240752995363595
```

分解n

n=

[2217990919](http://www.factordb.com/index.php?id=2217990919)<10> · [2338725373](http://www.factordb.com/index.php?id=2338725373)<10> · [2370292207](http://www.factordb.com/index.php?id=2370292207)<10> · [2463878387](http://www.factordb.com/index.php?id=2463878387)<10> · [2706073949](http://www.factordb.com/index.php?id=2706073949)<10> · [2794985117](http://www.factordb.com/index.php?id=2794985117)<10> · [2804303069](http://www.factordb.com/index.php?id=2804303069)<10> · [2923072267](http://www.factordb.com/index.php?id=2923072267)<10> · [2970591037](http://www.factordb.com/index.php?id=2970591037)<10> · [3207148519](http://www.factordb.com/index.php?id=3207148519)<10> · [3654864131](http://www.factordb.com/index.php?id=3654864131)<10> · [3831680819](http://www.factordb.com/index.php?id=3831680819)<10> · [3939901243](http://www.factordb.com/index.php?id=3939901243)<10> · [4093178561](http://www.factordb.com/index.php?id=4093178561)<10> · [4278428893](http://www.factordb.com/index.php?id=4278428893)<10>

```python
from Crypto.Util.number import *
from gmpy2 import *
primes = [2217990919, 2338725373, 2370292207, 2463878387, 2706073949, 2794985117, 2804303069, 2923072267, 2970591037, 3207148519, 3654864131, 3831680819, 3939901243, 4093178561, 4278428893]
n = 1
for prime in primes:
    n *= prime
e = 65537
c = 14322038433761655404678393568158537849783589481463521075694802654611048898878605144663750410655734675423328256213114422929994037240752995363595
phi_n = 1
for prime in primes:
    phi_n *= (prime - 1)
d = inverse(e, phi_n)
m = pow(c, d, n)
 
print(long_to_bytes(m))
#b'flag{us4_s1ge_t0_cal_phI}'
```

已知n,c,e

分解n为15个质数

phi(n)=15个质数减1后相乘

d=inverse(e,phi(n))

m=pow(c,d,n)



#### Small d

```python
from secret import flag
from Crypto.Util.number import *

p = getPrime(1024)
q = getPrime(1024)

d = getPrime(32)
e = inverse(d, (p-1)*(q-1))
n = p*q
m = bytes_to_long(flag)

c = pow(m,e,n)

print(c)
print(e)
print(n)

# c = 6755916696778185952300108824880341673727005249517850628424982499865744864158808968764135637141068930913626093598728925195859592078242679206690525678584698906782028671968557701271591419982370839581872779561897896707128815668722609285484978303216863236997021197576337940204757331749701872808443246927772977500576853559531421931943600185923610329322219591977644573509755483679059951426686170296018798771243136530651597181988040668586240449099412301454312937065604961224359235038190145852108473520413909014198600434679037524165523422401364208450631557380207996597981309168360160658308982745545442756884931141501387954248
# e = 8614531087131806536072176126608505396485998912193090420094510792595101158240453985055053653848556325011409922394711124558383619830290017950912353027270400567568622816245822324422993074690183971093882640779808546479195604743230137113293752897968332220989640710311998150108315298333817030634179487075421403617790823560886688860928133117536724977888683732478708628314857313700596522339509581915323452695136877802816003353853220986492007970183551041303875958750496892867954477510966708935358534322867404860267180294538231734184176727805289746004999969923736528783436876728104351783351879340959568183101515294393048651825
# n = 19873634983456087520110552277450497529248494581902299327237268030756398057752510103012336452522030173329321726779935832106030157682672262548076895370443461558851584951681093787821035488952691034250115440441807557595256984719995983158595843451037546929918777883675020571945533922321514120075488490479009468943286990002735169371404973284096869826357659027627815888558391520276866122370551115223282637855894202170474955274129276356625364663165723431215981184996513023372433862053624792195361271141451880123090158644095287045862204954829998614717677163841391272754122687961264723993880239407106030370047794145123292991433

```

**考点：**低解密指数 维纳攻击

```python
import gmpy2
import libnum
 
def continuedFra(x, y):
    """计算连分数
    :param x: 分子
    :param y: 分母
    :return: 连分数列表
    """
    cf = []
    while y:
        cf.append(x // y)
        x, y = y, x % y
    return cf
def gradualFra(cf):
    """计算传入列表最后的渐进分数
    :param cf: 连分数列表
    :return: 该列表最后的渐近分数
    """
    numerator = 0
    denominator = 1
    for x in cf[::-1]:
        # 这里的渐进分数分子分母要分开
        numerator, denominator = denominator, x * denominator + numerator
    return numerator, denominator
def solve_pq(a, b, c):
    """使用韦达定理解出pq，x^2−(p+q)∗x+pq=0
    :param a:x^2的系数
    :param b:x的系数
    :param c:pq
    :return:p，q
    """
    par = gmpy2.isqrt(b * b - 4 * a * c)
    return (-b + par) // (2 * a), (-b - par) // (2 * a)
def getGradualFra(cf):
    """计算列表所有的渐近分数
    :param cf: 连分数列表
    :return: 该列表所有的渐近分数
    """
    gf = []
    for i in range(1, len(cf) + 1):
        gf.append(gradualFra(cf[:i]))
    return gf
 
 
def wienerAttack(e, n):
    """
    :param e:
    :param n:
    :return: 私钥d
    """
    cf = continuedFra(e, n)
    gf = getGradualFra(cf)
    for d, k in gf:
        if k == 0: continue
        if (e * d - 1) % k != 0:
            continue
        phi = (e * d - 1) // k
        p, q = solve_pq(1, n - phi + 1, n)
        if p * q == n:
            return d
 
c = 6755916696778185952300108824880341673727005249517850628424982499865744864158808968764135637141068930913626093598728925195859592078242679206690525678584698906782028671968557701271591419982370839581872779561897896707128815668722609285484978303216863236997021197576337940204757331749701872808443246927772977500576853559531421931943600185923610329322219591977644573509755483679059951426686170296018798771243136530651597181988040668586240449099412301454312937065604961224359235038190145852108473520413909014198600434679037524165523422401364208450631557380207996597981309168360160658308982745545442756884931141501387954248
e = 8614531087131806536072176126608505396485998912193090420094510792595101158240453985055053653848556325011409922394711124558383619830290017950912353027270400567568622816245822324422993074690183971093882640779808546479195604743230137113293752897968332220989640710311998150108315298333817030634179487075421403617790823560886688860928133117536724977888683732478708628314857313700596522339509581915323452695136877802816003353853220986492007970183551041303875958750496892867954477510966708935358534322867404860267180294538231734184176727805289746004999969923736528783436876728104351783351879340959568183101515294393048651825
n = 19873634983456087520110552277450497529248494581902299327237268030756398057752510103012336452522030173329321726779935832106030157682672262548076895370443461558851584951681093787821035488952691034250115440441807557595256984719995983158595843451037546929918777883675020571945533922321514120075488490479009468943286990002735169371404973284096869826357659027627815888558391520276866122370551115223282637855894202170474955274129276356625364663165723431215981184996513023372433862053624792195361271141451880123090158644095287045862204954829998614717677163841391272754122687961264723993880239407106030370047794145123292991433
 
d=wienerAttack(e, n)
m=pow(c, d, n)
print(libnum.n2s(m).decode())
#flag{learn_some_continued_fraction_technique#dc16885c}
```

```python
两个解码函数：
print(libnum.n2s(m).decode())	//需要导入libnum库
print(long_to_bytes(m))		//需要导入Crypto.Util.number
```



#### babyxor

```python
from secret import *

ciphertext = []

for f in flag:
    ciphertext.append(f ^ key)

print(bytes(ciphertext).hex())
# e9e3eee8f4f7bffdd0bebad0fcf6e2e2bcfbfdf6d0eee1ebd0eabbf5f6aeaeaeaeaeaef2
```

根据给出的密文，我们可以看到每两个字符代表一个字节。

每一个明文异或key，得到密文。最主要是要知道key值，就能逆推明文，已知明文头是“flag”,用0xe9^ord("f"),0xe3^ord("l")得到的两个key都是key=143。

```python
import binascii
 
cipher = "e9e3eee8f4f7bffdd0bebad0fcf6e2e2bcfbfdf6d0eee1ebd0eabbf5f6aeaeaeaeaeaef2"
c = binascii.unhexlify(cipher)
 
key=143
 
result = ""
for i in c:
    result += chr(i ^ key)
print(result)
#flag{x0r_15_symm3try_and_e4zy!!!!!!}
```



#### Affine （Caesar with multiplication）

```python
from flag import flag, key

modulus = 256

ciphertext = []

for f in flag:
    ciphertext.append((key[0]*f + key[1]) % modulus)

print(bytes(ciphertext).hex())

# dd4388ee428bdddd5865cc66aa5887ffcca966109c66edcca920667a88312064

```

仿射密码

```markdown
仿射密码 (Affine cipher) 是一种基于线性变换的简单替换密码。它使用了以下的加密算法：

加密算法：
选择两个整数 a 和 b，其中 a 是一个密钥，且 a 必须与 26 互素（即 a 和 26 的最大公约数必须为 1）。
对于明文中的每个字母 x（假设为小写字母），应用以下变换：
将字母 x 转换为对应的数值，即 a=0, b=1, c=2, ..., z=25。
对数值应用仿射变换：y = (a * x + b) mod 26，其中 mod 表示取模运算。
将变换后的数值转换回字母形式，即 y=0 对应 a, y=1 对应 b, ..., y=25 对应 z。

解密算法：
计算 a 的乘法逆元 a'，即 a * a' ≡ 1 (mod 26)。
对于密文中的每个字母 y（假设为小写字母），应用以下变换：
将字母 y 转换为对应的数值。
对数值应用仿射变换的逆变换：x = (a' * (y - b)) mod 26，其中 mod 表示取模运算。
将变换后的数值转换回字母形式。
```

对于仿射加密过程我们可以把他看成一个一元线性同余方程

c = (a*m + b) % n

我们已知密文，和明文的前四个“flag”

c是密文，m是明文，a是所求的值

根据加密方程 `c = (a*m + b) % 256`

c1 = (a*m1 + b) % 256(1)

c2 = (a*m2+ b) %  256(2)

(2)-(1),得

c2 -c1= a*(m2-m1) %  256

a=c2-c1*(m2-m1)^-1 %256

得到a

算出b=c-(a*m)%n

```python
import gmpy2
c1=ord('\xdd')
c2=ord('C')
c3=ord('\x88')
c4=ord('\xee')
m1=ord('f')
m2=ord('l')
m3=ord('a')
m4=ord('g')
a=(c3-c1)*gmpy2.invert(m3-m1,256)%256
print(a)#17
b=(c1-a*m1)%256
print(b)#23
c="\xddC\x88\xeeB\x8b\xdd\xddXe\xccf\xaaX\x87\xff\xcc\xa9f\x10\x9cf\xed\xcc\xa9 fz\x881 d"
flag=''
for cc in c:
    m=(ord(cc)-b)*gmpy2.invert(a,256)%256
    flag+=chr(m)
print(flag)
#flag{4ff1ne_c1pher_i5_very_3azy}
```



#### babyaes

```python
from Crypto.Cipher import AES
import os
from flag import flag
from Crypto.Util.number import *


def pad(data):
    return data + b"".join([b'\x00' for _ in range(0, 16 - len(data))])


def main():
    flag_ = pad(flag)
    key = os.urandom(16) * 2
    iv = os.urandom(16)
    print(bytes_to_long(key) ^ bytes_to_long(iv) ^ 1)
    aes = AES.new(key, AES.MODE_CBC, iv)
    enc_flag = aes.encrypt(flag_)
    print(enc_flag)


if __name__ == "__main__":
    main()
# 3657491768215750635844958060963805125333761387746954618540958489914964573229
# b'>]\xc1\xe5\x82/\x02\x7ft\xf1B\x8d\n\xc1\x95i'
```

xor=print(bytes_to_long(key) ^ bytes_to_long(iv) ^ 1)
key=16*8*2=256bit,iv=16*8=128bit,256^128bit说明key的高位128bit不变，所以通过xor高128bit得到高位key的值*2还原key，iv=key^xor^1

```python
from Crypto.Util.number import *
from Crypto.Cipher import AES
from gmpy2 import*
xor=3657491768215750635844958060963805125333761387746954618540958489914964573229
c=b'>]\xc1\xe5\x82/\x02\x7ft\xf1B\x8d\n\xc1\x95i'
xor=long_to_bytes(xor)
key=xor[0:16]*2
print(key)
#b'\x08\x16\x11%\xa0\xa6\xc5\xcb^\x02\x99NF`\xea,\x08\x16\x11%\xa0\xa6\xc5\xcb^\x02\x99NF`\xea,'
iv = long_to_bytes(bytes_to_long(key) ^bytes_to_long(xor)  ^ 1)
print(iv)
#b'\xe3Z\x19Ga>\x07\xcc\xd1\xa1X\x01c\x11\x16\x00'
aes = AES.new(key, AES.MODE_CBC, iv)
flag = aes.decrypt(c)
print(flag)
#b'firsT_cry_Aes\x00\x00\x00'
```

AES加密过程：

```markdown
AES（高级加密标准，Advanced Encryption Standard）是一种对称加密算法，被广泛用于保护敏感数据的机密性。它是目前最常用的加密算法之一。

AES 使用相同的密钥进行加密和解密操作，因此被归类为对称加密算法。它支持三种不同的密钥长度：128 位、192 位和 256 位。加密和解密的过程是通过一系列的轮函数（round function）来完成的。

AES 的加密过程如下：
1. 密钥扩展（Key Expansion）：根据初始密钥，生成一系列轮密钥（round keys）。
2. 初始轮（Initial Round）：将明文与第一轮密钥进行异或运算。
3. 轮函数（Round Function）：执行多轮的轮函数操作，每轮包括字节替换（SubBytes）、行移位（ShiftRows）、列混淆（MixColumns）和轮密钥加（AddRoundKey）。
4. 最后一轮（Final Round）：在最后一轮中，省略列混淆操作。
5. 密文生成：经过多轮轮函数操作后，得到最终的加密结果，即密文。

AES 的解密过程与加密过程相似，但在轮函数的执行顺序上有所不同。解密过程如下：
1. 密钥扩展：根据初始密钥，生成一系列轮密钥。
2. 初始轮：将密文与最后一轮的轮密钥进行异或运算。
3. 轮函数逆运算：执行多轮的轮函数逆运算，逆序进行字节替换逆运算（InvSubBytes）、行移位逆运算（InvShiftRows）、列混淆逆运算（InvMixColumns）和轮密钥加逆运算（AddRoundKey）。
4. 最后一轮：在最后一轮中，省略列混淆逆运算。
5. 明文生成：经过多轮轮函数逆运算后，得到最终的解密结果，即明文。

AES 的安全性和性能都得到广泛认可，它在各种应用中被用于加密文件、保护通信和数据传输等领域。然而，正确使用和管理密钥对于保证 AES 的安全性至关重要。
```

