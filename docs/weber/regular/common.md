# 开发中常用正则

## 1.校验密码强度

密码的强度必须是包含大小写字母和数字的组合，不能使用特殊字符，长度在 8-10 之间

```
^(?=.*\\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$
```

## 2.校验中文

字符串仅能使中文

```
^[\\u4e00-\\u9fa5]{0,}$
```

## 3.由数字、26 个英文字母或下划线组成的字符串

```
^\\w+$
```

## 4.校验 E-mail 地址

```
[\\w!#$%&'*+/=?^_`{|}~-]+(?:\\.[\\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\\w](?:[\\w-]*[\\w])?\\.)+[\\w](?:[\\w-]*[\\w])?
```

## 5.校验身份证号码

15 位：

```
^[1-9]\\d{7}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}$
```

18 位:

```
^[1-9]\\d{5}[1-9]\\d{3}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}([0-9]|X)$
```

## 6.校验日期

'yyyy-mm-dd'格式的日期校验，考虑平闰年

```
^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$
```

## 7.校验金额

金额校验，精确到两位小数

```
^[0-9]+(.[0-9]{2})?$
```

## 8.校验手机号

国内 13,15,18 开头的手机号码正则表达式，可以根据目前国内手机号扩展前两位

```
^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\\d{8}$
```

## 9.判断 ie 的版本

ie 目前还没有被完全取代，很多页面还是要做版本兼容

```
^.*MSIE [5-8](?:\\.[0-9]+)?(?!.*Trident\\/[5-9]\\.0).*$
```

## 10.校验 IP-V4 地址

```
\\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\b
```

## 11.校验 IP-V6 地址

```
(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))
```

## 12.检查 URL 的前缀

应用开发中很多时候需要区分请求是 HTTPS 还是 HTTP

```
if (!s.match(/^[a-zA-Z]+:\\/\\//))
{
    s = 'http://' + s;
}
```

## 13.提取 URL 链接

对一段文本中的 URL 进行筛选

```
^(f|ht){1}(tp|tps):\\/\\/([\\w-]+\\.)+[\\w-]+(\\/[\\w- ./?%&=]*)?
```

## 14.文件路径及扩展校验

验证 windows 下文件路径和扩展名(下面例子中为.txt 文件)

```
^([a-zA-Z]\\:|\\\\)\\\\([^\\\\]+\\\\)*[^\\/:*?"<>|]+\\.txt(l)?$
```

## 16.提取网页图片

```
\\< *[img][^\\\\>]*[src] *= *[\\"\\']{0,1}([^\\"\\'\\ >]*)
```

## 17.提取页面超链接

```
(<a\\s*(?!.*\\brel=)[^>]*)(href="https?:\\/\\/)((?!(?:(?:www\\.)?'.implode('|(?:www\\.)?', $follow_list).'))[^"]+)"((?!.*\\brel=)[^>]*)(?:[^>]*)>
```

## 18.查找 css 属性

```
^\\s*[a-zA-Z\\-]+\\s*[:]{1}\\s[a-zA-Z0-9\\s.#]+[;]{1}
```

## 19.抽取注释

```
<!--(.*?)-->
```

### 20.匹配 HTML 标签

```
<\\/?\\w+((\\s+\\w+(\\s*=\\s*(?:".*?"|'.*?'|[\\^'">\\s]+))?)+\\s*|\\s*)\\/?>
```

## 中国所有合法手机号码的正则表达式

```
/^1(?:3\d|4[4-9]|5[0-35-9]|6[67]|7[013-8]|8\d|9\d)\d{8}$/
```

## 规范用户输入到小数点后两位

```
 _handdata(val) {
  val = String(val).replace(/^\./g, '') // 验证第一个字符是数字
  val = String(val).replace(/[^\d.]/g, '') // 清除"数字"和"."以外的字符
  val = String(val).replace(/\.{2,}/g, '.') // 只保留第一个, 清除多余的
  val = String(val).replace('.', '$#$').replace(/\./g, '').replace('$#$', '.')
  val = String(val).replace(/^(-)*(\d+)\.(\d\d).*$/, '$1$2.$3') // 只能输入两个小数
  if (val.indexOf('.') < 0 && val !== '') {
    val = parseFloat(val)
  }
  return val
}
```
