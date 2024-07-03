# Excel的重要概念
- [Excel中的错误值](#excel中的错误值)
- [Excel中的常见函数](#excel中的常见函数)

# 工作实例的解决方案
### 不可见的特殊字符的处理
常见的不可见特殊字符包括空格、换行符、制表符等。可使用以下函数：
- [`TRIM(text)`](#trimtext)移除文本两端的空格。
- [`CLEAN(text)`](#cleantext)移除文本中的不可打印字符。
- [`SUBSTITUTE(text, old_text, new_text, [instance_num])`](#substitutetext-old_text-new_text-instance_num)替换文本中的某些子字符串。
 
### 提取身份证号中的出生日期
使用Excel的`MID`函数可以从两种身份证号码长度：15位（老身份证号）和18位（新身份证号）中提取出出生日期。如果在身份证前存在其他字符，需要修改子字符串的开始位置。
```excel
=IF(LEN(A2)=15,TEXT("19"&MID(A2,7,6),"0000-00-00")*1,IF(LEN(A2)=18,TEXT(MID(A2,7,8),"0000-00-00")*1,""))

# 然后将单元格改为日期格式
```
- [`IF(condition, value_if_true, value_if_false)`](#ifcondition-value_if_true-value_if_false):第一个 IF 检查身份证号是否为15位。如果是，执行 TEXT("19" & MID(A2, 7, 6), "0000-00-00") * 1；如果不是，执行第二个 IF。第二个 IF 检查身份证号是否为18位。如果是，执行 TEXT(MID(A2, 7, 8), "0000-00-00") * 1；如果不是，返回空字符串 ""。
- [`LEN(text)`](#lentext)：检查单元格 A2 中身份证号的长度。
- [`MID(text, start_num, num_chars)`](#mid): 对于15位身份证号：MID(A2, 7, 6) 从第7个字符开始提取6个字符（通常为YYMMDD格式的出生日期）。对于18位身份证号：MID(A2, 7, 8) 从第7个字符开始提取8个字符（通常为YYYYMMDD格式的出生日期）。
- `"19" & MID(A2, 7, 6)`: 对于15位身份证号将 "19" 与提取的出生年份前两位拼接起来，形成四位年份（因为老身份证号只有最后两位年份）。
- [`TEXT(value, format_text)`](#textvalue-format_text): "0000-00-00" 是日期格式，将提取的数字转换成日期格式的文本。
- `*1`: 将格式化的文本（日期）转换成Excel日期值。Excel可以将正确格式的文本日期转换为内部的日期序列值，乘以1是一个常用技巧来确保将文本转换为数值格式的日期。

### 提取身份证号中的性别
使用Excel的`IF`和`MOD`函数组合可以从18位的身份证号中提取性别。身份证号中第17位数字为性别代码，奇数代表男性，偶数代表女性。
```excel
=IF(MOD(MID(A2,17,1),2)=0,"女","男")
```
- [`MID(A2, 17, 1)`](#midtext-start_num-num_chars): 从单元格`A2`中的第17个字符开始，提取1个字符。根据身份证编码规则，这一位数字用于指示性别。
- [`MOD(number, divisor)`](#modnumber-divisor): `MOD`函数用于计算两个数相除的余数。此处用于判断第17位数字除以2的余数。
- [`IF(condition, value_if_true, value_if_false)`](#ifcondition-value_if_true-value_if_false): 如果条件（`condition`）为真，返回`value_if_true`；否则返回`value_if_false`。
  - **条件**: `MOD(MID(A2,17,1),2)=0` 判断提取的数字是否为偶数（女性）。
  - **value_if_true**: "女" - 如果条件为真，表示性别为女。
  - **value_if_true**: "男" - 如果条件为假，表示性别为男。
 
### 格式化日期和时间的函数
使用Excel的`IFERROR`和`TEXT`函数组合可以将单元格中的日期和时间统一格式化，并在转换出错时返回原始单元格的内容。这对于处理可能不符合日期格式的数据或错误输入非常有用，可以确保数据的整洁性和一致性。
```excel
首先使用函数：

=IFERROR(TEXT(A2,"yyyy/mm/dd hh:MM:ss"),A2)

第二步使用功能：分列-日期-YMD
```
- [`TEXT(A2, "yyyy/mm/dd hh:MM:ss")`](#textvalue-format_text): 尝试将单元格A2中的值按照"yyyy/mm/dd hh:MM:ss"的格式（年/月/日 时:分:秒）转换为文本。
- [`IFERROR(...)`](#iferrorvalue-value_if_error): 如果TEXT函数的转换操作出错（比如A2中的内容不是有效的日期或时间），则返回A2单元格的原始内容。

### 从地区编码中提取地区信息的流程
使用Excel函数XLOOKUP, VALUE, 和LEFT，可以分步从地区编码中提取出乡镇街道、县区、地级市的中文名称。
```excel
# 提取乡镇街道中文

=XLOOKUP(A2, '[地区编码对照表.xlsx]地区信息'!$A:$A, '[地区编码对照表.xlsx]地区信息'!$B:$B, "")

# 提取县区街道中文

=XLOOKUP(VALUE(LEFT(A2,6)), '[地区编码对照表.xlsx]地区信息'!$K:$K, '[地区编码对照表.xlsx]地区信息'!$L:$L, "")

# 提取地级市中文

=XLOOKUP(VALUE(LEFT(A2,4)), '[地区编码对照表.xlsx]地区信息'!$M:$M, '[地区编码对照表.xlsx]地区信息'!$N:$N, "")
```
- A2: 单元格A2中包含地区9位完整编码。
- [`XLOOKUP`](#xlookuplookup_value-lookup_array-return_array-if_not_found-match_mode-search_mode)的查找列: 包含地区编码的列。返回列: 包含县区名称的列。默认值: 如果查找失败，返回空字符串。
- [`LEFT`](#lefttext-num_chars)从完整地区编码中提取前6位，通常这6位代表地级市级别的编码。
- [`VALUE`](#valuetext)将字符转换为数值，以便和《地区编码对照表》中的编码格式一致。

### 中文地址进行省市县乡四级地址分词的流程
使用Excel的INDEX, MATCH, ISNUMBER, FIND 和 IF 函数组合，可以从不规则的中文地址中有效地提取省、市、县和乡镇四级地址信息。分词的顺序是先正向从省级-市级-县级-乡镇，然后再利用已有的信息反向查找，从乡镇-县级-市级-省级，最后把同级地址合并到一起。
```excel
# 正向查找省级关键词

=INDEX('[地区编码对照表.xlsx]地区信息'!$O:$O, MATCH(TRUE, ISNUMBER(FIND('[地区编码对照表.xlsx]地区信息'!$O:$O, $A2)), 0))

# 反向利用市级地址查找省级关键词

=INDEX('[地区编码对照表.xlsx]地区信息'!$O:$O, MATCH(TRUE, ISNUMBER(FIND('[地区编码对照表.xlsx]地区信息'!$N:$N, $G2)), 0))

# 然后将正反向的同级地址组合在一起

=IF(B2=0, H2, B2)
```
- [`FIND`](#findfind_text-within_text-start_num):在《地区编码对照表》中 A2（中文地址）或 G2（市级地址） 中搜索省级关键词，如果找到，则返回其位置，否则返回错误。
- [`ISNUMBER`](#isnumbervalue): 随后检查 FIND 的输出是否为数值（即是否成功找到关键词），从而为 MATCH 函数提供逻辑测试。
- [`MATCH`](#matchlookup_value-lookup_array-match_type)：判断ISNUMBER是否返回了TRUE，即使用FIND是否在A2或G2中发现包含省级关键词，0为精确匹配。
- [`INDEX`](#indexarray-row_num-column_num-area_num)：MATCH到的行号对应的省级关键词。
- [`IF`](#ifcondition-value_if_true-value_if_false)：如果正向的B2单元格的值为0，则取反向的H2单元格的值；否则，直接使用正向的B2单元格的值。

### 心血管病事件监测的个案排重
排重的基本原则：
- 相同身份证号，诊断日期小于28天，病种相同，为重复。
- 有死亡保留死亡那条记录。其他情况下总是保留日期较早的记录。
- I20包含其下的所有小数位，如I20和I20.1或I20.2是同一病种。
  - 但是I20.1和I20.2不是同一病种。

相邻两条记录排重的基本流程：  

- 分别进行向上和向下两个方向判断。按身份证、诊断日期排序后，先做诊断日期向上方向的判断。依次检查日期差，身份证号 病种ICD编码，身份证号是否重复，对I20编码判断，根据转归判断，确定本条对于上条是否需要排除。然后反向重复，即确定本条对于下条是否需要排除。最终只要任一方向的判断被标记了需排除，本条就需排除。  

- 对于可能存在3条同时重复的情况，增加判断与本条间隔一条的上下条记录重复情况。更多重复条数的情况类似处理。

```excel
# 1.使用自定义排序，将身份证号作为主要关键字，诊断日期作为次要关键字，都是按升序排序。

# 2.判断本条与上条记录的是否相差28天以内

=IF(AND(DATEDIF($C1,$C2,"D")>=0,DATEDIF($C1,$C2,"D")<28),1,0)

# 3.判断本条与上条记录的身份证号是否一致

=IF($A2=$A1,1,0)

# 4.根据第2步和第3步的结果M2,N2判断是否有必要进一步查重

=IF(AND($M2=1,$N2=1),1,0)

# 5.判断本条与上条记录是否是相同的I20下的编码

=IF(AND(LEFT($J1,FIND(".",J1&".")-1)="I20",LEFT($J2,FIND(".",J2&".")-1)="I20"),IF(AND(OR(ISNUMBER(FIND(".1",$J1)),ISNUMBER(FIND(".2",$J1))),OR(ISNUMBER(FIND(".1",$J2)),ISNUMBER(FIND(".2",$J2)))),0,1),0)

# 6.根据第4步和第5步的结果O2,P2判断是否有必要进一步查重

=IF($O2=1,IF(OR($J2=$J1,$P2=1),1,0),0)

# 7.对转归进行判断，结合第6步结果Q2确定本条对于上条是否需要排除

=IF($Q2=0,0,IF(AND(I1="存活",I2="死亡"),0,1))

# 8.判断本条与下条记录的是否相差28天以内

=IF(AND(DATEDIF($C2,$C3,"D")>=0,DATEDIF($C2,$C3,"D")<28),1,0)

# 9.判断本条与下条记录的身份证号是否一致

=IF($A2=$A3,1,0)

# 10.根据第8步和第9步的结果T2,U2判断是否有必要进一步查重

=IF(AND($T2=1,$U2=1),1,0)

# 11.判断本条与下条记录是否是相同的I20下的编码

=IF(AND(LEFT($J3,SEARCH(".",J3&".")-1)="I20",LEFT($J2,SEARCH(".",J2&".")-1)="I20"),IF(AND(OR(ISNUMBER(SEARCH(".1",$J3)),ISNUMBER(SEARCH(".2",$J3))),OR(ISNUMBER(SEARCH(".1",$J2)),ISNUMBER(SEARCH(".2",$J2)))),0,1),0)

# 12.根据第10步和第11步的结果V2,W2判断是否有必要进一步查重

=IF($V2=1,IF(OR($J2=J3,$W2=1),1,0),0)

# 13.对转归进行判断，结合第12步结果X2确定本条对于下条是否需要排除

=IF(AND($X2=1,I2="存活",I3="死亡"),1,0)

# 14.两个方向的结果R2和Y2组合起来，不等于0的即确定本条需排除

=R2+Y2

# 第2-14步嵌套后即是相邻两条的排重判断

# A列是身份证号，C列是诊断日期，I列是转归，J列是ICD编码

=IF(IF(IF(AND(IF($A2=$A1,1,0)=1,IF(AND(IFERROR(DATEDIF($C1,$C2,"D"),0)>=0,IFERROR(DATEDIF($C1,$C2,"D"),0)<28),1,0)=1),1,0)=1,IF(OR($J2=$J1,IF(AND(LEFT($J1,SEARCH(".",J1&".")-1)="I20",LEFT($J2,SEARCH(".",J2&".")-1)="I20"),IF(AND(OR(ISNUMBER(SEARCH(".1",$J1)),ISNUMBER(SEARCH(".2",$J1))),OR(ISNUMBER(SEARCH(".1",$J2)),ISNUMBER(SEARCH(".2",$J2)))),0,1),0)=1),1,0),0)=0,0,IF(AND(I1="存活",I2="死亡"),0,1))+IF(AND(IF(IF(AND(IF($A2=$A3,1,0)=1,IF(AND(DATEDIF($C2,$C3,"D")>=0,DATEDIF($C2,$C3,"D")<28),1,0)=1),1,0)=1,IF(OR($J2=J3,IF(AND(LEFT($J3,SEARCH(".",J3&".")-1)="I20",LEFT($J2,SEARCH(".",J2&".")-1)="I20"),IF(AND(OR(ISNUMBER(SEARCH(".1",$J3)),ISNUMBER(SEARCH(".2",$J3))),OR(ISNUMBER(SEARCH(".1",$J2)),ISNUMBER(SEARCH(".2",$J2)))),0,1),0)=1),1,0),0)=1,I2="存活",I3="死亡"),1,0)
```  

- [`DATEDIF`](#datedifstart_date-end_date-unit)：用于计算两个诊断日期之间的天数差异，以判断两条记录的日期是否相差28天以内。
- [`IF`](#ifcondition-value_if_true-value_if_false)：在多个步骤中使用，如判断日期差、身份证号是否相同、以及最终的排除逻辑。
- [`AND`](#andlogical1-logical2-)：检查全部给定条件是否全部为真。用于组合多个逻辑条件，如同时满足日期和身份证号相同，才考虑进一步的查重步骤。
- [`FIND`](#findfind_text-within_text-start_num)：用于查找诊断编码中是否含有特定的子编码（如".1"或".2"），帮助判断两条记录是否属于同一个I20编码下的不同子编码。
- [`LEFT`](#lefttext-num_chars)：用于提取诊断编码的主要部分（如I20），忽略其后的子编码部分，以判断主诊断编码是否一致。
- [`ISNUMBER`](#isnumbervalue)：结合 `FIND` 使用，判断 `FIND` 返回的结果是否为数值，即判断是否成功找到了子编码。
- [`OR`](#orlogical1-logical2-)：检查给定的任一条件是否为真。在判断是否同一编码时使用，如果任一记录含有特定标记，则进一步的判断逻辑会有所不同。


---


# Excel重要概念的具体内容
### Excel中的错误值

- **"####"**：这通常不是一个错误值，而是当单元格中的数字或日期太大，无法在单元格宽度内显示完整时出现的情况。增加列宽或减少显示的小数位数可以解决这个问题。

- **"#DIV/0!"**：除以零错误。当一个公式试图将一个数字除以零时，会出现这个错误。

- **"#N/A"**：不可用错误。当函数或公式无法找到适用的结果或值时，会出现这个错误，例如VLOOKUP或HLOOKUP函数在查找表中未找到匹配项。

- **"#NAME?"**：无效名称错误。当Excel遇到无法识别的文本，比如一个不存在的命名范围、错误的公式或者函数名时，会出现这个错误。

- **"#NUM!"**：数值错误。当数字过大或太小，超出Excel可处理的范围，或者公式或函数中使用了无效的数字参数时，会出现这个错误。

- **"#VALUE!"**：值错误。当公式或函数中的参数类型不正确，比如在需要数字的情况下提供了文本，或者DATEVALUE函数的日期格式不正确时，会出现这个错误。

- **"#REF!"**：无效引用错误。当公式引用了不存在的单元格，比如被删除的单元格或工作表，会出现这个错误。

- **"#NULL!"**：空交互错误。当公式试图引用空的相交区域（即两个不重叠的范围），比如使用空的INTERSECT函数时，会出现这个错误。

- **"#SPILL!"**：溢出错误。当动态数组操作导致数据溢出到相邻单元格时，会出现这个错误。

### Excel中的常见函数
##### AND(logical1, [logical2], ...)
- **功能**: 用于检查所有提供的条件是否全部为真。
- **参数**:
  - logical1: 第一个条件。
  - logical2, ...: 可选，其他条件。

##### TRIM(text)
- **功能**: 移除文本两端的空格。
- **参数**:
  - `text`: 需要处理的文本字符串。

##### CLEAN(text)
- **功能**: 移除文本中的不可打印字符。
- **参数**:
  - `text`: 需要处理的文本字符串。

##### DATEDIF(start_date, end_date, unit)
- **功能**: 用于计算两个日期之间的差异。
- **参数**:
  - start_date: 开始日期。
  - end_date: 结束日期。
  - unit: 日期差的单位，"D"表示天数，"M"表示月数，"Y"表示年数。

##### OR(logical1, [logical2], ...)
- **功能**: 用于检查提供的任一条件是否为真。
- **参数**:
  - logical1: 第一个条件。
  - logical2, ...: 可选，其他条件。

##### SUBSTITUTE(text, old_text, new_text, [instance_num])
- **功能**: 替换文本中的某些子字符串。
- **参数**:
  - `text`: 需要替换的原文本字符串。
  - `old_text`: 要替换的子字符串。
  - `new_text`: 用于替换的新字符串。
  - `instance_num`: （可选）指定替换第几次出现的旧文本。如果省略，将替换所有出现的旧文本。

##### MID(text, start_num, num_chars)
- **功能**: 用于从文本字符串中提取指定长度的子字符串。
- **参数**:
  - text: 需要从中提取子字符串的原始文本。
  - start_num: 子字符串的开始位置（第一个字符的位置为1）。
  - num_chars: 需要提取的字符数量。
 
##### IF(condition, value_if_true, value_if_false)
- **功能**: 根据条件判断返回不同的结果。
- **参数**:
  - `condition`: 条件表达式，返回真或假。
  - `value_if_true`: 如果条件为真，则返回此值。
  - `value_if_false`: 如果条件为假，则返回此值。

##### MOD(number, divisor)
- **功能**: 返回两数相除的余数。
- **参数**:
  - `number`: 被除数，即需要计算余数的数值。
  - `divisor`: 除数，即用于除的数值。
 
##### IFERROR(value, value_if_error)
- **功能**: 检查公式是否有错误，有错误时返回指定的值。
- **参数**:
  - `value`: 要检查的公式或表达式。
  - `value_if_error`: 如果公式计算结果错误，将返回此值。

##### TEXT(value, format_text)
- **功能**: 将数字格式化为文本，并按照指定的格式显示。
- **参数**:
  - `value`: 要格式化的数值。
  - `format_text`: 指定数值的格式，例如日期或时间格式。

##### XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])
- **功能**: `XLOOKUP`函数用于搜索一个范围或数组中的某个值，并返回相应的结果数组中的对应项。
- **参数**:
  - `lookup_value`: 需要查找的值。
  - `lookup_array`: 用于查找`lookup_value`的数组或范围。
  - `return_array`: 当在`lookup_array`中找到匹配项时，从此数组或范围返回对应的值。
  - `if_not_found`: 可选参数，如果没有找到匹配项，将返回此值。
  - `match_mode`: 可选参数，指定匹配的类型（如精确匹配或近似匹配）。
  - `search_mode`: 可选参数，指定搜索的方向（如从头开始或从尾开始搜索）。

##### VALUE(text)
- **功能**: `VALUE`函数将包含数字的字符串转换为数字。
- **参数**:
  - `text`: 需要转换的文本字符串。

##### LEFT(text, num_chars)
- **功能**: `LEFT`函数从文本字符串的开始位置返回指定数量的字符。
- **参数**:
  - `text`: 需要从中提取字符的文本。
  - `num_chars`: 可选参数，指定需要返回的字符数。如果省略此参数，默认为1。

##### LEN(text)
- **功能**: `LEN`函数用于计算文本中的字符数，包括所有的字母、数字、特殊字符和空格。
- **参数**:
  - `text`: 需要计算长度的文本。这可以是直接输入的文本字符串，也可以是包含文本的单元格引用。

##### ISNUMBER(value)
- **功能**: `ISNUMBER` 函数用于检查给定的值是否为数值。
- **参数**:
  - `value`: 需要检查的值或表达式。

##### FIND(find_text, within_text, [start_num])
- **功能**: `FIND` 函数用于在文本字符串中查找一个子字符串，并返回子字符串的起始位置数值。如果找不到子字符串，将返回错误。
- **参数**:
  - `find_text`: 需要查找的子字符串。
  - `within_text`: 被搜索的文本字符串。
  - `start_num`: 可选参数，指定开始搜索的位置。

##### INDEX(array, row_num, [column_num], [area_num])
- **功能**: `INDEX` 函数返回数组中特定位置的值。
- **参数**:
  - `array`: 一个范围或数组。
  - `row_num`: 选取数组中的行号。
  - `column_num`: 可选参数，选取数组中的列号。

##### MATCH(lookup_value, lookup_array, [match_type])
- **功能**: `MATCH` 函数用于查找指定项在数组中的位置。
- **参数**:
  - `lookup_value`: 需要匹配的值。
  - `lookup_array`: 用于搜索的数组或范围。
  - `match_type`: 可选参数，指定匹配的类型（如精确匹配0、向下最近匹配1、向上最近匹配-1）。


