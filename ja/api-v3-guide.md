<!-- pre-align:aligned sig=812d7e85772e -->

<a id="application-service-role-api-v3-guide"></a>
## Application Service > ROLE > API v3ガイド { #application-service-role-api-v3-guide }

> ROLEサービスを利用して権限をチェックするためには
> RESTful APIを呼び出すか、クライアントSDKを利用する必要があります。

<a id="authentication-and-authorization"></a>
## 認証および権限 { #authentication-and-authorization }

ROLE APIを使用するには、AppkeyとSecretKeyが必要です。
Appkeyは、API呼び出し時にリクエストURLに含めて特定のリソースを指定し、識別するために使用されます。SecretKeyは、APIへのアクセスを制御するシークレットキーです。
Appkey及びSecretKeyの確認及び使用に関する詳細は、[Appkey](/nhncloud/ja/public-api/appkey/)を参照してください。
Appkeyの代わりにプロジェクト統合Appkeyを使用することも可能です。プロジェクト統合Appkeyの作成及び使用に関する詳細は、[プロジェクト統合Appkey](/nhncloud/ja/public-api/project-integrated-appkey/)を参照してください。

<a id="restful-api-guide"></a>
## RESTful APIガイド { #restful-api-guide }

<a id="common-response-body"></a>
### Common Response Body { #common-response-body }

すべてのAPIリクエストに対してHTTPレスポンスコードは200でレスポンスします。
詳細なレスポンス結果はResponse Bodyのheader項目を参照してください。

```json
{
  "cache": {
    "cacheFlushTime": "",
    "size": 0,
    "sizeTree": 1,
    "ttl": 5,
    "sizeByPath": 6
  },
  "header": {
    "isSuccessful": true,
    "resultCode": 5,
    "resultMessage": "resultMessage"
  }
}
```

| Key                  |  Type    | Description                            |
|----------------------|----------|----------------------------------------|
| header               |  Object  | レスポンスヘッダ                                 |
| header.isSuccessful  |  boolean | 成否                                 |
| header.resultCode    |  int     | レスポンスコード。成功時は0、失敗時はエラーコードを返す           |
| header.resultMessage |  String  | レスポンスメッセージ。成功時は"SUCCESS"、失敗時はエラーメッセージを返す |
| cache                | Object   | キャッシュ                                    |
| cache.cacheFlushTime | String   | キャッシュ削除時間                              | 
| cache.size | int      | リソースIDベースの認証キャッシュサイズ                    |
| cache.sizeByPath | int      | リソースPathベースの認証キャッシュサイズ                  |
| cache.sizeTree | int      | リソースHierarchy照会キャッシュサイズ                |
| cache.ttl | int      | キャッシュデータ維持時間(秒単位)                     |

<a id="user"></a>
## ユーザー { #user }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users**](#create-a-user) | ユーザーの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#deleting-a-user) | ユーザーの削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users**](#delete-users) | ユーザーの一括削除 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/id**](#get-a-list-of-all-user-ids) | すべてのユーザーIDリストの照会 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#get-user-information) | ユーザー情報照会 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/histories**](#view-a-list-of-changes-to-roles-assigned-to-a-user) | ユーザーに割り当てられたロールの変更履歴リストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/search**](#get-a-list-of-users) | ユーザーリストの照会 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#edit-users) | ユーザーの修正 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}**](#edit-user-scopes) | ユーザースコープ限定修正 |


<a id="create-a-user"></a>
### **ユーザーの作成** { #create-a-user }
> POST "/role/v3.0/appkeys/{appKey}/users"

<a id="create-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateUserRequest** | **CreateUserRequest**| **Yes** |  | |





##### CreateUserRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **users** | **List&lt;CreateUserRequest.UserProtocol>**| **Yes** | ユーザーリスト |

##### CreateUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | ユーザーの説明 |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | ユーザー関連ロール |
|   **userId** | **String**| **Yes** | ユーザーID  |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | ロールID |
|   **scopeId** | **String**| **Yes** | スコープID    |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |



























<a id="create-a-user-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="deleting-a-user"></a>
### **ユーザーの削除** { #deleting-a-user }
> DELETE "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="deleting-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**userId** | **String**| **Yes** | ユーザーID | 









<a id="deleting-a-user-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-users"></a>
### **ユーザーの一括削除** { #delete-users }
> DELETE "/role/v3.0/appkeys/{appKey}/users"

<a id="delete-users-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**userIds** |  **List&lt;String>**| **Yes** | ユーザーIDリスト |


<a id="delete-users-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="get-a-list-of-all-user-ids"></a>
### **すべてのユーザーIDリストの照会** { #get-a-list-of-all-user-ids }
> POST "/role/v3.0/appkeys/{appKey}/users/id"

<a id="get-a-list-of-all-user-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | ユーザーの説明(部分一致)  |
|   **needRoleRelations** | **Boolean**| **No** | レスポンス時に関連ロール関係を含めるかどうか(デフォルト値: true)  |
|   **needRoleTags** | **Boolean**| **No** | レスポンス時にロール関連関係を含める場合、ロールタグを含めるかどうか(デフォルト値: false)  |
|   **needRoleCount** | **Boolean**| **No** | レスポンス時、ユーザーが持つロール数を含めるかどうか(デフォルト値: false)        |
|   **roleIdPreLike** | **String**| **No** | ロールID(前方一致)  |
|   **roleIds** | **List&lt;String>**| **No** | ロールIDリスト(完全一致)  |
|   **scopeIdPreLike** | **String**| **No** | スコープID(前方一致)  |
|   **scopeIds** | **List&lt;String>**| **No** | スコープIDリスト(完全一致)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | ユーザーID(前方一致)  |
|   **userIds** | **List&lt;String>**| **No** | ユーザーIDリスト(完全一致)  |






<a id="get-a-list-of-all-user-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "userIds" : [ "userIds", "userIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAllUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 全体数 |
|   **userIds** | **List&lt;String>**| **Yes** | ユーザーリスト |











<a id="get-user-information"></a>
### **ユーザー情報照会** { #get-user-information }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="get-user-information-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**userId** | **String**| **Yes** | ユーザーID | 
|  Query |**searchRoleOptionCode** | **String**| **No** | アクセス可能なロールリスト検索方式 | [optional] [default to null] [enum: DIRECT_ROLE, INDIRECT_ROLE] |
|  Query |**roleIds** |  **List&lt;String>**| **No** | 関連関係ロールID |
|  Query |**scopeIds** |  **List&lt;String>**| **No** | 関連関係スコープID |










