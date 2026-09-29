<!-- pre-align:aligned sig=ea6cb95d87e9 -->

<a id="application-service-role-sdk"></a>
## Application Service > ROLE > SDK User Guide { #application-service-role-sdk }


> To check permissions using the ROLE service,
> You must call the RESTful API or use the Client SDK.
> If you're using Spring Framework, you can use the Java Client SDK more conveniently.

<a id="section-1"></a>
## Authentication and Authorization { #section-1 }

AppKey and SecretKey are required to use the ROLE SDK.
The Appkey is included in the request URL when calling the API to identify and point to a specific resource, and a SecretKey is a private key used to control access to the API.
For more information on checking and using Appkeys and SecretKeys, please refer to the [Appkey](/nhncloud/en/public-api/appkey/).
Alternatively, a Project-integrated Appkey can be used in place of Appkey. For more information about Project-integrated Appkey, see [Project-integrated Appkey](/nhncloud/en/public-api/project-integrated-appkey/).


<a id="spring-client-sdk"></a>
## Spring Client SDK { #spring-client-sdk }

<a id="spring-client-sdk-2"></a>
### What Is Spring Client SDK? { #spring-client-sdk-2 }

In an MVC project using Spring Framework
Provides @Annotation and Interceptor to make it easier to use the JAVA Client SDK.
To control access to the RESTful API, you can check access permissions by using the Spring Client SDK.
The `@Annotation` provided by the Spring Client SDK is used together with `@RequestMapping`,
The `value` of `@RequestMapping` is mapped to the Resource Path, and the `method` is mapped to the Operation ID, respectively.
Depending on the @Annotation configuration, you can map User ID and Scope ID to specific values in Path Variable, Query Parameter, and Header.


<a id="maven-java-client-sdk-for-spring"></a>
### Use JAVA Client SDK For Spring with Maven { #maven-java-client-sdk-for-spring }

To use the JAVA Client SDK For Spring, you need to configure the Maven repository and dependencies in pom.xml.

**[Maven Repository]**

```xml
<repositories>
	<repository>
		<id>com.toast.cloud</id>
		<name>TOAST Cloud Repository</name>
		<url>http://nexus.nhnent.com/content/repositories/releases</url>
	</repository>
</repositories>
```

**[Maven Dependency]**

```xml
<dependencies>
	<dependency>
		<groupId>com.toast.cloud</groupId>
		<artifactId>role-client-spring</artifactId>
		<version>1.4.1</version>
	</dependency>
</dependencies>
```

<a id="spring-configuration"></a>
### Spring Configuration { #spring-configuration }

Register TCRoleClientFactory in [applicationContext.xml].

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	<bean id="client" class="com.toast.cloud.tcrole.sdk.TCRoleClientFactory"
		factory-method="getClient">
		<!-- TOASTCloud console 에서 발급 받은 앱키 -->
		<constructor-arg name="appKey" value="CIxy8T4QdkxoH5wh" />
		<!-- TOASTCloud console 에서 발급 받은 비밀 키 -->
		<constructor-arg name="secretKey" value="bX67pDaw" />
	</bean>
</beans>
```

Register TCRoleControllerInterceptor in [mvc-config.xml].

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="
		http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans-3.1.xsd">
	<mvc:interceptors>
		<bean class="com.toast.cloud.tcrole.sdk.TCRoleControllerInterceptor" />
	</mvc:interceptors>
</beans>
```

<a id="annotation"></a>
### Permission Check with @Annotation { #annotation }

To check the permission for @RequestMapping in a Spring MVC project, use @Authorization as shown in the following example.
If the authority check fails, an InvalidAuthInfoException or UnauthorizedException is thrown.
You must register the ExceptionHandler of Spring Framework's @ControllerAdvice to implement appropriate error checking.

```java
@Controller
public Example {
    /**
     * Path         : '/secure'
     * User ID      : value of userId
     * Operation ID : 'GET'
     */
    @Authorization(userId = @AuthParam(type = AuthParamType.QUERY_PARAM, value = "userId"))
    @RequestMapping(value = "/secure", method = RequestMethod.GET)
    public String printSecureInformation(@RequestParam("userId") String userId) {
          return "This is a secure information. !!";
    }
}
```

|Annotation|	Parent Annotation|	Key|	Value|	Description|	Required|
|---|---|---|---|---|---|
|@Authorization|	없음|	userId|	@AuthParam|	권한 체크를 할 User ID를 정의한다.|	Yes|
|-|-|scopeId|	@AuthParam|	Defines the Scope ID for the authority check. Default ALL if omitted.|	No|
|@AuthParam|	@Authorization|	type|	AuthParamType|	Defines the type of the parameter.|	Yes|
|-|-|value|	String|	Defines the value of the parameter type.|	No|

