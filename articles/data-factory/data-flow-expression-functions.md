---
title: 映射数据流中的表达式函数
description: 了解映射数据流中的表达式函数。
author: kromerm
ms.author: makromer
manager: anandsub
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/06/2020
ms.openlocfilehash: 5645bd37ec466619a4093d767db1c846b90a4365
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92281426"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>映射数据流中的数据转换表达式

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

## <a name="expression-functions"></a>表达式函数

在数据工厂中，使用映射数据流功能的表达式语言来配置数据转换。
___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
数字的绝对值。  
* ``abs(-20) -> 20``  
* ``abs(10) -> 10``  
___   
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算余弦反值。  
* ``acos(1) -> 0.0``  
___
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
添加一对字符串或数字。 将日期添加到日期数。 将持续时间添加到时间戳。 将类似类型的数组追加到另一个数组。 与 + 运算符相同。  
* ``add(10, 20) -> 30``  
* ``10 + 20 -> 30``  
* ``add('ice', 'cream') -> 'icecream'``  
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``  
* ``add(toDate('2012-12-12'), 3) -> toDate('2012-12-15')``  
* ``toDate('2012-12-12') + 3 -> toDate('2012-12-15')``  
* ``[10, 20] + [30, 40] -> [10, 20, 30, 40]``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
将日期添加到日期或时间戳。 与 date 的 + 运算符相同。  
* ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')``  
___
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral, [<i>&lt;value3&gt;</i> : string]) => datetime</b></code><br/><br/>
将月份添加到日期或时间戳。 您可以选择传递时区。  
* ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')``  
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> toTimestamp('2016-08-31 10:10:10')``  
___
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
“逻辑与”运算符。 与 && 相同。  
* ``and(true, false) -> false``  
* ``true && false -> false``  
___
### <code>array</code>
<code><b>array([<i>&lt;value1&gt;</i> : any], ...) => array</b></code><br/><br/>
创建项的数组。 所有项应属于同一类型。 如果未指定任何项，则默认值为空字符串数组。 与 [] 创建运算符相同。  
* ``array('Seattle', 'Washington')``
* ``['Seattle', 'Washington']``
* ``['Seattle', 'Washington'][1]``
* ``'Washington'``
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算反正弦值。  
* ``asin(0) -> 0.0``  
___
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算反正切值。  
* ``atan(0) -> 0.0``  
___
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
返回平面的正 x 轴与坐标给定的点之间的角度（以弧度表示）。  
* ``atan2(0, 0) -> 0.0``  
___
### <code>byOrigin</code>
<code><b>byOrigin(<i>&lt;column name&gt;</i> : string, [<i>&lt;origin stream name&gt;</i> : string]) => any</b></code><br/><br/>
按名称在源流中选择列值。 第二个参数是源流名称。 如果有多个匹配项，则返回第一个匹配项。 如果没有匹配项，则返回 NULL 值。 返回的值必须为类型转换函数之一转换 (TO_DATE，TO_STRING ... ) 。设计时已知的列名应该只按名称进行寻址。 不支持计算输入，但可以使用参数替换。  
* ``toString(byOrigin('ancestor', 'ancestorStream'))``
___
### <code>byOrigins</code>
<code><b>byOrigins(<i>&lt;column names&gt;</i> : array, [<i>&lt;origin stream name&gt;</i> : string]) => any</b></code><br/><br/>
在流中按名称选择列的数组。 第二个参数是它所源自的流。 如果有多个匹配项，则返回第一个匹配项。 如果没有匹配项，则返回 NULL 值。 返回的值必须是由类型转换函数之一转换 (TO_DATE，TO_STRING ... 在设计时已知的 ) 列名应该只按名称进行寻址。 不支持计算输入，但可以使用参数替换。
* ``toString(byOrigins(['ancestor1', 'ancestor2'], 'ancestorStream'))``
___
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
在流中按名称选择列值。 可以将可选流名称作为第二个参数传递。 如果有多个匹配项，则返回第一个匹配项。 如果没有匹配项，则返回 NULL 值。 返回的值必须是由某种类型转换函数（TO_DATE、TO_STRING...）转换的类型。设计时已知的列名应该只按名称进行寻址。 不支持计算输入，但可以使用参数替换。  
* ``toString(byName('parent'))``  
* ``toLong(byName('income'))``  
* ``toBoolean(byName('foster'))``  
* ``toLong(byName($debtCol))``  
* ``toString(byName('Bogus Column'))``  
* ``toString(byName('Bogus Column', 'DeriveStream'))``  
___
### <code>byNames</code>
<code><b>byNames(<i>&lt;column names&gt;</i> : array, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
在流中按名称选择列的数组。 可以将可选流名称作为第二个参数传递。 如果有多个匹配项，则返回第一个匹配项。 如果列没有匹配项，则整个输出为 NULL 值。 返回的值需要类型转换函数（toDate、toString...）。设计时已知的列名应该只按名称进行寻址。 不支持计算输入，但可以使用参数替换。
* ``toString(byNames(['parent', 'child']))``
* ``byNames(['parent']) ? string``
* ``toLong(byNames(['income']))``
* ``byNames(['income']) ? long``
* ``toBoolean(byNames(['foster']))``
* ``toLong(byNames($debtCols))``
* ``toString(byNames(['a Column']))``
* ``toString(byNames(['a Column'], 'DeriveStream'))``
* ``byNames(['orderItem']) ? (itemName as string, itemQty as integer)``
___
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
根据列在流中的相对位置（从 1 开始）选择列值。 如果位置超出界限，则返回 NULL 值。 返回的值必须是由类型转换函数之一转换 (TO_DATE，TO_STRING ) 计算的输入不受支持，但你可以使用参数替换。  
* ``toString(byPosition(1))``  
* ``toDecimal(byPosition(2), 10, 2)``  
* ``toBoolean(byName(4))``  
* ``toString(byName($colName))``  
* ``toString(byPosition(1234))``  
___
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
根据备用条件应用一个值或另一个值。 如果输入数为偶数，则在最后一个条件下，另一个默认值为 NULL。  
* ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``case(10 + 20 == 25, 'bojjus', 'do' < 'go', 'gunchus') -> 'gunchus'``  
* ``isNull(case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus')) -> true``  
* ``case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus', 'dumbo') -> 'dumbo'``  
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算一个数字的立方根。  
* ``cbrt(8) -> 2.0``  
___
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
返回不小于数字的最小整数。  
* ``ceil(-0.1) -> 0``  
___
### <code>coalesce</code>
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
返回一组输入中的第一个非 NULL 值。 所有输入应属于同一类型。  
* ``coalesce(10, 20) -> 10``  
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``  
___
### <code>collect</code>
<code><b>collect(<i>&lt;value1&gt;</i> : any) => array</b></code><br/><br/>
将聚合组中的表达式的所有值收集到一个数组中。 在此过程中，可以收集结构并将其转换为备用结构。 项目数将等于该组中的行数，并且可以包含 NULL 值。 收集的项的数量应该很小。  
* ``collect(salesPerson)``
* ``collect(firstName + lastName))``
* ``collect(@(name = salesPerson, sales = salesAmount) )``
___
### <code>columnNames</code>
<code><b>columnNames(<i>&lt;value1&gt;</i> : string) => array</b></code><br/><br/>
获取流的所有输出列。 可以将可选流名称作为第二个参数传递。  
* ``columnNames()``
* ``columnNames('DeriveStream')``

___
### <code>columns</code>
<code><b>columns([<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
获取流的所有输出列。 可以将可选流名称作为第二个参数传递。   
* ``columns()``
* ``columns('DeriveStream')``
___
### <code>compare</code>
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
比较同一类型的两个值。 如果 value1 < value2，则返回负整数; 如果 value1 = = value2，则为 0; 如果 value1 > value2，则返回正值。  
* ``(compare(12, 24) < 1) -> true``  
* ``(compare('dumbo', 'dum') > 0) -> true``  
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
将字符串的可变数字连接在一起。 与字符串的 + 运算符相同。  
* ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'``  
* ``'dataflow' + 'is' + 'awesome' -> 'dataflowisawesome'``  
* ``isNull('sql' + null) -> true``  
___
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
使用分隔符将字符串的可变数字连接在一起。 第一个参数是分隔符。  
* ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'``  
* ``isNull(concatWS(null, 'dataflow', 'is', 'awesome')) -> true``  
* ``concatWS(' is ', 'dataflow', 'awesome') -> 'dataflow is awesome'``  
___
### <code>contains</code>
<code><b>contains(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => boolean</b></code><br/><br/>
如果所提供的数组中的任何元素在提供的谓词中计算结果为 true，则返回 true。 Contains 需要引用谓词函数中的一个元素作为 #item。  
* ``contains([1, 2, 3, 4], #item == 3) -> true``  
* ``contains([1, 2, 3, 4], #item > 5) -> false``  
___
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算余弦值。  
* ``cos(10) -> -0.8390715290764524``  
___
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
计算值的双曲余弦值。  
* ``cosh(0) -> 1.0``  
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
根据位长度计算不同基元数据类型的列集的 CRC32 哈希，值只能为 0(256)、224、256、384、512。 它可用于计算行的指纹。  
* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L``  
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
当此作业开始运行时获取当前日期。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 当地时区用作默认值。 有关可用格式，请参阅 Java 的 `SimpleDateFormat` 类。 [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). 
* ``currentDate() == toDate('2250-12-31') -> false``  
* ``currentDate('PST')  == toDate('2250-12-31') -> false``  
* ``currentDate('America/New_York')  == toDate('2250-12-31') -> false``  
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
获取作业开始与本地时区一起运行时的当前时间戳。  
* ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false``  
___
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
获取 UTC 格式的当前时间戳。 如果希望在与群集时区不同的时区中解释当前时间，则可以传递格式为 "GMT"、"PST"、"UTC"、"中美洲/开曼" 的可选时区。 默认为当前时区。 有关可用格式，请参阅 Java 的 `SimpleDateFormat` 类。 [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). 若要将 UTC 时间转换为其他时区，请使用 `fromUTC()` 。  
* ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``currentUTC() != toTimestamp('2050-12-12 19:18:12') -> true``  
* ``fromUTC(currentUTC(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
获取给定日期的月份日期。  
* ``dayOfMonth(toDate('2018-06-08')) -> 8``  
___
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
获取给定日期的星期日期。 1-星期日，2-星期一 ...，7-星期六。  
* ``dayOfWeek(toDate('2018-06-08')) -> 6``  
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
获取给定日期的一年中的第几天。  
* ``dayOfYear(toDate('2016-04-09')) -> 100``  
___
### <code>days</code>
<code><b>days(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
天数的持续时间（以毫秒为单位）。  
* ``days(2) -> 172800000L``  
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
将弧度转换为度。  
* ``degrees(3.141592653589793) -> 180``  
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
将数字对相除。 与运算符相同 `/` 。  
* ``divide(20, 10) -> 2``  
* ``20 / 10 -> 2``  
___
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
检查字符串是否以提供的字符串结尾。  
* ``endsWith('dumbo', 'mbo') -> true``  
___
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
“等于”比较运算符。 与 = = 运算符相同。  
* ``equals(12, 24) -> false``  
* ``12 == 24 -> false``  
* ``'bad' == 'bad' -> true``  
* ``isNull('good' == toString(null)) -> true``  
* ``isNull(null == null) -> true``  
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
忽略大小写的“等于”比较运算符。 与 <=> 运算符相同。  
* ``'abc'<=>'Abc' -> true``  
* ``equalsIgnoreCase('abc', 'Abc') -> true``  
___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
计算数字的阶乘。  
* ``factorial(5) -> 120``  
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
始终返回 false 值。 `syntax(false())`如果存在名为 "false" 的列，请使用函数。  
* ``(10 + 20 > 30) -> false``  
* ``(10 + 20 > 30) -> false()``
___
### <code>filter</code>
<code><b>filter(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => array</b></code><br/><br/>
筛选出数组中不满足所提供谓词的元素。 筛选器需要引用谓词函数中的一个元素作为 #item。  
* ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]``  
* ``filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') -> ['a', 'b']``  
___
### <code>find</code>
<code><b>find(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
查找数组中与条件相匹配的第一项。 它采用筛选器函数，可在其中将数组中的项作为 #item 进行寻址。 对于深层嵌套的地图，可以使用 #item_n ( # item_1，#item_2 ... ) 表示法来引用父地图。  
* ``find([10, 20, 30], #item > 10) -> 20``
* ``find(['azure', 'data', 'factory'], length(#item) > 4) -> 'azure'``
* ``find([
      @(
         name = 'Daniel',
         types = [
                   @(mood = 'jovial', behavior = 'terrific'),
                   @(mood = 'grumpy', behavior = 'bad')
                 ]
        ),
      @(
         name = 'Mark',
         types = [
                   @(mood = 'happy', behavior = 'awesome'),
                   @(mood = 'calm', behavior = 'reclusive')
                 ]
        )
      ],
      contains(#item.types, #item.mood=='happy')  /*Filter out the happy kid*/
    )``
* ``
     @(
           name = 'Mark',
           types = [
                     @(mood = 'happy', behavior = 'awesome'),
                     @(mood = 'calm', behavior = 'reclusive')
                   ]
      )
    ``  
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 返回不大的最大整数 than the number.  
* ``floor(-0.1) -> -1``  
___
### <code>fromBase64</code>
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> 编码给定的tring in base64.  
* ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``  
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></代码><br/><br/> 转换为 UTC 格式的时间戳。 可以选择以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递时区。 它默认为可用格式的当前 timezo ne. Refer Java's `S i mpleDateFormat 的类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></代码><br/><br/> 比较更大运算符。 豪e as > operator.  
* ``greater(12, 24) -> false``  
* ``('dumbo' > 'dum') -> true``  
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></代码><br/><br/> 比较大于或等于运算符。 相同 as >= operator.  
* ``greaterOrEqual(12, 12) -> true``  
* ``('dumbo' >= 'dum') -> true``  
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></代码><br/><br/> 返回值列表中的最大值，因为输入将跳过 null 值。 如果 all inputs are null.  
* ``greatest(10, 30, ，则返回 null 15, 20) -> 30``  
* ``greatest(10, toInteger(null), 20) -> 20``  
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``  
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></代码><br/><br/> 按名称在流中检查列值。 可以将可选流名称作为第二个参数传递。 设计时已知的列名应该只按名称进行寻址。 不支持计算输入，但可以使用 languager substitutions.  
* ``hasColumn('parent')``  
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></代码><br/><br/> 获取时间戳的小时值。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 本地时区用作 lt. Refer Java's `S i 可用格式的默认 mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``  
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``  
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></代码><br/><br/> 持续时间（以毫秒为单位） number of hours.  
* ``hours(2) -> 7200000L``  
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></基于条件的代码><br/><br/> 应用一个值或另一个值。 如果未指定另一个值，则将它视为 NULL。 这两个值都必须兼容 (numeric, string...).  
* ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``  
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``  
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></代码><br/><br/> 检查第一个参数是否为 null。 如果不为 NULL，则返回第一个参数。 如果为 NULL，则返回第二个参数。 如果指定了三个参数，则行为与 iif (isNull (value1) ，value2，value3) ，并返回第三个参数（如果第一个 va lue is not null.  
* ``iifNull (10, 20) -> 10``  
* ``iifNull(null,20, 40) -> 20``  
* ``iifNull('azure', 'data', 'factory') -> 'factory'``  
* ``iifNull(null, 'data', 'factory') -> 'data'``  
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></代码><br/><br/> 检查项是否 is in the array.  
* ``in([10, 20, 30], 10) -> true``  
* ``in(['good', 'kid'], 'bad') -> false``  
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> 将每个单词的首字母转换为大写。 将单词标识为单独的d by whitespace.  
* ``initCap('cool iceCREAM') -> 'Cool Icecream'``  
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></代码><br/><br/> 查找字符串中的子字符串 (1) 的位置。 0返回ed if not found.  
* ``instr('dumbo', 'mbo') -> 3``  
* ``instr('microsoft', 'o') -> 5``  
* ``instr('good', 'bad') -> 0``  
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为要删除。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isDelete()``  
* ``isDelete(1)``  
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为 "错误"。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isError()``  
* ``isError(1)``  
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为忽略。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isIgnore()``  
* ``isIgnore(1)``  
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为要插入。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isInsert()``  
* ``isInsert(1)``  
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查行是否在查找时匹配。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isMatch()``  
* ``isMatch(1)``  
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></代码><br/><br/> 检查e value is NULL.  
* ``isNull(NULL()) -> true``  
* ``isNull('') -> false``  
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为更新。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isUpdate()``  
* ``isUpdate(1)``  
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></代码><br/><br/> 检查是否将行标记为要插入。 对于采用多个输入流的转换，可以传递流的（从 1 开始）索引。 流索引应为1或2，以及定义ault value is 1.  
* ``isUpsert()``  
* ``isUpsert(1)``  
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></代码><br/><br/> 获取星期一的最后日期th given a date.  
* ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``  
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></代码><br/><br/> 比较小于或等于运算符。 相同 as <= operator.  
* ``least(10, 30,15, 20) -> 10``  
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``  
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></代码><br/><br/> 提取从索引1开始的子字符串，其中包含多个字符。 与 SUBST 相同RING(str, 1, n).  
* ``left('bojjus', 2) -> 'bo'``  
* ``left('bojjus', 20) -> 'bojjus'``  
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></代码><br/><br/> 返回长度h of the string.  
* ``length('dumbo') -> 5``  
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></代码><br/><br/> 比较 less 运算符。 豪e as < operator.  
* ``lesser(12, 24) -> true``  
* ``('abcd' < 'abc') -> false``  
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></代码><br/><br/> 比较小于或等于运算符。 相同 as <= operator.  
* ``lesserOrEqual(12, 12) -> true``  
* ``('dumbo' <= 'dum') -> false``  
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></代码><br/><br/> 获取 levenshtein 距离 between two strings.  
* ``levenshtein('boys', 'girls') -> 4``  
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></代码><br/><br/> 模式是按原义匹配的字符串。 以下特殊符号除外：_ 与输入中的任何一个字符匹配（类似于 在 posix regu l ar 表达式中) % 与输入 (中的零个或多个字符相匹配，在 posix regul a r 表达式中) 。
转义字符为 ''。 如果转义字符的之前带有特殊符号或其他转义字符，则在字面上匹配后面的字符。 转义任何 other character.  
* ``like('icecream', 'ice%') -> true``  
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></代码><br/><br/>，将查找字符串中从特定位置开始的子字符串 (1) 位置。 如果省略位置，则视为从字符串开头查找。 0返回 ed if not found.  
* ``locate('mbo','dumbo') -> 3``  
* ``locate('o', 'microsoft', 6) -> 7``  
* ``locate('bad', 'good') -> 0``  
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></代码><br/><br/> 计算日志值。 可以为其他欧拉提供可选的基 number if used.  
* ``log(100, 10) -> 2``  
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 计算日志值 based on 10 base.  
* ``log10(100) -> 2``  
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> Lowercases a string.  
* ``lower('GunChus') -> 'gunchus'``  
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></代码><br/><br/> 左按所提供的填充填充字符串，直到达到特定长度。 如果字符串等于或大于该长度，则为 "> trimme" " d to the length.  
* ``lpad('dumbo', 10, '-') - > '-----dumbo'``  
* ``lpad('dumbo', 4,  '-') ( lpad" "dumbo" 的 ""， 8, '<>') -> '<><dumbo'``  ___
### <code>ltrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></代码><br/><br/> 左剪裁前导字符的字符串。 如果未指定第二个参数，则裁剪掉空格。 否则，它会修整中指定的任何字符econd parameter.  
* ``ltrim('  dumbo  ') -> 'dumbo  '``  
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``  
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></代码><br/><br/> 将数组的每个元素映射到使用提供的表达式的新元素。 Map 需要对 expression fu 中的一个元素的引用。nction as #item.  
* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``  
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``  
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></代码><br/><br/> 将数组的每个元素映射到使用提供的表达式的新元素。 Map 需要引用 expression 函数中的一个元素作为 #item 和对元素的引用 index as #index.  
* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``  
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></代码><br/><br/> 计算一组不同基元数据类型的列的 MD5 摘要，并返回32个字符的十六进制字符串。 它可用于计算手指print for a row.  
* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``  
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></代码><br/><br/> 获取日期的毫秒值。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 本地时区用作 lt. Refer Java's `S i 可用格式的默认 mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></代码><br/><br/> 持续时间（以毫秒为单位） of milliseconds.  
* ``milliseconds(2) -> 2L``  
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></代码><br/><br/> 减去数字。 从某一日期减去天数。 从时间戳中减去持续时间。 减去两个时间戳以获取差异（以毫秒为单位）。 相同于 the - operator.  
* ``minus(20, 10) -> 10``  
* ``20 - 10 -> 10``  
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``  
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``  
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></代码><br/><br/> 获取时间戳的分钟值。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 本地时区用作 lt. Refer Java's `S i 可用格式的默认 mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``  
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``  
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></用于 nu 的代码><br/><br/> 持续时间（以毫秒为单位）mber of minutes.  
* ``minutes(2) -> 120000L``  
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></代码><br/><br/> 一对数字的模数。 相同于 the % operator.  
* ``mod(20, 8) -> 4``  
* ``20 % 8 -> 4``  
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></代码><br/><br/> 获取 da 的月份值te or timestamp.  
* ``month(toDate('2012-8-8')) -> 8``  
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></代码><br/><br/> 获取两个日期之间的月数。 可以舍入计算。可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 本地时区用作 lt. Refer Java's `S i 可用格式的默认 mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``  
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></代码><br/><br/> 相乘的数字对。 相同于 the * operator.  
* ``multiply(20, 10) -> 200``  
* ``20 * 10 -> 200``  
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 会对数字求反。 将正数变为负数 and vice versa.  
* ``negate(13) -> -13``  
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></代码><br/><br/> 返回下一个唯一序列。 该数字仅在分区内是连续的，并以 the partitionId.  
* ``nextSequence() == 12313112 -> false``  
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></代码><br/><br/> 标准化字符串值，以区分重音code characters.  
* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``  
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></代码><br/><br/> 逻辑 negation operator.  
* ``not(true) -> false``  
* ``not(10 == 20) -> true``  
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></代码><br/><br/> 比较不等于运算符。 相同 as != operator.  
* ``12!= 24 -> true``  
* ``'bojjus' != 'bo' + 'jjus' -> false``  
___
### <code>notNull</code>
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></代码><br/><br/> 检查 value is not NULL.  
* ``notNull(NULL()) -> false``  
* ``notNull('') -> true``  
___
### <code>null</code>
<code><b>null() => null</b></代码><br/><br/> 返回 NULL 值。 Use the function `如果存在名为 "null" 的列，则语法 (null ( # A2 # B3 "。 使用的任何操作都将 r esult in a NULL.  
* ``isNull('dumbo' + null) -> true``  
* ``isNull(10 *null) -> true``  
* ``isNull('') -> false``  
* ``isNull(10 + 20) -> false``  
* ``isNull(10/0) -> true``  
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></代码><br/><br/> 逻辑或 operator. Same as ||.  
* ``or(true, false) -> true``  
* ``true || false -> true``  
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></代码><br/><br/> 正模数 pair of numbers.  
* ``pmod(-20, 8) -> 4``  
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></代码><br/><br/> 返回当前分区 id input row is in.  
* ``partitionId()``  
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></代码><br/><br/> 对 p 引发一个数字ower of another.  
* ``power(10, 2) -> 100``  
___
### <code>random</code>
<code><b>random(<i>&lt;value1&gt;</i> : integral) => long</b></代码><br/><br/> 返回给定分区内的可选种子的随机数字。 种子应为固定值，并且与 partitionId 和 produ 一起使用。ce random values  
* ``random(1) == 1 -> false``
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></代码><br/><br/> 会积累数组中的元素。 减少需要引用第一个表达式函数中的累加器和一个元素作为 #acc 和 #item，并且它期望在第二个表达式中使用 #result 的结果值ession function.  
* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``  
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></代码><br/><br/> 提取给定正则表达式模式匹配的子字符串。 最后一个参数标识匹配组并且默认为 1 if omitted-24000b. 使用 "<正则表达式>" (后引号) 来匹配字符串 without escaping.  
* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``  
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``  
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></代码><br/><br/> 检查字符串是否与给定的 rege 匹配x pattern. 使用 "<正则表达式>" (后引号) 来匹配字符串 without escaping.  
* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``  
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``  
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></代码><br/><br/> 将出现的所有 regex 模式替换为 gi 中的另一个子字符串ven string 使用 "<正则表达式>" (后引号) 来匹配字符串 without escaping.  
* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``  
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``  
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></代码><br/><br/> 基于基于 regex 的分隔符拆分字符串，并返回rray of strings.  
* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``  
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``  
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``  
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``  
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></代码><br/><br/> 将出现的所有子字符串替换为给定字符串中的另一个子字符串。 如果省略最后一个参数，则它是默认值 to empty string.  
* ``replace('doggie dog', 'dog', 'cat') - > 'catgie cat'``  
* ``replace('doggie dog', 'dog','') -> 'gie '``  
* ``replace('doggie dog', 'dog') -> 'gie '``  
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> Reverses a string.  
* ``reverse('gunchus') -> 'suhcnug'``  
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></代码><br/><br/> 提取包含右侧字符数的子字符串。 与 SUBSTRING (str 相同，LENGTH(str) - n, n).  
* ``right('bojjus', 2) -> 'us'``  
* ``right('bojjus', 20) -> 'bojjus'``  
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></代码><br/><br/> 检查字符串是否与给定的匹配n regex pattern.  
* ``rlike('200.50', `(\d+).(\d+)`) -> true``  
* ``rlike('bogus', `M[0-9]+.*`) -> false``  
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></代码><br/><br/> 在给定可选比例和可选舍入模式的情况下舍入数值。 如果省略小数位数，则默认为 0。 如果省略模式，则默认为 ROUND_HALF_UP(5)。 Ounding 的值（ r 包括 u de） ROUND_U P 2-ROUND_DOWN 3-ROUND_CEILING 4-ROUND_FLOOR 5-ROUND_HALF_UP 6-ROUND_HALF_DOWN 8-ROUND_HALF_EVEN 8-ROUND_UNNECESSARY.  
* ``round(100.123) -> 100.0``  
* ``round(2.5, 0) -> 3.0``  
* ``round(5.3999999999999995, 2, 7) -> 5.40``  
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></代码><br/><br/> 右键按照提供的填充填充字符串，直到达到特定长度。 如果字符串等于或大于长度，则为 trimmed to the length.  
* ``rpad('dumbo', 10, '-') -> 'dumbo-----'``  
* ``rpad('dumbo', 4, '-') -> 'dumb'``  
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'``  ___
### <code>rtrim</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></代码><br/><br/> 右键修整尾随字符的字符串。 如果未指定第二个参数，则裁剪掉空格。 否则，它会修整中指定的任何字符econd parameter.  
* ``rtrim('  dumbo  ') -> '  dumbo'``  
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``  
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></代码><br/><br/> 获取日期的第二个值。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 本地时区用作 lt. Refer Java's `S i 可用格式的默认 mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``  
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></用于 nu 的代码><br/><br/> 持续时间（以毫秒为单位）mber of seconds.  
* ``seconds(2) -> 2000L``  
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></代码><br/><br/> 计算一组不同基元数据类型的列的 SHA-1 摘要，并返回40个字符的十六进制字符串。 它可用于计算手指print for a row.  
* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``  
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></代码><br/><br/> 计算一组可变基元数据类型的一组列的 SHA-1 摘要（给定位长度，只能为值 0 (256) 、224、256、384、512）。 它可用于计算手指print for a row.  
* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``  
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> Calculates a sine value.  
* ``sin(2) -> 0.9092974268256817``  
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 计算 hyperbolic sine value.  
* ``sinh(0) -> 0.0``  
___
### <code>size</code>
<code><b>size(<i>&lt;value1&gt;</i> : any) => integer</b></代码><br/><br/> 查找rray or map type  
* ``size(['element1', 'element2']) -> 2``
* ``size([1,2,3]) -> 3``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></代码><br/><br/> 从位置提取数组的子集。 位置从 1 开始。 如果省略长度，则默认为 end of the string.  
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``  
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``  
* ``slice([10, 20, 30, 40], 2)[1] -> 20``  
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``  
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``  
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``  
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></代码><br/><br/> 使用提供的谓词函数对数组进行排序。 Sort 需要对 expression 函数中的两个连续元素的引用作为 #item1 and #item2.  
* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``  
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``  
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> 获取 soundex 代码 for the string.  
* ``soundex('genius') -> 'G520'``  
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></代码><br/><br/> 根据分隔符拆分字符串并返回一个rray of strings.  
* ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``  
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``  
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 计算方块字 root of a number.  
* ``sqrt(9) -> 3``  
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></代码><br/><br/> 检查字符串是否以 supplied string.  
* ``startsWith('dumbo', 'du') -> true``  
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></代码><br/><br/> 用日期或时间戳减去天数。 与-op 相同erator for date.  
* ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``  
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></代码><br/><br/> 减去一个 da 中的月te or timestamp.  
* ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``  
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></代码><br/><br/> 从位置提取特定长度的子字符串。 位置从 1 开始。 如果省略长度，则默认为 en d of the string.  
* ``substring('Cat in the hat',5, 2) -> 'in'``  
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``  
* ``substring('Cat in the hat', 5) -> 'in the hat'``  
* ``substring('Cat in the hat', 100, 100) -> ''``  
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 计算 a tangent value.  
* ``tan(0) -> 0.0``  
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 计算 hyperbolic tangent value.  
* ``tanh(0) -> 0.0``  
___
### <code>toBase64</code>
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> 编码给定的tring in base64.  
* ``toBase64('bojjus') -> 'Ym9qanVz'``  
___
### <code>toBinary</code>
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></代码><br/><br/> 将任何数字/日期/时间戳/字符串转换为二进制 representation.  
* ``toBinary(3) -> [0x11]``  
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></代码><br/><br/> 将值 ( "t"、"true"、"y"、"是"、"1" ) 为 true，并将 ( "f"、"false"、"n"、"no"、"0" ) 为 false 且为 NULL any other value.  
* ``toBoolean('true') -> true``  
* ``toBoolean('n') -> false``  
* ``isNull(toBoolean('truthy')) -> true``  
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></代码><br/><br/> 将输入日期字符串转换为日期，使用可选的输入日期格式 at. Refer Java's `S i mpleDateFormat "类作为可用格式。 如果省略输入日期格式，则默认格式为 yyyy-[M]M-[d]d。 接受的格式为： [yyyy，yyyy-[M] M，yyyy-[M] M-[d] d，yyyy-[M]M-[d]dT* ].  
* ``toDate('2012-8-18') -> toDate('2012-08-18')``  
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``  
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></代码><br/><br/> 将任何数字或字符串转换为十进制值。 如果未指定精度和小数位数，则默认为 (10,2)。可以使用可选的 Java 十进制格式进行转换。 格式为 BCP47 语言的可选区域设置格式，如 en-US, de, zh-CN.  
* ``toDecimal(123.45) -> 123.45``  
* ``toDecimal('123.45', 8, 4) -> 123.4500``  
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``  
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``  
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></代码><br/><br/> 将任何数值或字符串转换为双精度值。 可以使用可选的 Java 十进制格式进行转换。 格式为 BCP47 语言的可选区域设置格式，如 en-US, de, zh-CN.  
* ``toDouble(123.45) -> 123.45``  
* ``toDouble('123.45') -> 123.45``  
* ``toDouble('$123.45', '$###.00') -> 123.45``  
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``  
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></代码><br/><br/> 将任何数字或字符串转换为浮点值。 可以使用可选的 Java 十进制格式进行转换。 Truncates any double.  
* ``toFloat(123.45) -> 123.45f``  
* ``toFloat('123.45') -> 123.45f``  
* ``toFloat('$123.45', '$###.00') -> 123.45f``  
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></代码><br/><br/> 将任何数字或字符串转换为整数值。 可以使用可选的 Java 十进制格式进行转换。 截断任意长, float, double.  
* ``toInteger(123) -> 123``  
* ``toInteger('123') -> 123``  
* ``toInteger('$123', '$###') -> 123``  
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></代码><br/><br/> 将任何数字或字符串转换为长值。 可以使用可选的 Java 十进制格式进行转换。 截断y float, double.  
* ``toLong(123) -> 123``  
* ``toLong('123') -> 123``  
* ``toLong('$123', '$###') -> 123``  
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></代码><br/><br/> 将任何数字或字符串转换为短值。 可以使用可选的 Java 十进制格式进行转换。 截断任意整数，长整型, float, double.  
* ``toShort(123) -> 123``  
* ``toShort('123') -> 123``  
* ``toShort('$123', '$###') -> 123``  
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></代码><br/><br/> 将基元数据类型转换为字符串。 对于数字和日期，可以指定格式。 如果未指定，则选择系统默认值。对数字使用 Java 十进制格式。 有关所有可能的日期格式，请参阅 Java SimpleDateFormat;t is yyyy-MM-dd.  
* ``toStri ng(10) -> '10'``  
* ``toString('engineer') -> 'engineer'``  
* ``toString(123456.789, '##,###.##') - 默认 > '123,456.79'``  
* ``toString(123.78, '000000.000') - 估价 > '000123.780'``  
* ``toString(12345, '##0.#####E0')-> '12.345E3'``  
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``  
* ``isNull(toString(toDate('2018-12-31', 'MM/dd/yy'))) -> true``  
* ``toString(4 == 20) -> 'false'``  
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></代码><br/><br/> 将字符串转换为给定可选时间戳格式的时间戳。 如果省略时间戳，则使用默认模式 yyyy-[M] M-[d] d hh： mm： ss [. f ...]。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 对于可用格式，时间戳最多支持毫秒准确性，值为 9 99. Refer Java's `S i mpleDateFormat "类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``  
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></代码><br/><br/> 将时间戳转换为 UTC。 可以“GMT”、“PST”、“UTC”、“America/Cayman”格式传递可选的时区。 它默认为可用格式的当前 timezo ne. Refer Java's `S i mpleDateFormat 的类。 https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></代码><br/><br/> 将一组字符替换为字符串中的另一组字符。 字符具有1个 t o 1 replacement.  
* ``translate('(bojjus)', '()', '[]')-> '[bojjus]'``  
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``  
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></代码><br/><br/> 修整前导和尾随字符的字符串。 如果未指定第二个参数，则裁剪掉空格。 否则，它会修整中指定的任何字符econd parameter.  
* ``trim('  dumbo  ') -> 'dumbo'``  
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``  
___
### <code>true</code>
<code><b>true() => boolean</b></代码><br/><br/> 始终返回 true 值。 Use the function `语法 (true ( # A2 # B3 ' if mmn named 'true'.  
* ``(10 + 20 == 30) -> true``  
* ``(10 + 20 == 30) -> true()``  
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></代码><br/><br/> 与列的类型相匹配。 仅可用于模式表达式。数字匹配 short、integer、long、double、float 或 decimal、integer 与 short、integer、long、decimal 匹配 double、float、decimal 和 datetime 匹配日期或 timestamp type.  
* ``typeMatch(type, 'number')``  
* ``typeMatch('date', 'datetime')``  
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></代码><br/><br/> Uppercases a string.  
* ``upper('bojjus') -> 'BOJJUS'``  
___
### <code>uuid</code>
<code><b>uuid() => string</b></代码><br/><br/> 返回 generated UUID.  
* ``uuid()``  
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></代码><br/><br/> 获取了从 year given a date.  
* ``weekOfYear(toDate('2008-02-20')) -> 8``  
___
### <code>weeks</code>
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></代码><br/><br/> 持续时间（以毫秒为单位） number of weeks.  
* ``weeks(2) -> 1209600000L``  
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></代码><br/><br/> 逻辑 XOR 运算符。 豪e as ^ operator.  
* ``xor(true, false) -> true``  
* ``xor(true, true) -> false``  
* ``true ^ false -> true``  
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></代码><br/><br/> 获取年份 value of a date.  
* ``year(toDate('2012- 8-8')) -> 2012 "" # # 聚合 r egate 函数以下函数仅在聚合、透视、逆透视和窗口中可用transformations.
___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 获取 val 的平均值ues of a column.  
* ``avg(sales)``  
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></基于条件的代码><br/><br/> 获取 val 的平均值ues of a column.  
* ``avgIf(region == 'West', sales)``  
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></代码><br/><br/> 获取值的聚合计数。 如果指定了可选列 (s) ，则将忽略 NULL valu es in the count.  
* ` `count(custId)``  
* ``count(cus tId, custName)``* ``count()``  
* ``count(iif(isNull(custId), 1, NULL))``  
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></代码><br/><br/> 获取的非重复值的聚合计数 set of columns.  
* ``countDistinct(custId, custName)``  
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></基于条件的代码><br/><br/> 获取值的聚合计数。 如果指定了可选列，则将忽略 NULL values in the count.  
* ``countIf(state == 'CA' && commission < 10000, name)``  
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></代码><br/><br/> 获取总体协方差 between two columns.  
* ``covariancePopulation(sales, profit)``  
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取总体协方差 of two columns.  
* ``covariancePopulationIf(region == 'West', sales)``  
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></代码><br/><br/> 获取样本协方差 of two columns.  
* ``covarianceSample(sales, profit)``  
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取样本协方差 of two columns.  
* ``covarianceSampleIf(region == 'West', sales, profit)``  
___
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></代码><br/><br/> 获取列组的第一个值。 如果省略第二个参数 ignoreNulls，则s assumed false.  
* ``first(sales)``  
* ``first(sales, false)``  
___
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取 kurtosis of a column.  
* ``kurtosis(sales)``  
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></代码><基于条件的 br/><br/> 获取 kurtosis of a column.  
* ``kurtosisIf(region == 'West', sales)``  
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></代码><br/><br/> 获取列组的最后一个值。 如果省略第二个参数 ignoreNulls，则 s assumed false.  
*``last(sales)``  
* ``last(sales, false)``  
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></代码><br/><br/> 获取最大 value of a column.  
* ``max(sales)``  
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></基于条件的代码><br/><br/> 获取最大 value of a column.  
* ``maxIf(region == 'West', sales)``  
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 获取 m 值的平均值mn. Same as AVG.  
* ``mean(sales)``  
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></基于条件的代码><br/><br/> 获取列的值的平均值. Same as avgIf.  
* ``meanIf(region == 'West', sales)``  
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></代码><br/><br/> 获取最低 value of a column.  
* ``min(sales)``  
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></基于条件的代码><br/><br/> 获取最低 value of a column.  
* ``minIf(region == 'West', sales)``  
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取 skewness of a column.  
* ``skewness(sales)``  
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></代码><基于条件的 br/><br/> 获取 skewness of a column.  
* ``skewnessIf(region == 'West', sales)``  
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取标准 deviation of a column.  
* ``stdDev(sales)``  
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取标准 deviation of a column.  
* ``stddevIf(region == 'West', sales)``  
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取总体标准 deviation of a column.  
* ``stddevPopulation(sales)``  
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取总体标准 deviation of a column.  
* ``stddevPopulationIf(region == 'West', sales)``  
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取示例标准 deviation of a column.  
* ``stddevSample(sales)``  
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></代码><基于条件的 br/><br/> 获取示例标准 deviation of a column.  
* ``stddevSampleIf(region == 'West', sales)``  
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 获取聚合总和 numeric column.  
* ``sum(col)``  
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></代码><br/><br/> 获取的非重复值的聚合和 numeric column.  
* ``sumDistinct(col)``  
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></基于条件的代码><br/><br/> 获取数值列的聚合总和。 条件可以是基数 d on any column.  
* ``sumDistinctIf(state == 'CA' && commission <10000, sales)``  
* ``sumDistinctIf(true, sales)``  
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></基于条件的代码><br/><br/> 获取数值列的聚合总和。 条件可以是基数 d on any column.  
* ``sumIf(state == 'CA' && commission <10000, sales)``  
* ``sumIf(true, sales)``  
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取 variance of a column.  
* ``variance(sales)``  
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取 variance of a column.  
* ``varianceIf(region == 'West', sales)``  
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取总体 variance of a column.  
* ``variancePopulation(sales)``  
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></基于条件的代码><br/><br/> 获取总体 variance of a column.  
* ``variancePopulationIf(region == 'West', sales)``  
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></代码><br/><br/> 获取无偏差 variance of a column.  
* ``varianceSample(sales)``  
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></代码><基于条件的 br/><br/> 获取非偏置的 varia nce of a column.  
* ``varianceSampleIf(region ==  'West' ，销售) "" # # W 窗口函数仅在窗口中可用transformations.
___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></代码><br/><br/> CumeDist 函数计算相对于分区中所有值的值的位置。 结果是前面的行数，或者等于当前行在分区中的顺序除以窗口分区中的总行数。 排序中的任何并列值都将计算为第 the same position.  
* ``cumeDist()``  
___
### <code>denseRank</code>
<code><b>denseRank() => integer</b></代码><br/><br/> 计算在窗口的 order by 子句中指定的一组值中的值的排名。 结果是 1 加上前面的行数，或者等于当前行在分区中的顺序。 值不会在序列中生成空隙。 即使数据未排序并查找 c，密集排名仍有效hange in values.  
* ``denseRank()``  
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></代码><br/><br/> 获取第一个参数的值，该参数在当前行之前计算 n 行。 第二个参数是要反向查找的行数，默认值为 1。 如果没有多行，则返回值为 null 的值，除非默认值为ue is specified.  
* ``lag(amount, 2)``  
* ``lag(amount, 2000, 100)``  
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></代码><br/><br/> 获取第一个参数的值，该参数在当前行之后计算 n 行。 第二个参数是要正向查找的行数，默认值为 1。 如果没有多行，则返回值为 null 的值，除非默认值为ue is specified.  
* ``lead(amount, 2)``  
* ``lead(amount, 2000, 100)``  
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></代码><br/><br/> NTile 函数将每个 windo tition 的行 w pa r 分为 "n" 个存储桶， rom 最多为 "n"。 桶值最大相差 1。 如果分区中的行数不能均匀分割成桶数，则余值将逐个分布在每个桶中，从第一个桶开始。 NTile 函数适合用于计算三分位数、四分位数、十分位数和其他常见的摘要统计数据。 在初始化期间，该函数将计算两个变量：常规桶的大小将额外添加一行。 这两个变量都以当前分区的大小为基础。 在计算过程中，该函数将跟踪当前行号、当前桶号，以及发生桶更改的行号 (bucketThreshold)。 当当前行号达到 bucket 阈值时，存储桶值将增加1，并按 bucket 大小增加阈值 (在当前 bu cket is padded).* ``nTile()``  
* ``nTile(numOfBuckets)``  
___
### <code>rank</code>
<code><b>rank() => integer</b></代码><br/><br/> 计算在窗口的 order by 子句中指定的一组值中的值的排名。 结果是 1 加上前面的行数，或者等于当前行在分区中的顺序。 值将在序列中生成空隙。 即使数据未排序且查找 change in values.  
* ``rank()``  
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></代码><br/><br/> 为窗口中的行分配顺序行号 starting with 1.  
* ``rowNumber()``* ``
 @(
       name = 'Mark',
       types = [
                 @(mood = 'happy', behavior = 'awesome'),
                 @(mood = 'calm', behavior = 'reclusive')
               ]
  )
``  
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Returns the largest integer not greater than the number.  
* ``floor(-0.1) -> -1``  
___
### <code>fromBase64</code>
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Encodes the given string in base64.  
* ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``  
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Converts to the timestamp from UTC. You can optionally pass the timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. It is defaulted to the current timezone. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Comparison greater operator. Same as > operator.  
* ``greater(12, 24) -> false``  
* ``('dumbo' > 'dum') -> true``  
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Comparison greater than or equal operator. Same as >= operator.  
* ``greaterOrEqual(12, 12) -> true``  
* ``('dumbo' >= 'dum') -> true``  
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Returns the greatest value among the list of values as input skipping null values. Returns null if all inputs are null.  
* ``greatest(10, 30, 15, 20) -> 30``  
* ``greatest(10, toInteger(null), 20) -> 20``  
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``  
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></code><br/><br/>
Checks for a column value by name in the stream. You can pass a optional stream name as the second argument. Column names known at design time should be addressed just by their name. Computed inputs are not supported but you can use parameter substitutions.  
* ``hasColumn('parent')``  
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Gets the hour value of a timestamp. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. The local timezone is used as the default. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``  
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``  
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duration in milliseconds for number of hours.  
* ``hours(2) -> 7200000L``  
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Based on a condition applies one value or the other. If other is unspecified it is considered NULL. Both the values must be compatible(numeric, string...).  
* ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``  
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``  
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></code><br/><br/>
Checks if the first parameter is null. If not null, the first parameter is returned. If null, the second parameter is returned. If three parameters are specified, the behavior is the same as iif(isNull(value1), value2, value3) and the third parameter is returned if the first value is not null.  
* ``iifNull(10, 20) -> 10``  
* ``iifNull(null, 20, 40) -> 20``  
* ``iifNull('azure', 'data', 'factory') -> 'factory'``  
* ``iifNull(null, 'data', 'factory') -> 'data'``  
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Checks if an item is in the array.  
* ``in([10, 20, 30], 10) -> true``  
* ``in(['good', 'kid'], 'bad') -> false``  
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Converts the first letter of every word to uppercase. Words are identified as separated by whitespace.  
* ``initCap('cool iceCREAM') -> 'Cool Icecream'``  
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Finds the position(1 based) of the substring within a string. 0 is returned if not found.  
* ``instr('dumbo', 'mbo') -> 3``  
* ``instr('microsoft', 'o') -> 5``  
* ``instr('good', 'bad') -> 0``  
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked for delete. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isDelete()``  
* ``isDelete(1)``  
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked as error. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isError()``  
* ``isError(1)``  
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked to be ignored. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isIgnore()``  
* ``isIgnore(1)``  
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked for insert. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isInsert()``  
* ``isInsert(1)``  
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is matched at lookup. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isMatch()``  
* ``isMatch(1)``  
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Checks if the value is NULL.  
* ``isNull(NULL()) -> true``  
* ``isNull('') -> false``  
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked for update. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isUpdate()``  
* ``isUpdate(1)``  
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Checks if the row is marked for insert. For transformations taking more than one input stream you can pass the (1-based) index of the stream. The stream index should be either 1 or 2 and the default value is 1.  
* ``isUpsert()``  
* ``isUpsert(1)``  
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Gets the last date of the month given a date.  
* ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``  
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Comparison lesser than or equal operator. Same as <= operator.  
* ``least(10, 30, 15, 20) -> 10``  
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``  
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Extracts a substring start at index 1 with number of characters. Same as SUBSTRING(str, 1, n).  
* ``left('bojjus', 2) -> 'bo'``  
* ``left('bojjus', 20) -> 'bojjus'``  
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Returns the length of the string.  
* ``length('dumbo') -> 5``  
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Comparison less operator. Same as < operator.  
* ``lesser(12, 24) -> true``  
* ``('abcd' < 'abc') -> false``  
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Comparison lesser than or equal operator. Same as <= operator.  
* ``lesserOrEqual(12, 12) -> true``  
* ``('dumbo' <= 'dum') -> false``  
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Gets the levenshtein distance between two strings.  
* ``levenshtein('boys', 'girls') -> 4``  
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
The pattern is a string that is matched literally. The exceptions are the following special symbols:  _ matches any one character in the input (similar to . in posix regular expressions)
  % matches zero or more characters in the input (similar to .* in posix regular expressions).
  The escape character is ''. If an escape character precedes a special symbol or another escape character, the following character is matched literally. It is invalid to escape any other character.  
* ``like('icecream', 'ice%') -> true``  
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Finds the position(1 based) of the substring within a string starting a certain position. If the position is omitted it is considered from the beginning of the string. 0 is returned if not found.  
* ``locate('mbo', 'dumbo') -> 3``  
* ``locate('o', 'microsoft', 6) -> 7``  
* ``locate('bad', 'good') -> 0``  
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Calculates log value. An optional base can be supplied else a Euler number if used.  
* ``log(100, 10) -> 2``  
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates log value based on 10 base.  
* ``log10(100) -> 2``  
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Lowercases a string.  
* ``lower('GunChus') -> 'gunchus'``  
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Left pads the string by the supplied padding until it is of a certain length. If the string is equal to or greater than the length, then it is trimmed to the length.  
* ``lpad('dumbo', 10, '-') -> '-----dumbo'``  
* ``lpad('dumbo', 4, '-') -> 'dumb'``  
* ``lpad('dumbo', 8, '<>') -> '<><dumbo'``  
___
### <code>ltrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Left trims a string of leading characters. If second parameter is unspecified, it trims whitespace. Else it trims any character specified in the second parameter.  
* ``ltrim('  dumbo  ') -> 'dumbo  '``  
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``  
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item.  
* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``  
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``  
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/>
Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item and a reference to the element index as #index.  
* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``  
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Calculates the MD5 digest of set of column of varying primitive datatypes and returns a 32 character hex string. It can be used to calculate a fingerprint for a row.  
* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``  
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Gets the millisecond value of a date. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. The local timezone is used as the default. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duration in milliseconds for number of milliseconds.  
* ``milliseconds(2) -> 2L``  
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Subtracts numbers. Subtract number of days from a date. Subtract duration from a timestamp. Subtract two timestamps to get difference in milliseconds. Same as the - operator.  
* ``minus(20, 10) -> 10``  
* ``20 - 10 -> 10``  
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``  
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``  
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Gets the minute value of a timestamp. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. The local timezone is used as the default. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``  
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``  
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duration in milliseconds for number of minutes.  
* ``minutes(2) -> 120000L``  
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Modulus of pair of numbers. Same as the % operator.  
* ``mod(20, 8) -> 4``  
* ``20 % 8 -> 4``  
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Gets the month value of a date or timestamp.  
* ``month(toDate('2012-8-8')) -> 8``  
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></code><br/><br/>
Gets the number of months between two dates. You can round off the calculation.You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. The local timezone is used as the default. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``  
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Multiplies pair of numbers. Same as the * operator.  
* ``multiply(20, 10) -> 200``  
* ``20 * 10 -> 200``  
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Negates a number. Turns positive numbers to negative and vice versa.  
* ``negate(13) -> -13``  
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Returns the next unique sequence. The number is consecutive only within a partition and is prefixed by the partitionId.  
* ``nextSequence() == 12313112 -> false``  
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
Normalizes the string value to separate accented unicode characters.  
* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``  
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logical negation operator.  
* ``not(true) -> false``  
* ``not(10 == 20) -> true``  
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Comparison not equals operator. Same as != operator.  
* ``12 != 24 -> true``  
* ``'bojjus' != 'bo' + 'jjus' -> false``  
___
### <code>notNull</code>
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Checks if the value is not NULL.  
* ``notNull(NULL()) -> false``  
* ``notNull('') -> true``  
___
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
Returns a NULL value. Use the function `syntax(null())` if there is a column named 'null'. Any operation that uses will result in a NULL.  
* ``isNull('dumbo' + null) -> true``  
* ``isNull(10 * null) -> true``  
* ``isNull('') -> false``  
* ``isNull(10 + 20) -> false``  
* ``isNull(10/0) -> true``  
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logical OR operator. Same as ||.  
* ``or(true, false) -> true``  
* ``true || false -> true``  
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Positive Modulus of pair of numbers.  
* ``pmod(-20, 8) -> 4``  
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Returns the current partition id the input row is in.  
* ``partitionId()``  
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Raises one number to the power of another.  
* ``power(10, 2) -> 100``  
___
### <code>random</code>
<code><b>random(<i>&lt;value1&gt;</i> : integral) => long</b></code><br/><br/>
Returns a random number given an optional seed within a partition. The seed should be a fixed value and is used in conjunction with the partitionId to produce random values  
* ``random(1) == 1 -> false``
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Accumulates elements in an array. Reduce expects a reference to an accumulator and one element in the first expression function as #acc and #item and it expects the resulting value as #result to be used in the second expression function.  
* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``  
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Extract a matching substring for a given regex pattern. The last parameter identifies the match group and is defaulted to 1 if omitted. Use `<regex>`(back quote) to match a string without escaping.  
* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``  
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``  
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Checks if the string matches the given regex pattern. Use `<regex>`(back quote) to match a string without escaping.  
* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``  
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``  
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Replace all occurrences of a regex pattern with another substring in the given string Use `<regex>`(back quote) to match a string without escaping.  
* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``  
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``  
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Splits a string based on a delimiter based on regex and returns an array of strings.  
* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``  
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``  
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``  
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``  
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></code><br/><br/>
Replace all occurrences of a substring with another substring in the given string. If the last parameter is omitted, it is default to empty string.  
* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``  
* ``replace('doggie dog', 'dog', '') -> 'gie '``  
* ``replace('doggie dog', 'dog') -> 'gie '``  
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Reverses a string.  
* ``reverse('gunchus') -> 'suhcnug'``  
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Extracts a substring with number of characters from the right. Same as SUBSTRING(str, LENGTH(str) - n, n).  
* ``right('bojjus', 2) -> 'us'``  
* ``right('bojjus', 20) -> 'bojjus'``  
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Checks if the string matches the given regex pattern.  
* ``rlike('200.50', `(\d+).(\d+)`) -> true``  
* ``rlike('bogus', `M[0-9]+.*`) -> false``  
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Rounds a number given an optional scale and an optional rounding mode. If the scale is omitted, it is defaulted to 0. If the mode is omitted, it is defaulted to ROUND_HALF_UP(5). The values for rounding include
1 - ROUND_UP
2 - ROUND_DOWN
3 - ROUND_CEILING
4 - ROUND_FLOOR
5 - ROUND_HALF_UP
6 - ROUND_HALF_DOWN
7 - ROUND_HALF_EVEN
8 - ROUND_UNNECESSARY.  
* ``round(100.123) -> 100.0``  
* ``round(2.5, 0) -> 3.0``  
* ``round(5.3999999999999995, 2, 7) -> 5.40``  
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Right pads the string by the supplied padding until it is of a certain length. If the string is equal to or greater than the length, then it is trimmed to the length.  
* ``rpad('dumbo', 10, '-') -> 'dumbo-----'``  
* ``rpad('dumbo', 4, '-') -> 'dumb'``  
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'``  
___
### <code>rtrim</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Right trims a string of trailing characters. If second parameter is unspecified, it trims whitespace. Else it trims any character specified in the second parameter.  
* ``rtrim('  dumbo  ') -> '  dumbo'``  
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``  
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Gets the second value of a date. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. The local timezone is used as the default. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``  
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duration in milliseconds for number of seconds.  
* ``seconds(2) -> 2000L``  
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Calculates the SHA-1 digest of set of column of varying primitive datatypes and returns a 40 character hex string. It can be used to calculate a fingerprint for a row.  
* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``  
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Calculates the SHA-2 digest of set of column of varying primitive datatypes given a bit length which can only be of values 0(256), 224, 256, 384, 512. It can be used to calculate a fingerprint for a row.  
* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``  
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates a sine value.  
* ``sin(2) -> 0.9092974268256817``  
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates a hyperbolic sine value.  
* ``sinh(0) -> 0.0``  
___
### <code>size</code>
<code><b>size(<i>&lt;value1&gt;</i> : any) => integer</b></code><br/><br/>
Finds the size of an array or map type  
* ``size(['element1', 'element2']) -> 2``
* ``size([1,2,3]) -> 3``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Extracts a subset of an array from a position. Position is 1 based. If the length is omitted, it is defaulted to end of the string.  
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``  
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``  
* ``slice([10, 20, 30, 40], 2)[1] -> 20``  
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``  
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``  
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``  
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/>
Sorts the array using the provided predicate function. Sort expects a reference to two consecutive elements in the expression function as #item1 and #item2.  
* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``  
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``  
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Gets the soundex code for the string.  
* ``soundex('genius') -> 'G520'``  
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Splits a string based on a delimiter and returns an array of strings.  
* ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``  
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``  
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates the square root of a number.  
* ``sqrt(9) -> 3``  
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Checks if the string starts with the supplied string.  
* ``startsWith('dumbo', 'du') -> true``  
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Subtract days from a date or timestamp. Same as the - operator for date.  
* ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``  
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Subtract months from a date or timestamp.  
* ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``  
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Extracts a substring of a certain length from a position. Position is 1 based. If the length is omitted, it is defaulted to end of the string.  
* ``substring('Cat in the hat', 5, 2) -> 'in'``  
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``  
* ``substring('Cat in the hat', 5) -> 'in the hat'``  
* ``substring('Cat in the hat', 100, 100) -> ''``  
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates a tangent value.  
* ``tan(0) -> 0.0``  
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calculates a hyperbolic tangent value.  
* ``tanh(0) -> 0.0``  
___
### <code>toBase64</code>
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Encodes the given string in base64.  
* ``toBase64('bojjus') -> 'Ym9qanVz'``  
___
### <code>toBinary</code>
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></code><br/><br/>
Converts any numeric/date/timestamp/string to binary representation.  
* ``toBinary(3) -> [0x11]``  
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Converts a value of ('t', 'true', 'y', 'yes', '1') to true and ('f', 'false', 'n', 'no', '0') to false and NULL for any other value.  
* ``toBoolean('true') -> true``  
* ``toBoolean('n') -> false``  
* ``isNull(toBoolean('truthy')) -> true``  
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Converts input date string to date using an optional input date format. Refer Java's `SimpleDateFormat` class for available formats. If the input date format is omitted, default format is yyyy-[M]M-[d]d. Accepted formats are :[ yyyy, yyyy-[M]M, yyyy-[M]M-[d]d, yyyy-[M]M-[d]dT* ].  
* ``toDate('2012-8-18') -> toDate('2012-08-18')``  
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``  
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Converts any numeric or string to a decimal value. If precision and scale are not specified, it is defaulted to (10,2).An optional Java decimal format can be used for the conversion. An optional locale format in the form of BCP47 language like en-US, de, zh-CN.  
* ``toDecimal(123.45) -> 123.45``  
* ``toDecimal('123.45', 8, 4) -> 123.4500``  
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``  
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``  
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Converts any numeric or string to a double value. An optional Java decimal format can be used for the conversion. An optional locale format in the form of BCP47 language like en-US, de, zh-CN.  
* ``toDouble(123.45) -> 123.45``  
* ``toDouble('123.45') -> 123.45``  
* ``toDouble('$123.45', '$###.00') -> 123.45``  
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``  
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Converts any numeric or string to a float value. An optional Java decimal format can be used for the conversion. Truncates any double.  
* ``toFloat(123.45) -> 123.45f``  
* ``toFloat('123.45') -> 123.45f``  
* ``toFloat('$123.45', '$###.00') -> 123.45f``  
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Converts any numeric or string to an integer value. An optional Java decimal format can be used for the conversion. Truncates any long, float, double.  
* ``toInteger(123) -> 123``  
* ``toInteger('123') -> 123``  
* ``toInteger('$123', '$###') -> 123``  
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Converts any numeric or string to a long value. An optional Java decimal format can be used for the conversion. Truncates any float, double.  
* ``toLong(123) -> 123``  
* ``toLong('123') -> 123``  
* ``toLong('$123', '$###') -> 123``  
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Converts any numeric or string to a short value. An optional Java decimal format can be used for the conversion. Truncates any integer, long, float, double.  
* ``toShort(123) -> 123``  
* ``toShort('123') -> 123``  
* ``toShort('$123', '$###') -> 123``  
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Converts a primitive datatype to a string. For numbers and date a format can be specified. If unspecified the system default is picked.Java decimal format is used for numbers. Refer to Java SimpleDateFormat for all possible date formats; the default format is yyyy-MM-dd.  
* ``toString(10) -> '10'``  
* ``toString('engineer') -> 'engineer'``  
* ``toString(123456.789, '##,###.##') -> '123,456.79'``  
* ``toString(123.78, '000000.000') -> '000123.780'``  
* ``toString(12345, '##0.#####E0') -> '12.345E3'``  
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``  
* ``isNull(toString(toDate('2018-12-31', 'MM/dd/yy'))) -> true``  
* ``toString(4 == 20) -> 'false'``  
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Converts a string to a timestamp given an optional timestamp format. If the timestamp is omitted the default pattern yyyy-[M]M-[d]d hh:mm:ss[.f...] is used. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. Timestamp supports up to millisecond accuracy with value of 999. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``  
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Converts the timestamp to UTC. You can pass an optional timezone in the form of 'GMT', 'PST', 'UTC', 'America/Cayman'. It is defaulted to the current timezone. Refer Java's `SimpleDateFormat` class for available formats. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Replace one set of characters by another set of characters in the string. Characters have 1 to 1 replacement.  
* ``translate('(bojjus)', '()', '[]') -> '[bojjus]'``  
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``  
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Trims a string of leading and trailing characters. If second parameter is unspecified, it trims whitespace. Else it trims any character specified in the second parameter.  
* ``trim('  dumbo  ') -> 'dumbo'``  
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``  
___
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Always returns a true value. Use the function `syntax(true())` if there is a column named 'true'.  
* ``(10 + 20 == 30) -> true``  
* ``(10 + 20 == 30) -> true()``  
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Matches the type of the column. Can only be used in pattern expressions.number matches short, integer, long, double, float or decimal, integral matches short, integer, long, fractional matches double, float, decimal and datetime matches date or timestamp type.  
* ``typeMatch(type, 'number')``  
* ``typeMatch('date', 'datetime')``  
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Uppercases a string.  
* ``upper('bojjus') -> 'BOJJUS'``  
___
### <code>uuid</code>
<code><b>uuid() => string</b></code><br/><br/>
Returns the generated UUID.  
* ``uuid()``  
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Gets the week of the year given a date.  
* ``weekOfYear(toDate('2008-02-20')) -> 8``  
___
### <code>weeks</code>
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duration in milliseconds for number of weeks.  
* ``weeks(2) -> 1209600000L``  
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logical XOR operator. Same as ^ operator.  
* ``xor(true, false) -> true``  
* ``xor(true, true) -> false``  
* ``true ^ false -> true``  
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Gets the year value of a date.  
* ``year(toDate('2012-8-8')) -> 2012``  
## Aggregate functions
The following functions are only available in aggregate, pivot, unpivot, and window transformations.
___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Gets the average of values of a column.  
* ``avg(sales)``  
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Based on a criteria gets the average of values of a column.  
* ``avgIf(region == 'West', sales)``  
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Gets the aggregate count of values. If the optional column(s) is specified, it ignores NULL values in the count.  
* ``count(custId)``  
* ``count(custId, custName)``  
* ``count()``  
* ``count(iif(isNull(custId), 1, NULL))``  
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Gets the aggregate count of distinct values of a set of columns.  
* ``countDistinct(custId, custName)``  
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Based on a criteria gets the aggregate count of values. If the optional column is specified, it ignores NULL values in the count.  
* ``countIf(state == 'CA' && commission < 10000, name)``  
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Gets the population covariance between two columns.  
* ``covariancePopulation(sales, profit)``  
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the population covariance of two columns.  
* ``covariancePopulationIf(region == 'West', sales)``  
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Gets the sample covariance of two columns.  
* ``covarianceSample(sales, profit)``  
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the sample covariance of two columns.  
* ``covarianceSampleIf(region == 'West', sales, profit)``  
___
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Gets the first value of a column group. If the second parameter ignoreNulls is omitted, it is assumed false.  
* ``first(sales)``  
* ``first(sales, false)``  
___
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the kurtosis of a column.  
* ``kurtosis(sales)``  
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the kurtosis of a column.  
* ``kurtosisIf(region == 'West', sales)``  
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Gets the last value of a column group. If the second parameter ignoreNulls is omitted, it is assumed false.  
* ``last(sales)``  
* ``last(sales, false)``  
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Gets the maximum value of a column.  
* ``max(sales)``  
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Based on a criteria, gets the maximum value of a column.  
* ``maxIf(region == 'West', sales)``  
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Gets the mean of values of a column. Same as AVG.  
* ``mean(sales)``  
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Based on a criteria gets the mean of values of a column. Same as avgIf.  
* ``meanIf(region == 'West', sales)``  
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Gets the minimum value of a column.  
* ``min(sales)``  
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Based on a criteria, gets the minimum value of a column.  
* ``minIf(region == 'West', sales)``  
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the skewness of a column.  
* ``skewness(sales)``  
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the skewness of a column.  
* ``skewnessIf(region == 'West', sales)``  
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the standard deviation of a column.  
* ``stdDev(sales)``  
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the standard deviation of a column.  
* ``stddevIf(region == 'West', sales)``  
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the population standard deviation of a column.  
* ``stddevPopulation(sales)``  
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the population standard deviation of a column.  
* ``stddevPopulationIf(region == 'West', sales)``  
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the sample standard deviation of a column.  
* ``stddevSample(sales)``  
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the sample standard deviation of a column.  
* ``stddevSampleIf(region == 'West', sales)``  
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Gets the aggregate sum of a numeric column.  
* ``sum(col)``  
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Gets the aggregate sum of distinct values of a numeric column.  
* ``sumDistinct(col)``  
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Based on criteria gets the aggregate sum of a numeric column. The condition can be based on any column.  
* ``sumDistinctIf(state == 'CA' && commission < 10000, sales)``  
* ``sumDistinctIf(true, sales)``  
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Based on criteria gets the aggregate sum of a numeric column. The condition can be based on any column.  
* ``sumIf(state == 'CA' && commission < 10000, sales)``  
* ``sumIf(true, sales)``  
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the variance of a column.  
* ``variance(sales)``  
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the variance of a column.  
* ``varianceIf(region == 'West', sales)``  
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the population variance of a column.  
* ``variancePopulation(sales)``  
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the population variance of a column.  
* ``variancePopulationIf(region == 'West', sales)``  
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Gets the unbiased variance of a column.  
* ``varianceSample(sales)``  
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Based on a criteria, gets the unbiased variance of a column.  
* ``varianceSampleIf(region == 'West', sales)``  
## Window functions
The following functions are only available in window transformations.
___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
The CumeDist function computes the position of a value relative to all values in the partition. The result is the number of rows preceding or equal to the current row in the ordering of the partition divided by the total number of rows in the window partition. Any tie values in the  ordering will evaluate to the same position.  
* ``cumeDist()``  
___
### <code>denseRank</code>
<code><b>denseRank() => integer</b></code><br/><br/>
Computes the rank of a value in a group of values specified in a window's order by clause. The result is one plus the number of rows preceding or equal to the current row in the ordering of the partition. The values will not produce gaps in the sequence. Dense Rank works even when data is not sorted and looks for change in values.  
* ``denseRank()``  
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Gets the value of the first parameter evaluated n rows before the current row. The second parameter is the number of rows to look back and the default value is 1. If there are not as many rows a value of null is returned unless a default value is specified.  
* ``lag(amount, 2)``  
* ``lag(amount, 2000, 100)``  
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Gets the value of the first parameter evaluated n rows after the current row. The second parameter is the number of rows to look forward and the default value is 1. If there are not as many rows a value of null is returned unless a default value is specified.  
* ``lead(amount, 2)``  
* ``lead(amount, 2000, 100)``  
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
The NTile function divides the rows for each window partition into `n` buckets ranging from 1 to at most `n`. Bucket values will differ by at most 1. If the number of rows in the partition does not divide evenly into the number of buckets, then the remainder values are distributed one per bucket, starting with the first bucket. The NTile function is useful for the calculation of tertiles, quartiles, deciles, and other common summary statistics. The function calculates two variables during initialization: The size of a regular bucket will have one extra row added to it. Both variables are based on the size of the current partition. During the calculation process the function keeps track of the current row number, the current bucket number, and the row number at which the bucket will change (bucketThreshold). When the current row number reaches bucket threshold, the bucket value is increased by one and the threshold is increased by the bucket size (plus one extra if the current bucket is padded).  
* ``nTile()``  
* ``nTile(numOfBuckets)``  
___
### <code>rank</code>
<code><b>rank() => integer</b></code><br/><br/>
Computes the rank of a value in a group of values specified in a window's order by clause. The result is one plus the number of rows preceding or equal to the current row in the ordering of the partition. The values will produce gaps in the sequence. Rank works even when data is not sorted and looks for change in values.  
* ``rank()``  
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
Assigns a sequential row numbering for rows in a window starting with 1.  
* ``rowNumber()``  

## <a name="next-steps"></a>后续步骤

[了解如何使用表达式生成器](concepts-data-flow-expression-builder.md)。