<a id="get-user-information-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "user" : {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId"
  }
}
```



##### GetUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **user** | **UserBundleProtocol**| **Yes** | ユーザー        |


##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 説明 |
|   **regYmdt** | **Date**| **No** | ユーザー作成日時 |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | ユーザーに割り当てられたロールリスト |
|   **userId** | **String**| **Yes** | ユーザーID  |



##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | ロール条件属性 |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regYmdt** | **Date**| **No** | 登録日時  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | ロールタグリスト |
|   **scopeId** | **String**| **Yes** | スコープID  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 条件属性                                                                                                                                                                                    |
|   **attributeId** | **String**| **Yes** | 条件属性ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値                                                                                                                                                                                  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | ロールタグID  |























<a id="view-a-list-of-changes-to-roles-assigned-to-a-user"></a>
### **ユーザーに割り当てられたロールの変更履歴リストの照会** { #view-a-list-of-changes-to-roles-assigned-to-a-user }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}/histories"

<a id="view-a-list-of-changes-to-roles-assigned-to-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**userId** | **String**| **Yes** | ユーザーID | 
|  Query |**roleId** | **String**| **No** | ロールID |
|  Query |**scopeId** | **String**| **No** | スコープID |
|  Query |**fromDateTime** | **Date**| **No** | 変更開始日時 |
|  Query |**toDateTime** | **Date**| **No** | 変更終了日時 |
|  Query |**historyType** |  **List&lt;String>**| **No** | 変更タイプ | [optional] [default to null] [enum: USER_ADD, USER_REMOVE, ADD, REMOVE] |
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`seq,DESC`)|















<a id="view-a-list-of-changes-to-roles-assigned-to-a-user-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "userHistory" : [ {
    "executionTime" : "2000-01-23T04:56:07.000+00:00",
    "scopeId" : "scopeId",
    "userHistorySeq" : 6,
    "roleId" : "roleId",
    "operatorUuid" : "operatorUuid",
    "conditions" : [ {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    }, {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    } ],
    "userId" : "userId"
  }, {
    "executionTime" : "2000-01-23T04:56:07.000+00:00",
    "scopeId" : "scopeId",
    "userHistorySeq" : 6,
    "roleId" : "roleId",
    "operatorUuid" : "operatorUuid",
    "conditions" : [ {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    }, {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    } ],
    "userId" : "userId"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetUserHistory.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 全体数 |
|   **userHistory** | **List&lt;UserHistoryProtocol>**| **Yes** | ユーザー変更履歴リスト |



##### UserHistoryProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **command** | **String**| **Yes** |   USER_ADD, USER_REMOVE, ADD, REMOVE |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | ロール条件属性 |
|   **executionTime** | **Date**| **Yes** | 変更日時 |
|   **operatorUuid** | **String**| **No** | 作業者UUID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |
|   **roleId** | **String**| **No** | ロールID  |
|   **scopeId** | **String**| **No** | スコープID  |
|   **userHistorySeq** | **Long**| **Yes** | ユーザー変更履歴シリアル番号 |
|   **userId** | **String**| **Yes** | ユーザーID  |


##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 条件属性                                                                                                                                                                                    |
|   **attributeId** | **String**| **Yes** | 条件属性ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値                                                                                                                                                                                  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |




<a id="get-a-list-of-users"></a>
### **ユーザーリストの照会** { #get-a-list-of-users }
> POST "/role/v3.0/appkeys/{appKey}/users/search"

<a id="get-a-list-of-users-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description                                  | 
|------------ | ------------- | ------------- |----------------------------------------------|
|   **descriptionLike** | **String**| **No** | ユーザーの説明(部分一致)                                |
|   **needRoleRelations** | **Boolean**| **No** | レスポンス時に関連ロール関係を含めるかどうか(デフォルト値: true)            |
|   **needRoleTags** | **Boolean**| **No** | レスポンス時にロール関連関係を含める場合、ロールタグを含めるかどうか(デフォルト値: false) |
|   **needRoleCount** | **Boolean**| **No** | レスポンス時にユーザーが持つロール数を含めるかどうか(デフォルト値: false)        |
|   **roleIdPreLike** | **String**| **No** | ロールID(前方一致)                                 |
|   **roleIds** | **List&lt;String>**| **No** | ロールIDリスト(完全一致)                              |
|   **scopeIdPreLike** | **String**| **No** | スコープID(前方一致)                                 |
|   **scopeIds** | **List&lt;String>**| **No** | スコープIDリスト(完全一致)                              |
|   **searchRoleOptionCode** | **String**| **No** | DIRECT_ROLE, INDIRECT_ROLE                   |
|   **userIdPreLike** | **String**| **No** | ユーザーID(前方一致)                                |
|   **userIds** | **List&lt;String>**| **No** | ユーザーIDリスト(完全一致)                             |








<a id="get-a-list-of-users-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "users" : [ {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId",
    "roleCounts": [
      {
        "roleCount": 2,
        "scopeId": "scopeId"
      }
    ]
  }, {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId",
    "roleCounts": [
      {
        "roleCount": 2,
        "scopeId": "scopeId"
      }
    ]
  } ]
}
```



##### SearchUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 全体数 |
|   **users** | **List&lt;UserBundleProtocol>**| **Yes** | ユーザーリスト |



##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- |----------| ------------ |
|   **description** | **String**| **No**   | 説明 |
|   **regYmdt** | **Date**| **No**   | ユーザー作成日時 |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No**   | ユーザーに割り当てられたロールリスト |
|   **userId** | **String**| **Yes**  | ユーザーID  |
|   **roleCounts** | **List&lt;UserRoleCountProtocol>**| **No**   | ユーザーに割り当てられたロール数 |


##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | ロール条件属性 |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regYmdt** | **Date**| **No** | 登録日時  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | ロールタグリスト |
|   **scopeId** | **String**| **Yes** | スコープID  |

##### UserBundleProtocol.UserRoleCountProtocol

| Name | Type | Required | Description | 
|------------ | ------------ | ------------- | ------------ |
|   **scopeId** | **String**| **Yes** | スコープID  |
|   **roleCount** | **Long**| **Yes** | スコープID別のロール数 |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 条件属性                                                                                                                                                                                    |
|   **attributeId** | **String**| **Yes** | 条件属性ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値                                                                                                                                                                                  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | ロールタグID  |























<a id="edit-users"></a>
### **ユーザーの修正** { #edit-users }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="edit-users-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵  |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー         | 
|  Path |**userId** | **String**| **Yes** | ユーザーID      | 
| Request Body | **PutUserRequest** | **PutUserRequest**| **Yes** | ユーザー        |






##### PutUserRequest


| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserRequest.UserProtocol**| **Yes** | ユーザー        |
| **createUserIfNotExist** | **Boolean** | **No** | リクエスト時に存在しない場合、ユーザーを作成するかどうか |

##### PutUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | ユーザーの説明 |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | ユーザー関連ロール |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性   |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | ロールID       |
|   **scopeId** | **String**| **Yes** | スコープID       |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |


























<a id="edit-users-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



<a id="edit-user-scopes"></a>
### **ユーザースコープ限定修正** { #edit-user-scopes }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}"

<a id="edit-user-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description |
|------------- |------------- | ------------- | ------------- |-------------|
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵  |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
|  Path |**userId** | **String**| **Yes** | ユーザーID |
|  Path |**scopeId** | **String**| **Yes** | スコープID |
| Request Body | **putUserScopeRequest** | **PutUserScopeRequest**| **Yes** | ユーザー |

##### PutUserScopeRequest

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserScopeRequest.UserProtocol**| **Yes** | ユーザー |
| **createUserIfNotExist** | **Boolean** | **No** | リクエスト時に存在しない場合、ユーザーを作成するかどうか |

##### PutUserScopeRequest.UserProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | ユーザーの説明 |
|   **roleRelations** | **List&lt;UserScopeRoleRelationProtocol>**| **No** | ユーザー関連ロール |


##### UserScopeRoleRelationProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | ロールID |

##### ConditionProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |

<a id="edit-user-scopes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="user-authentication"></a>
## ユーザー認証 { #user-authentication }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources**](#check-if-a-user-is-authorized-to-access-a-resource) | ユーザーがリソースにアクセス権限があるかどうか検査 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles**](#check-if-a-user-has-access-to-a-role) | ユーザーがロールにアクセスできる権限があるかどうか検査 |


<a id="check-if-a-user-is-authorized-to-access-a-resource"></a>
### **ユーザーがリソースのアクセス権限があるかどうか検査** { #check-if-a-user-is-authorized-to-access-a-resource }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources"

<a id="check-if-a-user-is-authorized-to-access-a-resource-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵  |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー         | 
|  Path |**userId** | **String**| **Yes** | ユーザーID      | 
| Request Body | **PostAuthorizationResource.Request** | **PostAuthorizationResource.Request**| **Yes** | リソースリスト     | |






##### PostAuthorizationResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **resources** | **List&lt;PostAuthorizationResource.ResourceProtocol>**| **Yes** | リソースリスト     |

##### PostAuthorizationResource.ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **No** | 条件属性ID  |
|   **authRequestId** | **String**| **No** | リクエスト認証識別キー  |
|   **operationId** | **String**| **Yes** | オペレーションID  |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resourcePath** | **String**| **No** | リソースPath  |
|   **scopeId** | **String**| **No** | スコープID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeValue** | **String**| **Yes** | 条件属性値 |























