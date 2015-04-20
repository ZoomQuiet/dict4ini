# `Dict4Ini` #
''This module is used to process ini format configuration file. It acts just like a dict, but you can also access it's sections and options with attribute syntax, just like x.test.''
## Why reinvent this module? ##

I used `Config4Obj` module for `GoogleTalkBot` software (confbot) to deal with configuration file. But I found its lacks on:

  * Only can access options as x['name']['o'], but not as x.name.o
  * You must create section first, then you can access its options. So if you didn't create x['name']={} section, so you cann't do x['name']['o'] = 3
  * The option's data can be saved as string format, but as read out again, `Config4Obj` cann't convert it to their original value type, so you must conver it yourself. I didn't tried validate module ships with `Config4Obj`.
  * Didn't support unicode

'''Above is only my opinions, so they may be not right. '''

So I decide to ''reinvent'' a new module to solve these lacks, I named it as `Dict4Ini`, it means you can access the config object just like a dict.

## What's it features ##

  * as simple as others
  * you can access options according to dict syntax, just like x['name']['o'] = 1, x['name'].keys(), x['name'].values(), etc.
  * you also can access options according to attr syntax, just like x.name.o = 1, x.name.keys(), x.name.values(), etc. So the name must be Identifier or single word.
  * you can save comments in it(but this feature is not tested so much)
  * support multi level section, subsection name will just like: [firsub/secsub]
  * can save config items order
  * support multi data types: string, unicode, int, float, list/tuple, dict, etc, you can save them to or regain them from config file
  * can convert to dict '''new'''
  * auto detect BOM of utf-8 file '''new'''
  * can encrypt or encode in base64 specified sections
  * It's a little module, just for my mind, so if you like, you could try it, but if you don't like, just skip it, that's ok

## About license ##

  * BSD

## What's new? ##
  * 2011/01/18 Version 0.9.7
    * Fix #8 bug.
  * 2009/11/02 Version 0.9.6
    * Add update method.
  * 2008/03/14 Version 0.9.5
    * Fix init.py bug, and you can import dict4ini from dict4ini package
  * 2008/02/16 Version 0.9.4.1
    * Fix comment process, if you comment with `node._comment`, then if the comment does not begin with '#'(commentdelimeter), it'll automatically add it.
  * 2007/09/22 Version 0.9.3
    * Improve the comment process
    * Improve empty section process
    * Add normal format support, so each value should be treated string type
  * 2007/09/19 Version 0.9.2.5
    * Save boolean value to 0 or 1
    * Add iterator support, and you can use `for i in ini` to iterator (key,value) now
    * Add 'in' test support, so you can test if a key is in a ini via `key in ini`, it's the same with `ini.has_key(key)`
  * 2007/09/17 Version 0.9.2.4
    * Fix the end string is \" bug
  * 2007/08/08 Version 0.9.2.3
    * Fix sub section bug, for secretSection argument, dict4ini will encrypt all subsections of a secretSection, for example, secretSection=['a', 'c'], then all subsections of 'a' and 'c' will be encrypted.
    * Add missing init.py file.
  * 2007/07/09 Version 0.9.2.1 thanks to Mayowa Akinyemi
    * Add the ability to protect specific sections. When use with the secretKey or hideData only section names listed in the secretSections parameter will be encrypted
  * 2007/07/03 Version 0.9.2
    * Add clear method
    * Added encryption code thanks to Mayowa Akinyemi. Using the secretKey parameter will encrypt the values using Paul Rubin's p3.py encryption module. Using the hideData parameter will perform base64 enc/dec of the values
  * 2007/06/26 Version 0.9.1
    * Fix float convert bug
  * 2007/06/13 Version 0.9
    * Thanks for Victor Stinner giving a output format patch, so you can define your own output format "%s = %s" to anything you want, for example "%s=%s" or "%s:%s". And Dict4Ini will auto remove '%s' from the fromat string, and the strip() the rest of the string, then use it to split the key and value in Ini file. For example, if you using "%s:%s", `Dict4Ini` will get "%s:%s".replace('%s', '').strip(), then use ':' to split the line.
  * 2007/04/20 Version 0.8
    * Add exception process when parsing file
    * Add BOM detect
  * 2007/04/19 Version 0.7
    * Fix '\' escape
  * 2006/03/21 Version 0.6
    * Fix ordereditems bug.
  * 2006/01/04 Version 0.5
    * 2006/01/04
      * Add ordereditems() method, so you can get the items according the ini file definition
    * 2005/12/30
      * Support onelevel parameter, you can set it True, than only one level can be used, so that the key can include section delimeter char in it.
      * Support sectiondelimeter parmeter, you can set the section delimeter to which you want
  * 2005/12/12 Version 0.4
    * Fixed a bug about "\" in option's value, thanks to Andreas Kaiser
  * 2005/12/09 Version 0.3
    * Adding dict() method, then you can change the `DictIni` object to dict type, so you can really use it as a dict alternative
  * 2005/10/16 Version 0.2
    * Saving config items order
    * Support float format

