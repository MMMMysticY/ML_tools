# Q2B_B2Q
一个字母或数字占一个汉字的位置叫全角 占半个汉字叫半角  
全角字符unicode编码从65281-65374(十六进制0xFF01-0xFF5E)  
半角字符unicode编码从33-126(十六进制 0x21-0x7E)  
空格比较特殊 全角为12288(0x3000) 半角为32(0x20)  
中文文字永远是全角 只有英文字母、数字键、符号键才有全角半角的概念  
## Q2B
strQ2B是将全角字转为半角字的方法  
空格(12288)直接转化为(32)  
其他部分除空格外 减去65248进行转化  
## B2Q
strB2Q是将半角字转为全角字的方法  
空格(32)直接转化为(12288)  
其他部分除空格外 加上65248进行转化  