<a id="check-if-a-user-is-authorized-to-access-a-resource-response-body"></a>
#### Response Body

```json
{
  "authorizations" : [ {
    "resourceId" : "resourceId",
    "scopeId" : "scopeId",
    "resourcePath" : "resourcePath",
    "authRequestId" : "authRequestId",
    "operationId" : "operationId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  }, {
    "resourceId" : "resourceId",
    "scopeId" : "scopeId",
    "resourcePath" : "resourcePath",
    "authRequestId" : "authRequestId",
    "operationId" : "operationId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostAuthorizationResource.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;PostAuthorizationResource.AuthorizationWithResourceProtocol>**| **No** | 権限チェック結果リスト |

##### PostAuthorizationResource.AuthorizationWithResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **Yes** | 条件属性ID  |
|   **authRequestId** | **String**| **No** | リクエスト認証識別キー  |
|   **operationId** | **String**| **Yes** | オペレーションID  |
|   **permission** | **Boolean**| **Yes** | 権限の有無  |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resourcePath** | **String**| **No** | リソースPath  |
|   **scopeId** | **String**| **Yes** | スコープID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeValue** | **String**| **Yes** | 条件属性値 |

























<a id="check-if-a-user-has-access-to-a-role"></a>
### **ユーザーがロールにアクセス権限があるかどうかを検査** { #check-if-a-user-has-access-to-a-role }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles"

<a id="check-if-a-user-has-access-to-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**userId** | **String**| **Yes** | ユーザーID | 
| Request Body | **PostAuthorizationRole.Request** | **PostAuthorizationRole.Request**| **Yes** |  | |






##### PostAuthorizationRole.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List&lt;PostAuthorizationRole.AuthRoleProtocol>**| **Yes** | 認証リクエストリスト |

##### PostAuthorizationRole.AuthRoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **No** | 条件属性 |
|   **authRequestId** | **String**| **No** | 認証リクエスト識別キー  |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **scopeId** | **String**| **No** | スコープID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeValue** | **String**| **Yes** | 条件属性値 |





















<a id="check-if-a-user-has-access-to-a-role-response-body"></a>
#### Response Body

```json
{
  "authorizations" : [ {
    "scopeId" : "scopeId",
    "roleId" : "roleId",
    "authRequestId" : "authRequestId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  }, {
    "scopeId" : "scopeId",
    "roleId" : "roleId",
    "authRequestId" : "authRequestId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostAuthorizationRole.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;PostAuthorizationRole.AuthorizationProtocol>**| **No** | 権限チェック結果リスト |

##### PostAuthorizationRole.AuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **Yes** | 条件属性ID  |
|   **authRequestId** | **String**| **No** | リクエスト認証識別キー  |
|   **permission** | **Boolean**| **Yes** | 権限の有無  |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **scopeId** | **String**| **Yes** | スコープID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeValue** | **String**| **Yes** | 条件属性値 |






















<a id="roles"></a>
## ロール { #roles }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles**](#create-a-role) | ロールの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#deleting-roles) | ロールの削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles**](#delete-roles) | ロールの一括削除 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable**](#whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled) | ロール使用有無をDENY(未使用)に変更可能かどうか |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#single-role-lookup) | ロール単件照会 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/id**](#get-a-list-of-all-role-ids) | すべてのロールIDリストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search**](#get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role) | ロールで設定可能なすべての条件属性リストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/containing-roles/search**](#roles-1) | 특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/search**](#get-a-list-of-roles) | ロールリストの照会 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#modify-roles) | ロール修正 |


<a id="create-a-role"></a>
### **ロールの作成** { #create-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles"

<a id="create-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateRoleRequest** | **CreateRoleRequest**| **Yes** |  | |





##### CreateRoleRequest


| Name | Type | Required | Description      | 
|------------ | ------------- | ------------- |------------------|
|   **role** | **RoleProtocol**| **No** | ロール |
|   **roleRelations** | **List&lt;CreateRoleRequest.RoleRelationProtocol>**| **No** | 条件属性と関連するロールIDリスト |
|   **roleTags** | **List&lt;CreateRoleRequest.RoleTagProtocol>**| **No** | ロールタグリスト        |

##### RoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |










##### CreateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **relatedRoleId** | **String**| **Yes** | 条件属性と関連するロールID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |














##### CreateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | ロールタグID  |













<a id="create-a-role-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="deleting-roles"></a>
### **ロールの削除** { #deleting-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="deleting-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**roleId** | **String**| **Yes** | ロールID | 









<a id="deleting-roles-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-roles"></a>
### **ロールの一括削除** { #delete-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="delete-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**roleIds** |  **List&lt;String>**| **Yes** | ロールIDリスト |


<a id="delete-roles-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled"></a>
### **ロール使用有無をDENY(未使用)に変更可能かどうか** { #whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable"

<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**roleId** | **String**| **Yes** | ロールID | 









<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled-response-body"></a>
#### Response Body

```json
{
  "deniable" : true,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetDeniableResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **deniable** | **Boolean**| **No** | ロール使用有無をDENY(未使用)に変更可能かどうか |










<a id="single-role-lookup"></a>
### **ロール単件照会** { #single-role-lookup }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="single-role-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**roleId** | **String**| **Yes** | ロールID | 









<a id="single-role-lookup-response-body"></a>
#### Response Body

```json
{
  "role" : {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetRoleResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **role** | **RoleBundleProtocol**| **Yes** | ロール |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | アプリケーションキー |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 条件属性リスト |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regDateTime** | **Date**| **Yes** | ロール作成日時 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 関連関係ロールリスト |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | ロールタグリスト |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | ロール条件属性 |
|   **description** | **String**| **No** | ロールの説明 |
|   **regDateTime** | **Date**| **Yes** | ロール作成日時 |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 役割タグリスト |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 条件属性                                                                                                                                                                                    |
|   **attributeId** | **String**| **Yes** | 条件属性ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値                                                                                                                                                                                  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | ロールタグID  |

















<a id="get-a-list-of-all-role-ids"></a>
### **すべてのロールIDリストの照会** { #get-a-list-of-all-role-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/id"

<a id="get-a-list-of-all-role-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**roleIdPreLike** | **String**| **No** | ロールID(前方一致) |
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.roleId,ASC`)|











<a id="get-a-list-of-all-role-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "roleIds" : [ "roleIds", "roleIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllRoleIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIds** | **List&lt;String>**| **Yes** | ロールIDリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |











<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **ロールで設定可能なすべての条件属性リストの照会** { #get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search"

<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description                                                | 
|------------- |------------- | ------------- | ------------- |------------------------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵                                                 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー                                                        | 
|  Path |**roleId** | **String**| **Yes** | ロールID                                                      | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1)                                      | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10)                                 |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`attributeCreationTypeCode,ASC&quot;,&quot;id.attributeId,ASC`)|
| Request Body | **SearchRoleAttributes.Request** | **SearchRoleAttributes.Request**| **Yes** |                                                            | |






##### SearchRoleAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeNameLike** | **String**| **No** | 条件属性名(部分一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |













<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchRoleAttributes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | ロールに付与可能な条件属性リスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 条件属性の説明 |



















<a id="roles-1"></a>
### **특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록 조회** { #roles-1 }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/containing-roles/search"

기준이 되는 역할(`{roleId}`)의 직접 하위 역할 목록을 모두 포함하는 상위 호환 역할 ID 목록을 조회합니다.

<a id="roles-1-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 기준이 되는 역할 ID |
| Request Body | **SearchContainingRoles.Request** | **SearchContainingRoles.Request**| **Yes** |  | |



##### SearchContainingRoles.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagIds** | **List&lt;String>**| **No** | 역할 태그 ID 목록(OR 조건)  |
|   **roleGroups** | **List&lt;String>**| **No** | 역할 그룹 목록(OR 조건)  |


<a id="roles-1-request"></a>
#### Request 예시

```json
{
  "roleTagIds" : [ "TAG_A", "TAG_B" ],
  "roleGroups" : [ "GROUP_1" ]
}
```


<a id="roles-1-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "SUCCESS"
  },
  "totalItems" : 3,
  "roleIds" : [ "ROLE_ADMIN", "ROLE_SUPER", "ROLE_MANAGER" ]
}
```



##### SearchContainingRoles.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIds** | **List&lt;String>**| **Yes** | 상위 호환 역할 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |










<a id="get-a-list-of-roles"></a>
### **ロールリストの照会** { #get-a-list-of-roles }
> POST "/role/v3.0/appkeys/{appKey}/roles/search"

<a id="get-a-list-of-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description                                   | 
|------------- |------------- | ------------- | ------------- |-----------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵                                    |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー                                           | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1)                         | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10)                    |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`exposureOrder,ASC&quot;,&quot;id.roleId,ASC`)|
| Request Body | **GetRoles.Request** | **GetRoles.Request**| **Yes** |                                               | |





##### GetRoles.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |
|   **descriptionLike** | **String**| **No** | ロールの説明(部分一致)  |
|   **needAttributes** | **Boolean**| **No** | レスポンス時に条件属性情報を含めるかどうか  |
|   **needRoleRelations** | **Boolean**| **No** | レスポンス時にロール関連関係IDリストを含めるかどうか  |
|   **needRoleTags** | **Boolean**| **No** | レスポンス時にロールタグIDリストを含めるかどうか  |
|   **relatedRoleIds** | **List&lt;String>**| **No** | 関連関係ロールIDリスト(完全一致)  |
|   **roleGroup** | **String**| **No** | ロールグループ(完全一致)  |
|   **roleGroupLike** | **String**| **No** | ロールグループ(部分一致)  |
|   **roleIdPreLike** | **String**| **No** | ロールID(前方一致)  |
|   **roleIds** | **List&lt;String>**| **No** | ロールIDリスト(完全一致)  |
|   **roleNameLike** | **String**| **No** | ロール名(部分一致)  |
|   **roleTagIdExpr** | **String**| **No** | ロールタグ条件(セパレータ&#39;;&#39;:OR, &#39;,&#39;:AND)  |
|   **roleTagIds** | **List&lt;String>**| **No** | ロールタグIDリスト(完全一致)  |























<a id="get-a-list-of-roles-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 6,
  "roles" : [ {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  }, {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetRoles.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List&lt;RoleBundleProtocol>**| **Yes** | ロールリスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | アプリケーションキー |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 条件属性リスト |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regDateTime** | **Date**| **Yes** | ロール作成日時 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 関連関係ロールリスト |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | ロールタグリスト |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | ロール条件属性 |
|   **description** | **String**| **No** | ロールの説明 |
|   **regDateTime** | **Date**| **Yes** | ロール作成日時 |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 役割タグリスト |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 条件属性                                                                                                                                                                                    |
|   **attributeId** | **String**| **Yes** | 条件属性ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値                                                                                                                                                                                  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | ロールタグID  |


















<a id="modify-roles"></a>
### **ロールの修正** { #modify-roles }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="modify-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**roleId** | **String**| **Yes** | ロールID | 
| Request Body | **UpdateRoleRequest** | **UpdateRoleRequest**| **Yes** |  | |






##### UpdateRoleRequest


| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **role** | **RoleMetadataProtocol**| **No** | ロール                 |
|   **roleRelations** | **List&lt;UpdateRoleRequest.RoleRelationProtocol>**| **No** | 条件属性と関連するロールIDリスト |
|   **roleTags** | **List&lt;UpdateRoleRequest.RoleTagProtocol>**| **No** | ロールタグリスト           |

##### RoleMetadataProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleName** | **String**| **No** | ロール名 |









##### UpdateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **relatedRoleId** | **String**| **Yes** | 条件属性と関連するロールID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |














##### UpdateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | ロールタグID  |













<a id="modify-roles-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="role-tags"></a>
## ロールタグ { #role-tags }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/tags/id**](#get-a-list-of-all-role-tag-ids) | すべてのロールタグIDリストの照会 |


<a id="get-a-list-of-all-role-tag-ids"></a>
### **すべてのロールタグIDリストの照会** { #get-a-list-of-all-role-tag-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/tags/id"

<a id="get-a-list-of-all-role-tag-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**roleTagIdPreLike** | **String**| **No** | ロールタグID(前方一致) |
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.roleTagId,ASC`)|











<a id="get-a-list-of-all-role-tag-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "roleTagIds" : [ "roleTagIds", "roleTagIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllRoleTagIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagIds** | **List&lt;String>**| **No** | ロールタグIDリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |




<a id="role-related-relations"></a>
## ロール関連関係 { #role-related-relations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#create-role-related-relations) | ロール関連関係の一括作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#delete-role-realated-relations) | ロール関連関係の一括削除 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#edit-role-related-relations) | ロール関連関係の一括修正 |

<a id="create-role-related-relations"></a>
### **ロール関連関係の一括作成** { #create-role-related-relations }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="create-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
|  Path |**roleId** | **String**| **Yes** | ロールID |
| Request Body | **CreateRoleRelationRequest** | **CreateRoleRelationRequest**| **Yes** |  | |

##### CreateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | ロール関連関係リスト |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **relatedRoleId** | **String**| **Yes** | 条件属性と関連するロールID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |


<a id="create-role-related-relations-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="delete-role-realated-relations"></a>
### **ロール関連関係の一括削除** { #delete-role-realated-relations }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="delete-role-realated-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
|  Path |**roleId** | **String**| **Yes** | ロールID |
| Request Body | **DeleteRoleRelationRequest** | **DeleteRoleRelationRequest**| **Yes** |  | |

##### DeleteRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **relatedRoleIds** | **List&lt;String>**| **Yes** | 関連関係ロールIDリスト |


<a id="delete-role-realated-relations-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="edit-role-related-relations"></a>
### **ロール関連関係の一括修正** { #edit-role-related-relations }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="edit-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
|  Path |**roleId** | **String**| **Yes** | ロールID |
| Request Body | **UpdateRoleRelationRequest** | **UpdateRoleRelationRequest**| **Yes** |  | |

##### UpdateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | ロール関連関係リスト |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | ロール条件属性 |
|   **relatedRoleId** | **String**| **Yes** | 条件属性と関連するロールID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |


<a id="edit-role-related-relations-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



<a id="scope"></a>
## スコープ { #scope }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](#create-a-scope) | スコープの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#delete-a-scope) | スコープの削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes**](#delete-scopes) | スコープの一括削除 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](#get-a-list-of-all-scope-ids) | すべてのスコープIDリストの照会 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#get-a-single-scope) | スコープ単件照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](#get-a-list-of-scopes) | スコープリストの照会 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#modify-scope) | スコープ修正 |


<a id="create-a-scope"></a>
### **スコープ作成** { #create-a-scope }
> POST "/role/v3.0/appkeys/{appKey}/scopes"

<a id="create-a-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateScope.Request** | **CreateScope.Request**| **Yes** |  | |





##### CreateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | スコープ説明 |
|   **scopeId** | **String**| **Yes** | スコープID  |










<a id="create-a-scope-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-a-scope"></a>
### **スコープ削除** { #delete-a-scope }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="delete-a-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**scopeId** | **String**| **Yes** | スコープID | 









<a id="delete-a-scope-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-scopes"></a>
### **スコープの一括削除** { #delete-scopes }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes"

<a id="delete-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**scopeIds** |  **List&lt;String>**| **Yes** | スコープIDリスト |


<a id="delete-scopes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="get-a-list-of-all-scope-ids"></a>
### **すべてのスコープIDリストの照会** { #get-a-list-of-all-scope-ids }
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"

<a id="get-a-list-of-all-scope-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**scopeIdPreLike** | **String**| **No** | スコープID(前方一致) |
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.scopeId,ASC`)|











<a id="get-a-list-of-all-scope-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "scopeIds" : [ "scopeIds", "scopeIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllScopeIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **scopeIds** | **List&lt;String>**| **No** | スコープIDリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |











<a id="get-a-single-scope"></a>
### **スコープ単件照会** { #get-a-single-scope }
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="get-a-single-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**scopeId** | **String**| **Yes** | スコープID | 









<a id="get-a-single-scope-response-body"></a>
#### Response Body

```json
{
  "scope" : {
    "scopeId" : "scopeId",
    "description" : "description"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetScope.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **scope** | **ScopeProtocol**| **No** | スコープ         |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | スコープ説明 |
|   **scopeId** | **String**| **Yes** | スコープID  |














<a id="get-a-list-of-scopes"></a>
### **スコープリストの照会** { #get-a-list-of-scopes }
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"

<a id="get-a-list-of-scopes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.scopeId,ASC`)|
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**| **Yes** |  | |





##### PostSearchScopes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | スコープの説明(部分一致)  |
|   **scopeIdPreLike** | **String**| **No** | スコープID(前方一致)  |
|   **scopeIds** | **List&lt;String>**| **No** | スコープIDリスト(完全一致)  |













<a id="get-a-list-of-scopes-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "scopes" : [ {
    "scopeId" : "scopeId",
    "description" : "description"
  }, {
    "scopeId" : "scopeId",
    "description" : "description"
  } ]
}
```



##### PostSearchScopes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **scopes** | **List&lt;ScopeProtocol>**| **No** | スコープリスト |
|   **totalItems** | **Long**| **No** | スコープの総数 |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | スコープ説明 |
|   **scopeId** | **String**| **Yes** | スコープID  |















<a id="modify-scope"></a>
### **スコープの修正** { #modify-scope }
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="modify-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**scopeId** | **String**| **Yes** | スコープID | 
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**| **Yes** |  | |






##### UpdateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 説明 |









<a id="modify-scope-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="resource"></a>
## リソース { #resource }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources**](#create-resources) | リソースの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#delete-resource) | リソースの削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources**](#delete-resources) | リソースの一括削除 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#single-resource-lookup) | リソース単件照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/id**](#get-a-list-of-resource-ids) | リソースIDリストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/attributes/search**](#resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role) | リソースで設定可能なすべての条件属性リストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/search**](#get-a-list-of-resources) | リソースリストの照会 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#modify-resources) | リソースの修正 |


<a id="create-resources"></a>
### **リソースの作成** { #create-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources"

<a id="create-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateResource.Request** | **CreateResource.Request**| **Yes** |  | |





##### CreateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |















<a id="create-resources-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-resource"></a>
### **リソースの削除** { #delete-resource }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="delete-resource-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 









<a id="delete-resource-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-resources"></a>
### **リソースの一括削除** { #delete-resources }
> DELETE "/role/v3.0/appkeys/{appKey}/resources"

<a id="delete-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**resourceIds** |  **List&lt;String>**| **Yes** | リソースIDリスト |


<a id="delete-resources-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="single-resource-lookup"></a>
### **リソース単件照会** { #single-resource-lookup }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="single-resource-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 









<a id="single-resource-lookup-response-body"></a>
#### Response Body

```json
{
  "resource" : {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetResource.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **resource** | **ResourceProtocol**| **No** | リソース        |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |



















<a id="get-a-list-of-resource-ids"></a>
### **リソースIDリストの照会** { #get-a-list-of-resource-ids }
> POST "/role/v3.0/appkeys/{appKey}/resources/id"

<a id="get-a-list-of-resource-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.resourceId,ASC`)|
| Request Body | **GetAllResourceIds.Request** | **GetAllResourceIds.Request**| **Yes** |  | |





##### GetAllResourceIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operationIds** | **List&lt;String>**| **No** | リソースID(前方一致)      |
|   **resourceIdPreLike** | **String**| **No** | リソースにアクセス可能なユーザーID      |
|   **roleIds** | **List&lt;String>**| **No** | リソースに付与されたロールID      |
|   **userIds** | **List&lt;String>**| **No** | リソースに付与されたOperation ID      |














<a id="get-a-list-of-resource-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resourceIds" : [ "resourceIds", "resourceIds" ]
}
```



##### GetAllResourceIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resourceIds** | **List&lt;String>**| **Yes** | リソースIDリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |











<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **リソースで設定可能なすべての条件属性リストの照会** { #resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/resources/attributes/search"

<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.attributeId,ASC`)|
| Request Body | **SearchResourceAttributes.Request** | **SearchResourceAttributes.Request**| **Yes** |  | |





##### SearchResourceAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | オペレーションID  |
|   **resourceId** | **String**| **No** | リソースID。IDとPathが両方ある場合、ID基準でのみ提供 |
|   **resourcePath** | **String**| **No** | リソースPath  |













<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchResourceAttributes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | リソースに付与可能な条件属性リスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **description** | **String**| **No** | 条件属性の説明 |



















<a id="get-a-list-of-resources"></a>
### **リソースリストの照会** { #get-a-list-of-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources/search"

<a id="get-a-list-of-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`uiPath,ASC`)|
| Request Body | **PostSearchResources.Request** | **PostSearchResources.Request**| **Yes** |  | |





##### PostSearchResources.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **No** | リソースに付与されたOperation IDリスト |
|   **resourceIdPreLike** | **String**| **No** | リソースID(前方一致)  |
|   **resourceIds** | **List&lt;String>**| **No** | リソースIDリスト |
|   **resourcePath** | **String**| **No** | リソースPath(完全一致)  |
|   **resourcePathLike** | **String**| **No** | リソースPath(前方一致)  |
|   **resourcePaths** | **List&lt;String>**| **No** | リソースPathリスト(完全一致)  |
|   **resourceUiPath** | **String**| **No** | リソースUI Path(完全一致)  |
|   **resourceUiPaths** | **List&lt;String>**| **No** | リソースUI Pathリスト(完全一致)  |
|   **roleIds** | **List&lt;String>**| **No** | リソースに付与されたロールIDリスト |
|   **scopeIds** | **List&lt;String>**| **No** | リソースにアクセス可能なスコープIDリスト |
|   **searchRoleOptionCode** | **String**| **No** | アクセス可能なロールリスト検索方式 DIRECT_ROLE, INDIRECT_ROLE |
|   **userIds** | **List&lt;String>**| **No** | リソースにアクセス可能なユーザーIDリスト |






