## Examples ##

### Example 1 Create a ini file ###
```
    import dict4ini

    x = dict4ini.DictIni('test.ini')
    x.common.name = 'limodou'
    x.common.bool = 1
    x.common.list = [3, 1.2, 'Hello', 'have spaces']
    x.save()
```

This example will save option to test.ini. As you can see, you needn't create section "common" at first, just think it's there, it's ok. The result of test.ini is:

```
[common]
list = 3,1.2,Hello,"have spaces",
bool = 1
name = limodou
```

And you can see, once the value has special chars, just like ' ', ',', '\"', etc, the string will be quoted by double quoter. But "Hello" is a single word, and it has not the special chars, so it won't be quoted. If the value is number, it'll be just like number literal, but if the value is number string, it'll be quoted by double quoter.

In this time, the `Dict4Ini` support int, float, list/tuple, string, unicode data type, for others you should convert yourself.

### Example 2 Open an existed ini file ###

```
    import dict4ini

    x = dict4ini.DictIni('test.ini')
    print x.common.bool
    print x.common.list
    print x.common.name
```

So it's easy. The result will be:

```
1
[3, 1.2, 'Hello', 'have spaces']
limodou
```

The data is auto converted to its original type.

### Example 3 Dealing default values ###

Many times, you may want to set default values of options, once it is not set in configuration file. Using `Dict4Ini`, you have many ways to do that:

#### Using dict's setdefault() method ####

```
x = dict4ini.DictIni()
x.test.setdefault('a', 'b')
print x.test.a
```

#### Passing a dict to DictIni init method ####

```
d = {'test':{'a':'b'}}
x = dict4ini.DictIni(values=d)
print x.test.a
```

#### Creating config object first, then assign a dict value ####

```
x = dict4ini.DictIni()
d = {'a':'b'}
x.test = d
print x.test.a
```

### Example 4 Saving comments in ini file ###

```
    import dict4ini
    x = dict4ini.DictIni('test.ini')

    x.common._comment = 'This is a comment test.\nThis is the second line.'
    x.common.name = 'limodou'
    x.common.comment('name', 'Input your name')
    x.common.bool = 1
    x.common.comment('bool', 'Boot type')
    x.common.list = ['3', 'Hello', 'have spaces']
    x.common.comment('list', 'list testing')
    x.save()
```

You can save comments in configuration also. Adding comments to section, you should using `x.section._comment = 'comments'`. Comments could be multi lines. Or you could use more commonly method, `x.comment()`. Just like `x.comment('common', 'comments')`. Add comments to options, you can only using comment() method, just like `x.common.comment('list', 'comments')`.

The result of the ini file will be:

```
# This is a comment test.
# This is the second line.
[common]
# Boot type
bool = 1
# list testing
list = "3",Hello,"have spaces",
# Input your name
name = limodou
```

### Example 5 Using unicode in ini file ###
#### Using default encoding ####
```
#coding=utf-8
import dict4ini
x = dict4ini.DictIni('test.ini')
x.common.name = u'中文名'
x.save()
```

