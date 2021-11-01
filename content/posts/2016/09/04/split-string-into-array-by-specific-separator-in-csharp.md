---
title: C#指定并保留分隔符，字符串转数组
date: 2016-09-04T14:41:00+08:00
draft: false
summary: "mmmmmmynameismickeym -> [\"m\", \"m\", \"m\", \"m\", \"m\", \"m\", \"yna\", \"m\", \"eis\", \"m\", \"ickey\", \"m\"]"
tags: ["CSharp"]
---

# 要求如下：

```
source string: mmmmmmynameismickeym

separator: m

result string []: {"m", "m", "m", "m", "m", "m", "yna", "m", "eis", "m", "ickey", "m"}
```

# 思路分析：

```plain
1 判断 source string 是否包含 separator
    1.1 若不包含，则将其包装为 string 数组返回
    1.2 若包含，则进行下列操作

2 将 source string 转换为 char 数组

3 对数组每个字符依次进行检测（循环）
    3.1 若不为分隔符，则先将内容存入临时 string 变量 temp
    3.2 若为分隔符，则检测 temp 是否为空
        - 若 temp 不为空，则先将 temp 变量的值写入 result 数组
        - 将分隔符写入 result 数组

4 循环体外，检查 temp 变量是否为 null
    - 若不为空，则将其写入 result 数组
    - 返回 string 数组 result
```

# 代码：

```cs
// 需要引入命名空间
// using System;
// using System.Linq;

static string[] splitString(string source_str, char separator)
{
    //1. 判断 source string 是否包含 separator
    //1.1 string 中不包括分隔符
    if (source_str.IndexOf(separator) == -1)
    {
        //为了返回原字符串，将其包成一个只有一项的string数组返回
        string[] source_str_pack = new string[1]; //试一试string[source_str]
        source_str_pack[0] = source_str;
        return source_str_pack;
    }

    //1.2 string 中包括分隔符
    else
    {
        //2. 将 source_str 转换为 char 数组
        char[] source = source_str.ToCharArray();
        string temp = null;
        int resultID = 0;
        string[] result = new string[source.Length];//这样的结果会有大量的 null 元素，后面在返回结果前，进行处理去除无用的 null 元素
        //3. 对数组每个字符依次进行检测
        for (int i = 0; i < source.Length; i++)
        {
            //3.1 若不为分隔符，则先将内容存入临时 string 变量 temp
            if (source[i].Equals(separator) == false)
            {
                if (temp == null)
                {
                    temp = "";
                }
                temp = temp.Insert(temp.Length, source[i].ToString());
            }
            //3.2 若为分隔符，则检测 temp 是否为空
            else
            {
                //temp不为空，先将 temp 变量的值写入 result 数组
                if (temp != null)
                {
                    result[resultID] = temp;
                    resultID++;
                    temp = null;
                }
                //将分隔符写入 result 数组
                result[resultID] = source[i].ToString();
                resultID++;
                }
            }
            //4. 检查 temp 变量是否为 null
            //若不为空，先将其写入 result 数组
            if (temp != null)
            {
                result[resultID] = temp;
                resultID++;
                temp = null;
            }
            //返回 string 数组 result
            //后面在返回结果前，进行处理去除无用的 null 元素
            result = result.Where(s => !String.IsNullOrEmpty(s)).ToArray();
            return result;
    }
}
```