<a id="get-a-list-of-resources-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ]
}
```



##### PostSearchResources.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **List&lt;ResourceProtocol>**| **Yes** | リソースリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |




















<a id="modify-resources"></a>
### リソースの修正 { #modify-resources }
> PUT "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="modify-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 
| Request Body | **UpdateResource.Request** | **UpdateResource.Request**| **Yes** |  | |






##### UpdateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **newResourceId** | **String**| **No** | 変更するリソースID  |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |















<a id="modify-resources-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="resource-hierarchy"></a>
## リソース階層構造 { #resource-hierarchy }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources**](#viewing-child-resource-pages-on-a-ui-path) | ui path上の下位リソースページ照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/hierarchy/search**](#get-resource-hierarchy) | リソースHierarchy照会 |


<a id="viewing-child-resource-pages-on-a-ui-path"></a>
### **ui path上の下位リソースページ照会** { #viewing-child-resource-pages-on-a-ui-path }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources"

<a id="viewing-child-resource-pages-on-a-ui-path-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 
|  Query |**userId** | **String**| **No** | ユーザーID |
|  Query |**roleId** | **String**| **No** | ロールID |
|  Query |**operationId** | **String**| **No** | オペレーションID |
|  Query |**scopeId** | **String**| **No** | スコープID |
|  Query |**depth** | **Integer**| **No** | リソースUI Pathの下位の階層の深さ |
|  Query |**limit** | **Integer**| **No** | 返すリストの位置。 default: INT_MAX |
|  Query |**offset** | **Integer**| **No** | 返すリストの開始位置。 default: 0 |
















<a id="viewing-child-resource-pages-on-a-ui-path-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ],
  "totalItemCount" : 0
}
```