Note: You should specify the coding used in the .py file. In this case is utf-8. Then I assign `x.common.name` with a unicode string. If you don't specify the encoding in create instance of the `DictIni`, the `Dict4Ini` will auto find the default encoding in the system in turns of:

```
 * local.getdefaultlocale()[1]
 * sys.getfilesystemencoding()
 * utf-8
 * The BOM of the file
```

The result of the ini file will be:

```
[common]
name = u"中文名"
```

You should notice the file encoding will be utf-8, and the name's value is like python unicode syntax. For easiness, it doesn't support using unicode in comments.

#### Specifying encoding name ####
You can also specify the encoding of ini file, just like:

```
#coding=utf-8
import dict4ini
x = dict4ini.DictIni('test.ini', encoding='gbk')
x.common.name = u'中文名'
x.save()
```

It's easy to set an encoding of ini file.

### Example 6 Using multi section ###

```
import dict4ini
x = dict4ini.DictIni('test.ini')
x.common.settings.a = 1
x.common.settings.b = ["3", "hello"]
x.special.name = 'limodou'
x.special.homepage = 'http://www.donews.net/limodou'
x.save()
```

You don't need to care if  subsection is created, you need to just use it.

The result of the ini file will be:

```
    [common/settings]
    a = 1
    b = "3",hello,


    [special]
    homepage = http://www.donews.net/limodou
    name = limodou
```

### Example 7 Getting ordered options ###
Sometimes we need to keep the order or the options according to the ini file, so how to get the ordered items?

```
ini = dict4ini.DictIni('x.ini')
for key, value in ini.ordereditems(ini):
   print key, value
```
This example is dealing with the first level section.

```
ini = dict4ini.DictIni('x.ini')
for key, value in ini.ordereditems(ini.autostring):
   print key, value
```
This example is dealing with certain section.

### Example 8 Encrypt options ###
You can also encrypt some specified sections if you need. The `Dict4Ini` ships with a p3.py module, which can be used to encrypt/decrypt.

```
import dict4ini

d = dict4ini.DictIni('tt.ini', secretKey='limodou', secretSections='a')
d.a.name = 'limodou'
d.save()
```

And the result will be
```
[a]
name = v1ToGlTNo+YoB0VF5wk1Ea1XnRIVIv7xNNKOXSTdeA==
```

Here the parameter `secretSections` can be a string list or a string with ',' delimeter, for example: 'a,b' will be treated as ['a', 'b']. You should know all subsections of a specified section will be encrypted.

### Example 8 Hide data with base64 encoding ###
The code is:
```
import dict4ini

d = dict4ini.DictIni('tt.ini', hideData=True)
d.a.name = 'limodou'
d.save()
```

And the result is:
```
[a]
name = bGltb2RvdQ==
```

### Examples waiting ###

## FAQ ##

### 1. Can I delete an option? ###

A: Yes. For example:
```
import dict4ini
x = dict4ini.DictIni('test.ini')
del x.a
x.save()
```

### 2. How to use 'xxx.xxx' style option key? ###

A: Easy. Just using dict syntax, for example:

```
x['common']['xxx.xxx'] = 'a'
```

or

```
x.common['xxx.xxx'] = 'a'
```

### 3. How to deal the key including section delimeter char, just like '/' ###

A: As you creating the `DictIni` instance, you can specify a "onelevel=True" parameter:

```
x = dict4ini.DictIni('inifile', onelevel=True)
```

But it'll not support multi section again. So you can also defined another sectiondelimeter char different from '/', just like:

```
x = dict4ini.DictIni('inifile', sectiondelimeter='@')
```

But every time you called `dict4ini.DictIni()` you may need including sectiondelimeter parameter.

### 4. Can `Dict4Ini` process the simple ini format ###

A: Yes, it can. You can just simple pass a `normal` parameter to `DictIni` class. And all value will be treated string type, and when you saving the ini file, the value will be converted to string type automatically, and in normal mode, only these types support: int, float, str, unicode(will be automatically encoded to specifial encoding). And it doesn't support multiple level sections. **And you need update to 0.9.3+**.