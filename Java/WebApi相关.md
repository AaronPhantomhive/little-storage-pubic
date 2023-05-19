# WebApi开发相关

### Jersey

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

### Jackson注解
（Jackson Annotation）
https://blog.csdn.net/blwinner/article/details/98532847

https://www.baeldung.com/jackson-annotations

https://www.baeldung.com/jackson-advanced-annotations

