<!-- pre-align:aligned sig=ea6cb95d87e9 -->

<a id="application-service-role-sdk"></a>
## Application Service > ROLE > SDK 使用ガイド { #application-service-role-sdk }


> Role 商品を利用して権限をチェックするためには
> RESTful API を呼び出すか、Client SDK を使用する必要があります。
> Spring Framework を使用する場合、より簡単に JAVA Client SDK を使用できます。

<a id="section-1"></a>
## 認証および権限 { #section-1 }

ROLE SDK を使用するには、Appkey と SecretKey が必要です。
AppkeyはAPI呼び出し時にリクエストURLに含めて特定のリソースを指定および識別するために使用され、SecretKeyはAPIへのアクセスを制御する秘密鍵です。
Appkey および SecretKey の確認と使用方法の詳細については、[Appkey](/nhncloud/ja/public-api/appkey/) を参照してください。
Appkey の代わりに、プロジェクト統合 Appkey を使用することもできます。プロジェクト統合 Appkey の詳細については、[プロジェクト統合 Appkey](/nhncloud/ja/public-api/project-integrated-appkey/) を参照してください。


<a id="spring-client-sdk"></a>
## Spring Client SDK { #spring-client-sdk }

<a id="spring-client-sdk-2"></a>
### Spring Client SDK とは { #spring-client-sdk-2 }

Spring Framework を使用した MVC プロジェクトで
JAVA Client SDK をより便利に使用するための @Annotation および Interceptor を提供します。
RESTful API に対するアクセス制御を行う場合、Spring Client SDK を使用することで、容易に権限を確認できます。
Spring Client SDK が提供する @Annotation と @RequestMapping を併用します。
@RequestMapping の value が Resource Path、method が Operation ID にそれぞれマッピングされ、
@Annotation の設定に応じて、User ID と Scope ID を Path Variable、Query Parameter、Header の特定の値にマッピングできます。


<a id="maven-java-client-sdk-for-spring"></a>
### Maven を使用した JAVA Client SDK For Spring の利用 { #maven-java-client-sdk-for-spring }

JAVA Client SDK For Spring を使用するには、pom.xml に maven repository および dependency の設定が必要です。

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

[applicationContext.xml] に TCRoleClientFactory を登録します。

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

[mvc-config.xml] に TCRoleControllerInterceptor を登録します。

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
### @Annotation を利用した権限チェック { #annotation }

Spring MVC プロジェクトの @RequestMapping の権限をチェックするには、以下の例のように @Authorization を使用します。
権限チェックに失敗した場合、InvalidAuthInfoException または UnauthorizedException がスローされます。
Spring Framework の @ControllerAdvice の ExceptionHandler を登録して、適切なエラーチェックを実装する必要があります。

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
|@Authorization|	なし|	userId|	@AuthParam|	権限チェックを行うユーザー ID を定義します。|	Yes|
|-|-|scopeId|	@AuthParam|	権限チェックを行う Scope ID を定義します。省略時のデフォルト値は ALL|	No|
|@AuthParam|	@Authorization|	type|	AuthParamType|	パラメータのタイプを定義します。|	Yes|
|-|-|value|	String|	パラメータのタイプの値を定義します。|	No|

|Enum|	Value|	Description|
|---|---|---|
|AuthParamType|	AuthParamType.STATIC|	@AuthParam の value を直接使用します。|
|-|AuthParamType.PATH_VARIABLE|	@AuthParam の value を Path Variable のキーとして使用して値を取得します。|
|-|AuthParamType.HEADER_PARAM|	@AuthParam の value を Header のキーとして使用して値を取得します。|
|-|AuthParamType.QUERY_PARAM|	@AuthParam の value を Query Parameter のキーとして使用して値を取得します。|

<a id="client-sdk"></a>
## Client SDK { #client-sdk }

<a id="client-sdk-2"></a>
### Client SDK とは { #client-sdk-2 }

RESTful API を簡単に呼び出すための Role 専用クライアント SDK です。
独自のCache機能を備えているため、より効率的にRoleサービスを利用できます。
現在は Java 言語のみサポートしています。

<a id="maven-java-client-sdk"></a>
### Maven を使用した JAVA Client SDK の使用 { #maven-java-client-sdk }

JAVA Client SDK を使用するには、pom.xml に maven repository および dependency の設定が必要です。

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
### JAVA Client SDK の使用方法 { #java-client-sdk }

JAVA Client SDK を使用するには、まず TCRoleClientFactory オブジェクトを使用して TCRoleClient オブジェクトのインスタンスを生成する必要があります。
TCRoleClient オブジェクトを作成したら、そのオブジェクトが提供する method を呼び出して、さまざまな処理を実行します。

```java
// TCRoleClient 객체를 생성하는 올바른 방법
TCRoleClient client = TCRoleClientFactory.getClient("TEST_APPKEY", "TEST_SECRETKEY");

// 아래처럼 직접 생성자를 호출하면 안된다.
TCRoleClient client = new TCRoleClient("TEST_APPKEY", "TEST_SECRETKEY");
```

> TCRoleClient のコンストラクタを直接呼び出さないでください。

<a id="client-sdk-cache"></a>
### Client SDK Cache { #client-sdk-cache }

Client SDK では、以下の 3 つのケースそれぞれについて、クライアント側のキャッシュを使用します。

- Resource ID を利用した権限チェック
- Resource Path を利用した権限チェック
- Resource Hierarchy の照会

LRU で管理されており、Cache のデフォルト値は 300 秒の TTL (Time To Live) と 1,000,000 個のサイズです。
該当する値を変更するには、[CONSOLE] にアクセスして変更できます。
[CONSOLE] で変更した設定は変更直後に反映され、変更と同時に既存の Cache はすべて削除されます。

![[図 2] Client SDK Cache 設定](http://static.toastoven.net/prod_role/role_61.png)
<center>[図 2] Client SDK キャッシュ設定</center>

<a id="transaction"></a>
### Transaction サポート { #transaction }

ROLE のデータを Atomic に追加 / 変更 / 削除したい場合は、TCRoleClient オブジェクトの beginTransaction() を呼び出して TCRole Session オブジェクトを取得して使用します。

例えば、以下のように複数のRoleを同時に登録する場合、途中でエラーが発生すると、一部は登録され、一部は登録されない可能性があります。

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

TCRoleSession オブジェクトを使用すると、このような状況で部分的な失敗をなくすことができます。

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

TCRoleSession オブジェクトを使用する際、commit() メソッドを呼び出すまでは、いかなる追加・修正・変更もサーバーに反映されないため、commit() を実行する前に変更したデータを読み取らないよう注意してください。

TCRoleSession オブジェクトを commit() または rollback() した後、再度再利用できます。