|Enum|	Value|	Description|
|---|---|---|
|AuthParamType|	AuthParamType.STATIC|	Uses the value of @AuthParam directly.|
|-|AuthParamType.PATH_VARIABLE|	Uses the value of @AuthParam as the key of the path variable to retrieve the value.|
|-|AuthParamType.HEADER_PARAM|	Retrieves the value by using the value of @AuthParam as the Header key.|
|-|AuthParamType.QUERY_PARAM|	Retrieves the value by using the value of @AuthParam as the key of the Query Parameter.|

<a id="client-sdk"></a>
## Client SDK { #client-sdk }

<a id="client-sdk-2"></a>
### What Is the Client SDK? { #client-sdk-2 }

This is a Client SDK dedicated to Role for easy calls to the RESTful API.
Because it has a built-in cache feature, you can use the Role service more efficiently.
Currently, only the Java language is supported.

<a id="maven-java-client-sdk"></a>
### Use the Java Client SDK with Maven { #maven-java-client-sdk }

To use the Java Client SDK, you need to configure the Maven repository and dependency in pom.xml.

**[Maven Repository]**

```xml
<repositories>
	<repository>
		<id>com.toast.cloud</id>
		<name>TOAST Cloud Repository</name>
		<url>http://nexus.nhnent.com/content/repositories/releases</url>
	</repository>
</repositories>
```

**[Maven Dependency]**

```xml
<dependencies>
	<dependency>
		<groupId>com.toast.cloud</groupId>
		<artifactId>role-client</artifactId>
		<version>1.4.1</version>
	</dependency>
</dependencies>
```

<a id="java-client-sdk"></a>
### How to Use the Java Client SDK { #java-client-sdk }

To use the Java Client SDK, you must first use a TCRoleClientFactory object to create an instance of a TCRoleClient object.
Once you have created a TCRoleClient object, you can call the methods provided by that object to handle various tasks.

```java
// TCRoleClient 객체를 생성하는 올바른 방법
TCRoleClient client = TCRoleClientFactory.getClient("TEST_APPKEY", "TEST_SECRETKEY");

// 아래처럼 직접 생성자를 호출하면 안된다.
TCRoleClient client = new TCRoleClient("TEST_APPKEY", "TEST_SECRETKEY");
```

> Be careful not to call the constructor of TCRoleClient directly.

<a id="client-sdk-cache"></a>
### Client SDK Cache { #client-sdk-cache }

The Client SDK uses client-side cache for each of the following three cases.

- Permission check using Resource ID
- Resource Path to check permissions
- Get Resource Hierarchy

The cache is managed using LRU, with a default TTL (Time to Live) of 300 seconds and a default size of 1,000,000.
To change the value, access the [CONSOLE] and make the necessary changes.
Settings changed in [CONSOLE] take effect immediately, and all existing cache is deleted as soon as the changes are applied.

![[Figure 2] Client SDK Cache Settings](http://static.toastoven.net/prod_role/role_61.png)
<center>[Figure 2] Client SDK Cache Settings</center>

<a id="transaction"></a>
### Transaction Support { #transaction }

To atomically add, change, or delete ROLE data, call `beginTransaction()` on the `TCRoleClient` object to obtain a `TCRole Session` object and use it.

For example, if you register multiple roles at the same time as shown below, an error occurring in the middle may result in some roles being registered and others not.

```java
TCRoleClient client = TCRoleClientFactory.getClient("TEST_APPKEY", "TEST_SECRETKEY");

try {
	UserID userId = UserID.valueOf("U1");
	client.createUser(userId, "Example User 1");

	client.assignRoleToUser(userId, ScopeID.valueOf("ALL"), RoleID.valueOf("R1"));
	// 만약 여기서 Exception 이 발생한다면
	// U1 은 R1 권한만 가지고, R2 권한은 부여되지 않는다.
	client.assignRoleToUser(userId, ScopeID.valueOf("ALL"), RoleID.valueOf("R2"));
} catch (Exception e) {
	// 에러시 자체 Rollback 로직을 구현해야 한다.
	client.removeUser(userId);
}
```

If you use the TCRoleSession object, you can eliminate partial failures in the circumstances above.

```java
TCRoleClient client = TCRoleClientFactory.getClient("TEST_APPKEY", "TEST_SECRETKEY");
TCRoleSession session = client.beginTransaction();

try {
	UserID userId = UserID.valueOf("U1");
	session.createUser(userId, "Example User 1");

	session.assignRoleToUser(userId, ScopeID.valueOf("ALL"), RoleID.valueOf("R1"));
	// 만약 여기서 Exception 이 발생한다 하여도, 부분 실패는 발생하지 않는다.
	session.assignRoleToUser(userId, ScopeID.valueOf("ALL"), RoleID.valueOf("R2"));

	// 에러가 발생하지 않았다면, 서버에 변경사항을 반영한다.
	session.commit();
} catch (Exception e) {
	// 에러 시, rollback 한다.
	session.rollback();
}
```

When using the TCRoleSession object, no additions, modifications, or changes are reflected on the server until the `commit()` method is called. Be careful not to read data that has been changed before calling `commit()`.

After calling `commit()` or `rollback()` on a `TCRoleSession` object, you can reuse it.