##### GetSubResources.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **List&lt;ResourceProtocol>**| **No** | リソースリスト |
|   **totalItemCount** | **Long**| **No** | リソースの総数 |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |




















<a id="get-resource-hierarchy"></a>
### **リソースHierarchy照会** { #get-resource-hierarchy }
> POST "/role/v3.0/appkeys/{appKey}/resources/hierarchy/search"

<a id="get-resource-hierarchy-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **SearchResourceHierarchy.Request** | **SearchResourceHierarchy.Request**| **Yes** |  | |





##### SearchResourceHierarchy.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
| **operationIds** | **List&lt;String>**| **No** | リソースに割り当てられたオペレーションIDリスト |
| **resourceIds** | **List&lt;String>**| **No** | 階層構造のRoot Resource IDリスト |
| **resourcePath** | **String**| **No** | 階層構造のRoot Resource Path |
| **resourceUiPath** | **String**| **No** | 階層構造のRoot Resource Ui Path |
| **roleIds** | **List&lt;String>**| **No** | リソースに割り当てられたロールIDリスト |
| **scopeIds** | **List&lt;String>**| **No** | ユーザーに割り当てられたスコープIDリスト |
| **userIds** | **List&lt;String>**| **No** | リソースにアクセス可能なユーザーIDリスト |















<a id="get-resource-hierarchy-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "resources" : [ null, null ],
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "resources" : [ null, null ],
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ]
}
```



##### SearchResourceHierarchy.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | リソース階層構造リスト |


##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 子階層のリソースリスト |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 子階層のリソースリスト |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 子階層のリソースリスト |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | リソースの説明 |
|   **metadata** | **String**| **No** | メタデータ |
|   **name** | **String**| **No** | リソース名 |
|   **path** | **String**| **Yes** | リソースPath  |
|   **priority** | **Integer**| **Yes** | 優先順位 |
|   **resourceId** | **String**| **No** | リソースID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 子階層のリソースリスト |
|   **uiPath** | **String**| **Yes** | リソースUI Path  |







(../Models/SearchResourceHierarchy.ResourceHierarchyProtocol.md)


























<a id="user-related-role"></a>
## リソース関連ロール { #user-related-role }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#add-a-resource-role-relation) | リソースロール関連関係を追加 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#get-a-list-of-resource-role-relations) | リソースロール関連関係リストの照会 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#delete-a-resource-role-relation) | リソースロール関連関係を削除 |


<a id="add-a-resource-role-relation"></a>
### **リソースロール関連関係の追加** { #add-a-resource-role-relation }
> POST "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="add-a-resource-role-relation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 
| Request Body | **AddAuthorization.Request** | **AddAuthorization.Request**| **Yes** |  | |






##### AddAuthorization.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | オペレーションID  |
|   **propagation** | **Boolean**| **No** | Rootを除いた全ての親Pathに指定したロールを同じように適用するかどうか  |
|   **roleId** | **String**| **Yes** | ロールID  |











<a id="add-a-resource-role-relation-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="get-a-list-of-resource-role-relations"></a>
### **リソースロール関連関係リストの照会** { #get-a-list-of-resource-role-relations }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="get-a-list-of-resource-role-relations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 









<a id="get-a-list-of-resource-role-relations-response-body"></a>
#### Response Body

```json
{
  "authorizations" : [ {
    "resourceId" : "resourceId",
    "roleId" : "roleId",
    "operationId" : "operationId"
  }, {
    "resourceId" : "resourceId",
    "roleId" : "roleId",
    "operationId" : "operationId"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAuthorizations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;ResourceAuthorizationProtocol>**| **No** | リソースロール関連関係リスト |

##### ResourceAuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | オペレーションID  |
|   **resourceId** | **String**| **Yes** | リソースID  |
|   **roleId** | **String**| **Yes** | ロールID  |
















<a id="delete-a-resource-role-relation"></a>
### **リソースロール関連関係の削除** { #delete-a-resource-role-relation }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="delete-a-resource-role-relation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**resourceId** | **String**| **Yes** | リソースID | 
|  Query |**operationId** | **String**| **Yes** | オペレーションID | 
|  Query |**roleId** | **String**| **Yes** | ロールID | 











<a id="delete-a-resource-role-relation-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="operations"></a>
## オペレーション { #operations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations**](#create-an-operation) | オペレーションの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#delete-operations) | オペレーションの削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations**](#delete-operatios) | オペレーションの一括削除 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#single-operation-lookup) | オペレーション単件照会 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/id**](#get-all-operation-ids) | すべてのオペレーションID照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations/search**](#get-operations-list-conditionspaging) | オペレーションリストの照会(条件/ページング) |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#modifying-operations) | オペレーションの修正 |


<a id="create-an-operation"></a>
### **オペレーションの作成** { #create-an-operation }
> POST "/role/v3.0/appkeys/{appKey}/operations"

<a id="create-an-operation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateOperation.Request** | **CreateOperation.Request**| **Yes** |  | |





##### CreateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | オペレーションの説明 |
|   **operationId** | **String**| **Yes** | オペレーションID  |










<a id="create-an-operation-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-operations"></a>
### **オペレーションの削除** { #delete-operations }
> DELETE "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="delete-operations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**operationId** | **String**| **Yes** | オペレーションID | 









<a id="delete-operations-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-operatios"></a>
### **オペレーションの一括削除** { #delete-operatios }
> DELETE "/role/v3.0/appkeys/{appKey}/operations"

<a id="delete-operatios-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**operationIds** |  **List&lt;String>**| **Yes** | オペレーションIDリスト |


<a id="delete-operatios-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="single-operation-lookup"></a>
### **オペレーション単件照会** { #single-operation-lookup }
> GET "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="single-operation-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**operationId** | **String**| **Yes** | オペレーションID | 









<a id="single-operation-lookup-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "operation" : {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  }
}
```



##### GetOperation.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operation** | **OperationResponseProtocol**| **Yes** | オペレーション      |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | アプリケーションキー |
|   **description** | **String**| **No** | オペレーションの説明 |
|   **operationId** | **String**| **Yes** | オペレーションID  |















<a id="get-all-operation-ids"></a>
### **すべてのオペレーションID照会** { #get-all-operation-ids }
> GET "/role/v3.0/appkeys/{appKey}/operations/id"

<a id="get-all-operation-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**operationIdPreLike** | **String**| **No** | オペレーションID(前方一致) |
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.operationId,ASC`)|











<a id="get-all-operation-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "operationIds" : [ "operationIds", "operationIds" ]
}
```



##### GetAllOperationIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **Yes** | オペレーションIDリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |











<a id="get-operations-list-conditionspaging"></a>
### **オペレーションリストの照会(条件/ページング)** { #get-operations-list-conditionspaging }
> POST "/role/v3.0/appkeys/{appKey}/operations/search"

<a id="get-operations-list-conditionspaging-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.operationId,ASC`)|
| Request Body | **PostSearchOperations.Request** | **PostSearchOperations.Request**| **Yes** |  | |





##### PostSearchOperations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | オペレーションの説明(部分一致)  |
|   **operationIdPreLike** | **String**| **No** | オペレーションID(前方一致)  |
|   **operationIds** | **List&lt;String>**| **No** | オペレーションIDリスト(完全一致)  |













<a id="get-operations-list-conditionspaging-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "operations" : [ {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  }, {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostSearchOperations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operations** | **List&lt;OperationResponseProtocol>**| **Yes** | オペレーションリスト |
|   **totalItems** | **Long**| **Yes** | 全体数 |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | アプリケーションキー |
|   **description** | **String**| **No** | オペレーションの説明 |
|   **operationId** | **String**| **Yes** | オペレーションID  |
















