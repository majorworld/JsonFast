# JSONFast 1.0.6

#### （1）介绍
JSON序列化工具，无第三方依赖，可单独复制[JSONFast.cs](https://github.com/majorworld/JsonFast/blob/main/JsonFast.cs)类文件使用，仅900行代码

#### （2）软件架构
最低适用于net 4.0及以后版本

除了数组、集合和基础类型，还包括Color、Point、DataTable、DataSet和Byte[]的序列化和反序列化，兼容Newstonsoft.Json序列化结果

#### （3）安装教程

复制[JSONFast.cs](https://github.com/majorworld/JsonFast/blob/main/JsonFast.cs)一个文件到自己项目中，就可以使用两个拓展方法JSONFrom()反序列化和JSONTo()序列化

#### （4）使用说明


##### 1、反序列化为字典，返回这种类型比Newstonsoft.Json更快

```cs
string str = "{\"state\":true,\"time\":\"2020-10-10 1:2:1\",\"num\":-33,\r\n\t\f     \"name\":\"你好\r\n\t\f左\b右，\\\"世界\\\"\",\"age\":9.9,\"yy\":{\"sex\":null}}";
Dictionary<string, object> t1 = str.JSONFrom();

```

##### 2、反序列化为实体类或动态类
```cs
string str = "{\"Name\":\"高老师\",\"Num\":3.1415926,\"Col\":\"2,3,4\"}";
Teacher t1 = str.JSONFrom<Teacher>();
dynamic t2 = str.JSONFrom<dynamic>();
```


##### 3、对象或集合序列化字符串
```cs
List<Student> list = new List<Student>();
string t1 = list.JSONTo();
```

##### 4、判断是不是数组
```cs
string str1 = "{\"A\":{\"a\":1,\"b\":2}}";
string str2 = "{\"A\":[1,2]}";

var dy1 = str1.JSONFrom();
if (dy1["A"] is IList)
    Console.WriteLine("d1里是数组");

var dy2 = str2.JSONFrom();
if (dy2["A"] is IList)
    Console.WriteLine("d2里是数组");
```


##### 5、类似Newstonsoft的JObject和JArray
```cs
//JObject
Dictionary<string, object> dict = new Dictionary<string, object>();
dict.Add("name", "tom");
dict.Add("age", 18);

//JArray
List<Dictionary<string, object>> list = new List<Dictionary<string, object>>();
list.Add(dict);

//序列化和反序列化
var stringArr = list.JsonTo();
Console.WriteLine(stringArr);
var Arr = stringArr.JsonFrom<List<Dictionary<string, object>>>();
```

##### 6、如果有字段存在无限递归引用，抛出异常System.StackOverflowException:“Exception of type 'System.StackOverflowException' was thrown.”，可使用[FastIgnore]特性过滤掉问题字段
```cs
class UserItem
{
    public User User { get; set; }
    public int UserId { get; set; }
}
class User
{
    public string Name { get; set; }
    //过滤掉无限递归引用的问题字段
    [FastIgnore] 
    public UserItem Bad { get; set; }
}
```

##### 7、支持对unicode字符串的解析
```cs
string str = "{\"Name\":\"\\u4f60\\u597d\"}";
string json = str.JsonFrom().JsonTo();
Console.WriteLine(json);// "{\"Name\":\"你好\"}";
```


#### （5）更新日志


| 更新日志 |版本|
|------------|--|
| 2022-01-03 |1.0.6|
| 1、优化代码结构，防止私有拓展方法全局污染||
| 2022-12-29 |1.0.5|
| 1、添加对unicode字符串的解析||
| 2022-12-28 |1.0.4|
| 1、修复了转DataTable时，字段存在null报错问题 ||
| 2、添加了对nullable的支持 ||
| 3、修改命名空间为System，方便使用 ||
| 2022-12-26 |1.0.3|
| 1、添加了对枚举的反序列化支持 ||


 #### （6）单元测试

如果没有net6和net7环境，可以将JsonFastBenchmark.csproj文件中的  
```xml
<TargetFrameworks>net461;netcoreapp3.1;net6.0;net7.0</TargetFrameworks>  
```
改成
```xml  
<TargetFrameworks>net461;netcoreapp3.1</TargetFrameworks>  
```

### （7）使用案例
[https://gitee.com/dotnetchina/TouchSocket](https://gitee.com/dotnetchina/TouchSocket)

[链接](https://gitee.com/dotnetchina/TouchSocket/blob/master/src/TouchSocket/Core/Serialization/Json/JsonFast.cs)
 


