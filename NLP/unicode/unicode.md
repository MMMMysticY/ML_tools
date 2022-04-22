# unicode(tensorflow version)
NLP模型经常处理不同的语言，不同的语言又有不同的词典  Unicode是针对于几乎所有语言都可以用其进行表示文字的方法  
unicode character是0-0x0FFFF的int值 unicode string是一串0或者unicode character  

## unicode的tf表示
1. 直接通过constant表示 这样的表示的结果是一个byte形式 b'\ \ \'的形式
```python
tf.constant(u"abc")
tf.constant([u"abc", u"def"])
```
2. 通过unicode codepoint表示
```python
tf.constant([ord(s) for s in u"自然语言处理"])
```
## tf.strings处理unicode
1. tf.strings.unicode_decode:将string scalar转为code points的向量
```python
text_utf8 = tf.constant("自然语言处理") # <tf.Tensor: shape=(), dtype=string, numpy=b'\xe8\x87\xaa\xe7\x84\xb6\xe8\xaf\xad\xe8\xa8\x80\xe5\xa4\x84\xe7\x90\x86'>
tf.strings.unicode_decode(text_utf8, input_encoding='UTF-8') # <tf.Tensor: shape=(6,), dtype=int32, numpy=array([33258, 28982, 35821, 35328, 22788, 29702], dtype=int32)>
```
2. tf.strings.unicode_encode:将code points向量转为 string scalar
```python
text_chars = tf.constant([ord(s) for s in u"自然语言处理"]) # <tf.Tensor: shape=(6,), dtype=int32, numpy=array([33258, 28982, 35821, 35328, 22788, 29702], dtype=int32)>
tf.strings.unicode_encode(text_chars, output_encoding='UTF-8') # <tf.Tensor: shape=(), dtype=string, numpy=b'\xe8\x87\xaa\xe7\x84\xb6\xe8\xaf\xad\xe8\xa8\x80\xe5\xa4\x84\xe7\x90\x86'>
```
3. tf.strings.unicode_transcode:将string scalar转为其他形式的编码 如utf-8 -> utf-16-be
```python
text_utf16be = tf.constant(u"自然语言处理".encode("UTF-16-BE")) # <tf.Tensor: shape=(), dtype=string, numpy=b'\x81\xeaq6\x8b\xed\x8a\x00Y\x04t\x06'>
tf.strings.unicode_transcode(text_utf16be, input_encoding='UTF-16-BE', output_encoding='UTF-8') # tf.strings.unicode_transcode(text_utf16be, input_encoding='UTF-16-BE', output_encoding='UTF-8')
```

## 对带有batch的内容处理 RaggedTensor
```python
batch_utf8 = [s.encode('UTF-8') for s in [u'hÃllo', u'What is the weather tomorrow', u'Göödnight', u'😊']]
'''
[b'h\xc3\x83llo',
 b'What is the weather tomorrow',
 b'G\xc3\xb6\xc3\xb6dnight',
 b'\xf0\x9f\x98\x8a']
'''
batch_chars_ragged = tf.strings.unicode_decode(batch_utf8, input_encoding='UTF-8')
'''
<tf.RaggedTensor [[104, 195, 108, 108, 111],
 [87, 104, 97, 116, 32, 105, 115, 32, 116, 104, 101, 32, 119, 101, 97, 116,
  104, 101, 114, 32, 116, 111, 109, 111, 114, 114, 111, 119]               ,
 [71, 246, 246, 100, 110, 105, 103, 104, 116], [128522]]>
'''
```

## 对unicode对象的操作
1. character length 通过不同的unit统计unicode的长度有多少units tf.strings.length
2. character substrings 取unicode的substring tf.strings.substr
3. split unicode strings 对unicode string进行切断 tf.strings.unicode_split
4. byte offset for characters  获得byte offset tf.strings.unicode_decode_with_offsets

## 获得unicode表示的character所在的语言
```python
# unicode 33464代表汉字芸 1041代表西里尔语Б
# 可以直接处理list对象
uscript = tf.strings.unicode_script([33464, 1041])  # ['芸', 'Б']
print(uscript.numpy())  # [17, 8] == [USCRIPT_HAN, USCRIPT_CYRILLIC]
# unicode_script之后得到了17->汉语 8->西里尔语
```