<a id="modifying-operations"></a>
### **オペレーションの修正** { #modifying-operations }
> PUT "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="modifying-operations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**operationId** | **String**| **Yes** | オペレーションID | 
| Request Body | **UpdateOperation.Request** | **UpdateOperation.Request**| **Yes** |  | |






##### UpdateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | オペレーションの説明 |









<a id="modifying-operations-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="condition-attribute"></a>
## 条件属性 { #condition-attribute }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes**](#create-condition-attribute) | 条件属性の作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#delete-condition-attribute) | 条件属性の削除 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes**](#delete-condition-attributes) | 条件属性の一括削除 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#single-lookup-of-condition-attribute) | 条件属性の単件照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/id**](#get-a-list-of-condition-attribute-ids) | 条件属性IDリストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/search**](#get-a-list-of-condition-attributes) | 条件属性リストの照会 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#modify-condition-attributes) | 条件属性の修正 |


<a id="create-condition-attribute"></a>
### **条件属性の作成** { #create-condition-attribute }
> POST "/role/v3.0/appkeys/{appKey}/attributes"

<a id="create-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **CreateAttribute.Request** | **CreateAttribute.Request**| **Yes** |  | |





##### CreateAttribute.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 条件属性と関連するロールIDリスト |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト |
|   **description** | **String**| **No** | 条件属性の説明 |














<a id="create-condition-attribute-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-condition-attribute"></a>
### **条件属性の削除** { #delete-condition-attribute }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="delete-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
|  Query |**forceDelete** | **Boolean**| **No** | 強制削除、デフォルト値(false) |










<a id="delete-condition-attribute-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="delete-condition-attributes"></a>
### **条件属性の一括削除** { #delete-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes"

<a id="delete-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー |
| Request Body |**attributeIds** |  **List&lt;String>**| **Yes** | 条件属性IDリスト |
| Request Body |**forceDelete** | **Boolean**| **No** | 強制削除、デフォルト値(false) |

<a id="delete-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a id="single-lookup-of-condition-attribute"></a>
### **条件属性単件照会** { #single-lookup-of-condition-attribute }
> GET "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="single-lookup-of-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 |
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 









<a id="single-lookup-of-condition-attribute-response-body"></a>
#### Response Body

```json
{
  "attributeInUse" : true,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attribute" : {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  }
}
```



##### GetAttribute.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **attribute** | **AttributeBundleProtocol**| **Yes** | 条件属性      |
|   **attributeInUse** | **Boolean**| **Yes** | 条件属性を使用するかどうか |

##### AttributeBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 条件属性と関連するロールIDリスト |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 条件属性タグID  |
|   **description** | **String**| **No** | 条件属性の説明 |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regYmdt** | **Date**| **Yes** | 条件属性と関連するロールIDの作成日時 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |












##### AttributeTagProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeTagId** | **String**| **Yes** | 条件属性タグID  |
|   **regYmdt** | **Date**| **Yes** | 条件属性タグの作成日時 |






















<a id="get-a-list-of-condition-attribute-ids"></a>
### **条件属性IDリストの照会** { #get-a-list-of-condition-attribute-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/id"

<a id="get-a-list-of-condition-attribute-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 条件属性作成タイプリスト |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 条件属性データタイプ |
|   **attributeIdPreLike** | **String**| **No** | 条件属性ID(前方一致)  |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |
|   **descriptionLike** | **String**| **No** | 条件属性説明(部分一致)  |
|   **roleIdPreLike** | **String**| **No** | ロールID(前方一致)  |
|   **roleIds** | **List&lt;String>**| **No** | ロールIDリスト(完全一致)  |


















<a id="get-a-list-of-condition-attribute-ids-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "attributeIds" : [ "attributeIds", "attributeIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeIds.Response


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **Yes** | 条件属性IDリスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |











<a id="get-a-list-of-condition-attributes"></a>
### **ロールで設定可能なすべての条件属性リストの照会** { #get-a-list-of-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/search"

<a id="get-a-list-of-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 条件属性作成タイプリスト |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 条件属性データタイプ |
|   **attributeIdPreLike** | **String**| **No** | 条件属性ID(前方一致)  |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |
|   **descriptionLike** | **String**| **No** | 条件属性説明(部分一致)  |
|   **roleIdPreLike** | **String**| **No** | ロールID(前方一致)  |
|   **roleIds** | **List&lt;String>**| **No** | ロールIDリスト(完全一致)  |


















<a id="get-a-list-of-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 6,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchAttributes.Response


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeBundleProtocol>**| **Yes** | 条件属性リスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |

##### AttributeBundleProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 条件属性と関連するロールIDリスト |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 条件属性タグID  |
|   **description** | **String**| **No** | 条件属性の説明 |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regYmdt** | **Date**| **Yes** | 条件属性と関連するロールIDの作成日時 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |












##### AttributeTagProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeTagId** | **String**| **Yes** | 条件属性タグID  |
|   **regYmdt** | **Date**| **Yes** | 条件属性タグの作成日時 |






















<a id="modify-condition-attributes"></a>
### **条件属性の修正** { #modify-condition-attributes }
> PUT "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="modify-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
| Request Body | **UpdateAttribute.Request** | **UpdateAttribute.Request**| **Yes** |  | |






##### UpdateAttribute.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeName** | **String**| **No** | 条件属性名 |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 条件属性と関連するロールIDリスト |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト |
|   **description** | **String**| **No** | 条件属性の説明 |













<a id="modify-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a id="condition-attribute-data-types"></a>
## 条件属性データ型 { #condition-attribute-data-types }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/data-types**](#get-condition-attribute-data-types) | 条件属性データ型リストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/condition/validate**](#validating-condition-values) | 条件値有効性チェック |


<a id="get-condition-attribute-data-types"></a>
### **条件属性データ型リストの照会** { #get-condition-attribute-data-types }
> POST "/role/v3.0/appkeys/{appKey}/attributes/data-types"

<a id="get-condition-attribute-data-types-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 








<a id="get-condition-attribute-data-types-response-body"></a>
#### Response Body

