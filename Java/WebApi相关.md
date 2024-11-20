# WebApi开发相关

## Jersey

https://eclipse-ee4j.github.io/jersey.github.io/documentation/latest/index.html



### 创建Maven项目记录

①eclipse：tomcat目录下的webapps文件夹下没有项目文件：

改servers location，选择第二个(Use tomcat Installation) ，同时修改deploy path为webapps

https://blog.csdn.net/m0_68467925/article/details/137876938



②web.xml配置：

```xml
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">
    
    <display-name>customSetName</display-name>

	<servlet>
		<servlet-name>JerseyServlet</servlet-name>
		<servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>
		<init-param>
			<param-name>jersey.config.server.provider.packages</param-name>
			<param-value>com.customSet.api</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>JerseyServlet</servlet-name>
		<url-pattern>/api/*</url-pattern>
	</servlet-mapping>
	
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>

    <error-page>
        <error-code>400</error-code>
        <location>/err/error.jsp</location>
    </error-page>
    <error-page>
        <error-code>401</error-code>
        <location>/err/error.jsp</location>
    </error-page>
    <error-page>
        <error-code>403</error-code>
        <location>/err/error.jsp</location>
    </error-page>
    <error-page>
        <error-code>404</error-code>
        <location>/err/error.jsp</location>
    </error-page>
    <error-page>
        <error-code>500</error-code>
        <location>/err/error.jsp</location>
    </error-page>

    <welcome-file-list>
		<welcome-file>./index.html</welcome-file>	
    </welcome-file-list>
</web-app>
```



③pom.xml配置：	

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <groupId>com.custom</groupId>
  <artifactId>custom-web-project</artifactId>
  <version>1.0.0</version>
  <packaging>pom</packaging>
  
  <name>custom</name>
  
  <build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>	
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
				<encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
  </build>
  <dependencies>
    <dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-lang3</artifactId>
		<version>3.9</version>
	</dependency>
	<dependency>
		<groupId>commons-io</groupId>
		<artifactId>commons-io</artifactId>
		<version>2.6</version>
	</dependency>
	<dependency>
		<groupId>com.google.code.gson</groupId>
		<artifactId>gson</artifactId>
		<version>2.2.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.hk2</groupId>
		<artifactId>hk2-api</artifactId>
		<version>2.6.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.hk2</groupId>
		<artifactId>hk2-locator</artifactId>
		<version>2.6.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.hk2</groupId>
		<artifactId>hk2-utils</artifactId>
		<version>2.6.1</version>
	</dependency>
	<dependency>
		<groupId>org.apache.httpcomponents</groupId>
		<artifactId>httpclient</artifactId>
		<version>4.5.13</version>
	</dependency>
	<dependency>
		<groupId>com.fasterxml.jackson.jaxrs</groupId>
		<artifactId>jackson-jaxrs-json-provider</artifactId>
		<version>2.10.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.core</groupId>
		<artifactId>jersey-client</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.core</groupId>
		<artifactId>jersey-common</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.containers</groupId>
		<artifactId>jersey-container-servlet</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.containers</groupId>
		<artifactId>jersey-container-servlet-core</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.ext</groupId>
		<artifactId>jersey-entity-filtering</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.inject</groupId>
		<artifactId>jersey-hk2</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.media</groupId>
		<artifactId>jersey-media-json-jackson</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.media</groupId>
		<artifactId>jersey-media-multipart</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.glassfish.jersey.core</groupId>
		<artifactId>jersey-server</artifactId>
		<version>2.29.1</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-client</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-http</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-io</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-jmx</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-security</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-server</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-servlet</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-util</artifactId>
		<version>9.4.12.v20180830</version>
	</dependency>
	<dependency>
		<groupId>ch.qos.logback</groupId>
		<artifactId>logback-classic</artifactId>
		<version>1.2.3</version>
	</dependency>
	<dependency>
		<groupId>ch.qos.logback</groupId>
		<artifactId>logback-core</artifactId>
		<version>1.2.3</version>
	</dependency>
	<dependency>
		<groupId>org.postgresql</groupId>
		<artifactId>postgresql</artifactId>
		<version>42.7.3</version>
	</dependency>
	<dependency>
		<groupId>org.apache.tomcat</groupId>
		<artifactId>tomcat-jdbc</artifactId>
		<version>8.5.97</version>
	</dependency>
	<dependency>
		<groupId>org.apache.tomcat</groupId>
		<artifactId>tomcat-juli</artifactId>
		<version>8.5.97</version>
	</dependency>
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>javax.servlet-api</artifactId>
		<version>4.0.0</version>
	</dependency>
	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-api</artifactId>
		<version>1.7.25</version>
	</dependency>
	<dependency>
		<groupId>software.amazon.awssdk</groupId>
		<artifactId>s3</artifactId>
		<version>2.25.2</version>
	</dependency>
  </dependencies>
</project>
```



④清理与重新构建项目

mvn clean package

mvn clean compile

mvn clean install

然后检查 `target/classes` 下是否生成了 `META-INF` 目录。



### 注解

`@DefaultValue("")`

写在bean的变量上，default value

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

