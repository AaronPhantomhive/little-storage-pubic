# WebApi开发相关

## Jersey

https://eclipse-ee4j.github.io/jersey.github.io/documentation/latest/index.html



### 注解

`@Path`

相对 URI 路径

eg: `@Path("/users/{username}")`



`@GET、@PUT、@POST、@DELETE、...（HTTP）`

@GET、@PUT、@POST、@DELETE和@HEAD 是 由 JAX-RS 定义的资源方法指示符 注释，对应于类似名称的 HTTP 方法。



`@Consumes`

指定处理请求的提交内容类型（Content-Type），例如application/json, text/html

eg: `@Consumes(MediaType.APPLICATION_JSON)`



`@Produces`

指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回

eg: `@Produces(MediaType.APPLICATION_JSON)`

### 流程




### 序列化

将Java对象序列化成byte数组，保存到数据库为bytea类型的字段中。

`byte[data] = SerializationUtils.serialize(javaObject);`

反序列化：`SerializationUtils.deserialize`

### Jackson注解：java <=> xml

#### java对象转成xml格式

- 创建XmlMapper对象

  ```java
  XmlMapper xmlMapper = new XmlMapper();
  ```

- 使用@JacksonXmlRootElement，@JacksonXmlProperty等注解来标注java对象的属性和节点

  - namespace：用于指定XML命名空间的名称

  - localName：用于指定XML节点的标签名

  - isAttribute：用于指定该属性是作为XML的属性还是作为子标签

    ```java
    @JacksonXmlProperty(isAttribute = true)
    ```

- 使用xmlMapper.writeValueAsString或xmlMapper.writeValue方法来将java对象序列化为xml字符串或文件

  ```java
  String xml = xmlMapper.writeValueAsString(new SimpleBean());
  ```

- 使用xmlMapper.readValue方法来将xml字符串或文件反序列化为java对象

- CDATA value可以使用@JacksonXmlCData注解来指定一个属性的值是要被包裹在CDATA标签中的，例如：

  ```xml
  <content><![CDATA[Hello, world!]]></content>
  ```

  可以写成：

  ```java
  @JacksonXmlCData
  public String content;
  ```

- list处理：

  - useWrapping：用于指定是否使用包裹节点，默认为true

    ```java
    @JacksonXmlElementWrapper(useWrapping = false)
    public List<Sender> list;
    ```

    ```xml
    <Sender id="1" />
    <Sender id="2" />
    <Sender id="3" />
    ```

#### 从xml读取成java对象

xml例子：

```xml
<root>
    <function>
        <name>name1</name>
        <ddl>
          aaaaaaaaaaaaaaaaa
        </ddl>
    </function>
    <function>
        <name>name2</name>
        <ddl>
          aaaaaaaaaaaaaaaaa
        </ddl>
    </function>
</root>
```

`ReadXmlEntry.java:`

```java
public class ReadXmlEntry {
    public static class Function {

        private String ddl;

        public String getDdl() {
            return ddl;
        }

        public void setDdl(String ddl) {
            this.ddl = ddl;
        }
    }

    public static class Triggers {
        @JacksonXmlElementWrapper(useWrapping = false)
        @JacksonXmlProperty(localName = "function")
        private List<Function> function;

        public List<Function> getFunction() {
            return function;
        }

        public void setFunction(List<Function> function) {
            this.function = function;
        }
    }
}
```

从xml转成java对象：

```java
XmlMapper xmlMapper = new XmlMapper();
ReadXmlEntry.Root readXml = xmlMapper.readValue(in, ReadXmlEntry.Root.class);
```

#### Jackson相关参考链接：

（Jackson Annotation）

使用jackson把java对象转成xml格式

https://blog.csdn.net/qq_20161461/article/details/111664807

Jackson之注解大全

https://blog.csdn.net/blwinner/article/details/98532847

Jackson Annotation Examples

https://www.baeldung.com/jackson-annotations

More Jackson Annotations

https://www.baeldung.com/jackson-advanced-annotations



## 读取文件执行sql

### BufferedReader

```java
private BufferedReader openTenantDdlFiles(String ddl) throws UnsupportedEncodingException {
    InputStream in = readTenantDdlFiles(ddl);
    if (in != null) {
        return new BufferedReader(new InputStreamReader(in, "UTF-8"));
    }
    
    return null;
}
```

```java
private InputStream readTenantDdlFiles(String ddl) {
    StringBuilder sb = new StringBuilder();
    sb.append(ddlPackage.replaceAll("\\.", "/"));
    sb.append("/").append(ddl);

    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
    return classLoader.getResourceAsStream(sb.toString());
}
```

```java
BufferedReader reader = openTenantDdlFiles(ddl)
reader.read();
```