```json
{
  "dataTypes" : [ {
    "operators" : [ {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    }, {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    } ],
    "dataType" : "dataType"
  }, {
    "operators" : [ {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    }, {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    } ],
    "dataType" : "dataType"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAttributeDataTypeResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **dataTypes** | **List&lt;GetAttributeDataTypeResponse.AttributeDataTypeProtocol>**| **Yes** | 条件属性データ型リスト |

##### GetAttributeDataTypeResponse.AttributeDataTypeProtocol


| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **dataType** | **String**| **Yes** | 条件属性データ型       |
|   **operators** | **List&lt;GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol>**| **Yes** | 条件属性が使用可能な演算子リスト |


##### GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **max** | **Integer**| **Yes** | 演算子が使用可能な値の最大数 |
|   **min** | **Integer**| **Yes** | 演算子が使用可能な値の最小数 |
|   **operatorTypeCode** | **String**| **Yes** | 演算子 |




















<a id="validating-condition-values"></a>
### **条件値有効性チェック** { #validating-condition-values }
> POST "/role/v3.0/appkeys/{appKey}/attributes/condition/validate"

<a id="validating-condition-values-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **ValidateConditionValuesRequest** | **ValidateConditionValuesRequest**| **Yes** |  | |





##### ValidateConditionValuesRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **Yes** | ロール条件属性 |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 条件属性値 |















<a id="validating-condition-values-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a id="condition-attribute-role-associations"></a>
## 条件属性ロールの関連関係 { #condition-attribute-role-associations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#create-multiple-roles-associated-with-condition-attributes) | 条件属性と関連するロールの一括作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#delete-multiple-roles-associated-with-condition-attributes) | 条件属性と関連するロールの一括削除 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search**](#get-roles-associated-with-condition-attributes) | 条件属性と関連するロールリストの照会 |


<a id="create-multiple-roles-associated-with-condition-attributes"></a>
### **条件属性と関連するロールの一括作成** { #create-multiple-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="create-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
| Request Body | **CreateAttributeRoleRelations.Request** | **CreateAttributeRoleRelations.Request**| **Yes** |  | |






##### CreateAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 条件属性と関連するロールIDリスト |









<a id="create-multiple-roles-associated-with-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-multiple-roles-associated-with-condition-attributes"></a>
### **条件属性と関連するロールの一括削除** { #delete-multiple-roles-associated-with-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="delete-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
| Request Body | **DeleteAttributeRoleRelations.Request** | **DeleteAttributeRoleRelations.Request**| **Yes** |  | |






##### DeleteAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 条件属性と関連するロールIDリスト |









<a id="delete-multiple-roles-associated-with-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="get-roles-associated-with-condition-attributes"></a>
### **条件属性と関連するロールリストの照会** { #get-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search"

<a id="get-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`attribute.id.attributeId,ASC`)|
| Request Body | **SearchAttributeRoleRelations.Request** | **SearchAttributeRoleRelations.Request**| **Yes** |  | |






##### SearchAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIdPreLike** | **String**| **No** | 条件属性と関連するロールID(前方一致)  |
|   **roleIds** | **List&lt;String>**| **No** | 条件属性と関連するロールIDリスト(完全一致)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |













<a id="get-roles-associated-with-condition-attributes-response-body"></a>
#### Response Body

```json
{
  "attributeRoleRelations" : [ {
    "attributeId" : "attributeId",
    "exposureOrder" : 0,
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "roleGroup" : "roleGroup"
  }, {
    "attributeId" : "attributeId",
    "exposureOrder" : 0,
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "roleGroup" : "roleGroup"
  } ],
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeRoleRelations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 条件属性関連関係Roleリスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |

##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **description** | **String**| **No** | ロールの説明 |
|   **exposureOrder** | **Integer**| **Yes** | 表示順序 |
|   **regYmdt** | **Date**| **Yes** | 条件属性と関連するロールIDの作成日時 |
|   **roleGroup** | **String**| **No** | ロールグループ |
|   **roleId** | **String**| **Yes** | ロールID  |
|   **roleName** | **String**| **No** | ロール名 |



















<a id="condition-attribute-tag"></a>
## 条件属性タグ { #condition-attribute-tag }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#create-condition-attribute-tag) | 条件属性タグの作成 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#delete-condition-attribute-tag) | 条件属性タグの削除 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/id**](#get-a-list-of-condition-attribute-tag-ids) | 条件属性タグIDリストの照会 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/search**](#get-a-list-of-condition-attribute-tags) | 条件属性タグリストの照会 |


<a id="create-condition-attribute-tag"></a>
### **条件属性タグの作成** { #create-condition-attribute-tag }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="create-condition-attribute-tag-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
| Request Body | **CreateAttributeTags.Request** | **CreateAttributeTags.Request**| **Yes** |  | |






##### CreateAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 条件属性タグIDリスト |









<a id="create-condition-attribute-tag-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="delete-condition-attribute-tag"></a>
### **条件属性タグの削除** { #delete-condition-attribute-tag }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="delete-condition-attribute-tag-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Path |**attributeId** | **String**| **Yes** | 条件属性ID | 
| Request Body | **DeleteAttributeTags.Request** | **DeleteAttributeTags.Request**| **Yes** |  | |






##### DeleteAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 条件属性タグIDリスト |









<a id="delete-condition-attribute-tag-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="get-a-list-of-condition-attribute-tag-ids"></a>
### **条件属性タグIDリストの照会** { #get-a-list-of-condition-attribute-tag-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/id"

<a id="get-a-list-of-condition-attribute-tag-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTagIds.Request** | **SearchAttributeTagIds.Request**| **Yes** |  | |





##### SearchAttributeTagIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 条件属性ID(前方一致)  |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeTagIdPreLike** | **String**| **No** | 条件属性タグID(前方一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |














<a id="get-a-list-of-condition-attribute-tag-ids-response-body"></a>
#### Response Body

```json
{
  "attributeTagIds" : [ "attributeTagIds", "attributeTagIds" ],
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeTagIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 条件属性タグIDリスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |











<a id="get-a-list-of-condition-attribute-tags"></a>
### **条件属性タグリストの照会** { #get-a-list-of-condition-attribute-tags }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/search"

<a id="get-a-list-of-condition-attribute-tags-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
|  Query |**page** | **Integer**| **No** | 検索したいページ番号(デフォルト値1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 結果を求めるページ別の検索数(デフォルト値10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | ソート順序(デフォルト値`id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTags.Request** | **SearchAttributeTags.Request**| **Yes** |  | |





##### SearchAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 条件属性ID(前方一致)  |
|   **attributeIds** | **List&lt;String>**| **No** | 条件属性IDリスト(完全一致)  |
|   **attributeTagIdPreLike** | **String**| **No** | 条件属性タグID(前方一致)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 条件属性タグIDリスト(完全一致)  |














<a id="get-a-list-of-condition-attribute-tags-response-body"></a>
#### Response Body

```json
{
  "totalItems" : 0,
  "attributeTags" : [ {
    "attributeId" : "attributeId",
    "attributeTagId" : "attributeTagId",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00"
  }, {
    "attributeId" : "attributeId",
    "attributeTagId" : "attributeTagId",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeTags.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 条件属性タグリスト |
|   **totalItems** | **Long**| **Yes** | ロールの総数 |

##### AttributeTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 条件属性ID  |
|   **attributeTagId** | **String**| **Yes** | 条件属性タグID  |
|   **regYmdt** | **Date**| **Yes** | 条件属性タグの作成日時 |















<a id="settings"></a>
## 設定 { #settings }


| Method | HTTP request                                                        | Description                     |
|------------- |---------------------------------------------------------------------|---------------------------------|
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config/cache-evict**](#purge-the-cache-of-the-server-and-client-sdks) | ROLEサービスサーバーとクライアントSDKのキャッシュを削除 |
| **GET** | [**/role/v3.0/appkeys/{appKey}/config**](#get-settings)         | 設定の照会                          |
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config**](#modify-settings)             | 設定を修正                          |


<a id="purge-the-cache-of-the-server-and-client-sdks"></a>
### **サーバーとクライアントSDKのキャッシュを削除** { #purge-the-cache-of-the-server-and-client-sdks }
> PUT "/role/v3.0/appkeys/{appKey}/config/cache-evict"

<a id="purge-the-cache-of-the-server-and-client-sdks-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 








<a id="purge-the-cache-of-the-server-and-client-sdks-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a id="get-settings"></a>
### **設定の照会** { #get-settings }
> GET "/role/v3.0/appkeys/{appKey}/config"

<a id="get-settings-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 








<a id="get-settings-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetTenantConfigResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**| **Yes** |   IDENTICAL_PATH, NON_IDENTICAL_PATH |










<a id="modify-settings"></a>
### **設定の修正** { #modify-settings }
> PUT "/role/v3.0/appkeys/{appKey}/config"

<a id="modify-settings-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 秘密鍵 | 
|  Path |**appKey** | **String**| **Yes** | アプリケーションキー | 
| Request Body | **UpdateConfig.Request** | **UpdateConfig.Request**| **Yes** |  | |





##### UpdateConfig.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **cacheSize** | **Integer**| **No** | リソースIDベースの認証キャッシュサイズ |
|   **cacheSizeByPath** | **Integer**| **No** | リソースHierarchy照会キャッシュサイズ |
|   **cacheSizeTree** | **Integer**| **No** | リソースPathベースの認証キャッシュサイズ |
|   **cacheTtl** | **Integer**| **No** | キャッシュデータ維持時間(秒単位) |
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**| **No** |   IDENTICAL_PATH, NON_IDENTICAL_PATH |



<a id="modify-settings-response-body"></a>
#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```
