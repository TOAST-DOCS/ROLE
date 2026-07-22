<!-- pre-align:aligned sig=812d7e85772e -->

<a id="application-service-role-api-v3-guide"></a>
## Application Service > ROLE > API v3 Guide { #application-service-role-api-v3-guide }

> To check the permissions to use the ROLE service, call RESTful API or use Client SDK.
> Call RESTful APIs or use client SDKs.

<a id="authentication-and-authorization"></a>
## Authentication and Authorization { #authentication-and-authorization }

AppKey and SecretKey are required to use the ROLE API.
The Appkey is included in the request URL to identify and specify a particular resource when making API calls. A SecretKey is a private key used to control access to the API. 
For more information on checking and using Appkeys and SecretKeys, please refer to [Appkey](/en/nhncloud/en/public-api/appkey/).
Alternatively, a Project-integrated Appkey can be used in place of Appkey. For more information on creating and using Project Integrated Appkeys, please refer to [Project Integrated Appkey](/en/nhncloud/en/public-api/project-integrated-appkey/).

<a id="restful-api-guide"></a>
## RESTful API Guide { #restful-api-guide }

<a id="common-response-body"></a>
### Common Response Body { #common-response-body }

All API requests are responded to with an HTTP response code of 200.
For detailed response results, see Headers in the Response Body.

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

| Key                  |  Type    |   Description                          |
|----------------------|----------|---------------------------------------|
| header               |  Object  |   [Response Header]                                 |
| header.isSuccessful  |  boolean |   Successful or not                                |
| header.resultCode    |  int     |   Response code. Returns 0 on success or an error code on failure.          |
| header.resultMessage |  String  |   Response message. Returns "SUCCESS" on success, error message on failure |
| cache                | Object   |  Cache                                      |
| cache.cacheFlushTime | String   | Cache clearing time | 
| cache.size | int      | Authentication cache size based on resource identity |
| cache.sizeByPath | int      | Resource Path-based authentication cache size |
| cache.sizeTree | int      | Resource Hierarchy Lookup Cache Size |
| cache.ttl | int      | Cache data retention time (in seconds) |

<a id="user"></a>
## User { #user }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users**](#createUsers) | Create a user |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#deleteUser) | Delete a user |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users**](#deleteUsers) | Delete users |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/id**](#getAllUsers) | Get a list of all user IDs |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#getUser) | Get user information |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/histories**](#getUserRoleHistories) | View a list of changes to roles assigned to a user |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/search**](#getUsers) | Get a list of users |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#updateUser) | Edit users |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}**](#updateUserScope) | Edit user scopes |


<a name="createUsers"></a>
<a id="create-a-user"></a>
### **Create a user** { #create-a-user }
> POST "/role/v3.0/appkeys/{appKey}/users"

<a id="create-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateUserRequest** | **CreateUserRequest**| **Yes** |  | |





##### CreateUserRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **users** | **List<CreateUserRequest.UserProtocol>**| **Yes** | User list  |

##### CreateUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | User description  |
|   **roleRelations** | **List<UserRoleRelationProtocol>**| **No** | User-related Role  |
|   **userId** | **String**| **Yes** | User ID  |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |------|
|   **conditions** | **List<ConditionProtocol>**| **No** | Role Condition Attributes |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | Role ID |
|   **scopeId** | **String**| **Yes** |  Scope ID    |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |



























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







<a name="deleteUser"></a>
<a id="deleting-a-user"></a>
### **Deleting a user** { #deleting-a-user }
> DELETE "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="deleting-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**userId** | **String**| **Yes** | User ID | 









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


<a name="deleteUsers"></a>
<a id="delete-users"></a>
### **Delete users** { #delete-users }
> DELETE "/role/v3.0/appkeys/{appKey}/users"

<a id="delete-users-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secret key |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**userIds** |  **List&lt;String>**| **Yes** | User IDs |


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




<a name="getAllUsers"></a>
<a id="get-a-list-of-all-user-ids"></a>
### **Get a list of all user IDs** { #get-a-list-of-all-user-ids }
> POST "/role/v3.0/appkeys/{appKey}/users/id"

<a id="get-a-list-of-all-user-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | User description (partial match)  |
|   **needRoleRelations** | **Boolean**| **No** | Whether to include role relations in responses (default: true)  |
|   **needRoleTags** | **Boolean**| **No** | Whether to include role tags when including role relations in responses (default: false)  |
|   **needRoleCount** | **Boolean**| **No** | Whether to include the number of roles users have in responses (default: false)        |
|   **roleIdPreLike** | **String**| **No** | Role ID (forward match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role ID list (exact match)  |
|   **scopeIdPreLike** | **String**| **No** | Scope ID (forward match)  |
|   **scopeIds** | **List&lt;String>**| **No** | List of scope IDs (exact match)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | User ID (forward matching)  |
|   **userIds** | **List&lt;String>**| **No** | List of user IDs (exact match)  |



















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
|   **totalItems** | **Long**| **Yes** | Total number  |
|   **userIds** | **List&lt;String>**| **Yes** | User list  |











<a name="getUser"></a>
<a id="get-user-information"></a>
### **Get user information** { #get-user-information }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="get-user-information-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**userId** | **String**| **Yes** | User ID | 
|  Query |**searchRoleOptionCode** | **String**| **No** | How to search the list of accessible roles | [optional] [default to null] [enum: DIRECT_ROLE, INDIRECT_ROLE] |
|  Query |**roleIds** |  **List&lt;String>**| **No** | Relationship role ID |
|  Query |**scopeIds** |  **List&lt;String>**| **No** | Relationship scope ID |










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
|   **user** | **UserBundleProtocol**| **Yes** | User         |


##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Description  |
|   **regYmdt** | **Date**| **No** | User Creation Date  |
|   **roleRelations** | **List<UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | List of roles assigned to the user  |
|   **userId** | **String**| **Yes** | User ID  |



##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionBundleProtocol>**| **No** | Role Condition Attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regYmdt** | **Date**| **No** | At enrollment  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleTags** | **List<UserBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |
|   **scopeId** | **String**| **Yes** | Scope ID  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | Condition attribute                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | Condition attribute ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | Role Tag ID  |























<a name="getUserRoleHistories"></a>
<a id="view-a-list-of-changes-to-roles-assigned-to-a-user"></a>
### **View a list of changes to roles assigned to a user** { #view-a-list-of-changes-to-roles-assigned-to-a-user }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}/histories"

<a id="view-a-list-of-changes-to-roles-assigned-to-a-user-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**userId** | **String**| **Yes** | User ID | 
|  Query |**roleId** | **String**| **No** | Role ID |
|  Query |**scopeId** | **String**| **No** | Scope ID |
|  Query |**fromDateTime** | **Date**| **No** | When the change starts |
|  Query |**toDateTime** | **Date**| **No** | When the change ends |
|  Query |**historyType** |  **List&lt;String>**| **No** | Change type | [optional] [default to null] [enum: USER_ADD, USER_REMOVE, ADD, REMOVE] |
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `seq,DESC`)|















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
|   **totalItems** | **Long**| **Yes** | Total number  |
|   **userHistory** | **List<UserHistoryProtocol>**| **Yes** | User change history list  |



##### UserHistoryProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **command** | **String**| **Yes** |   USER_ADD, USER_REMOVE, ADD, REMOVE |
|   **conditions** | **List<ConditionBundleProtocol>**| **No** | Role Condition Attributes  |
|   **executionTime** | **Date**| **Yes** | Date of change  |
|   **operatorUuid** | **String**| **No** | Worker UUID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |
|   **roleId** | **String**| **No** | Role ID  |
|   **scopeId** | **String**| **No** | Scope ID  |
|   **userHistorySeq** | **Long**| **Yes** | User change history serial number  |
|   **userId** | **String**| **Yes** | User ID  |


##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | Condition attribute                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | Condition attribute ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |




<a name="getUsers"></a>
<a id="get-a-list-of-users"></a>
### **Get a list of users** { #get-a-list-of-users }
> POST "/role/v3.0/appkeys/{appKey}/users/search"

<a id="get-a-list-of-users-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | User description (partial match)  |
|   **needRoleRelations** | **Boolean**| **No** | Whether to include role relations in responses (default: true)  |
|   **needRoleTags** | **Boolean**| **No** | Whether to include role tags when including role relations in responses (default: false)  |
|   **needRoleCount** | **Boolean**| **No** | Whether to include the number of roles users have in responses (default: false)        |
|   **roleIdPreLike** | **String**| **No** | Role ID (forward match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role ID list (exact match)  |
|   **scopeIdPreLike** | **String**| **No** | Scope ID (forward match)  |
|   **scopeIds** | **List&lt;String>**| **No** | List of scope IDs (exact match)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | User ID (forward matching)  |
|   **userIds** | **List&lt;String>**| **No** | List of user IDs (exact match)  |




















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
|   **totalItems** | **Long**| **Yes** | Total number  |
|   **users** | **List<UserBundleProtocol>**| **Yes** | User list  |



##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Description  |
|   **regYmdt** | **Date**| **No** | User Creation Date  |
|   **roleRelations** | **List<UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | List of roles assigned to the user  |
|   **userId** | **String**| **Yes** | User ID  |
|   **roleCounts** | **List&lt;UserRoleCountProtocol>**| **No**   | The number of roles assigned to users  |



##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionBundleProtocol>**| **No** | Role Condition Attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regYmdt** | **Date**| **No** | At enrollment  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleTags** | **List<UserBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |
|   **scopeId** | **String**| **Yes** | Scope ID  |

##### UserBundleProtocol.UserRoleCountProtocol

| Name | Type | Required | Description | 
|------------ | ------------ | ------------- | ------------ |
|   **scopeId** | **String**| **Yes** | Scope ID  |
|   **roleCount** | **Long**| **Yes** | The number of roles for each scope ID  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | Condition attribute                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | Condition attribute ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | Role Tag ID  |























<a name="updateUser"></a>
<a id="edit-users"></a>
### **Edit users** { #edit-users }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="edit-users-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey   | 
|  Path |**appKey** | **String**| **Yes** | Appkey          | 
|  Path |**userId** | **String**| **Yes** | User ID      | 
| Request Body | **PutUserRequest** | **PutUserRequest**| **Yes** | User         | |






##### PutUserRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserRequest.UserProtocol**| **Yes** | User         |
| **createUserIfNotExist** | **Boolean** | **No** | Whether to create if the user doesn't exist at the time of the request |

##### PutUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | User description  |
|   **roleRelations** | **List<UserRoleRelationProtocol>**| **No** | User-related Role  |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List<ConditionProtocol>**| **No** | Role Condition Attributes    |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | Role ID       |
|   **scopeId** | **String**| **Yes** | Scope ID       |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |


























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



<a name="updateUserScope"></a>
<a id="edit-user-scopes"></a>
### **Edit user scopes** { #edit-user-scopes }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}"

<a id="edit-user-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description |
|------------- |------------- | ------------- | ------------- |-------------|
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey   |
|  Path |**appKey** | **String**| **Yes** | Appkey |
|  Path |**userId** | **String**| **Yes** | User ID |
|  Path |**scopeId** | **String**| **Yes** | Scope ID |
| Request Body | **putUserScopeRequest** | **PutUserScopeRequest**| **Yes** | User |

##### PutUserScopeRequest

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserScopeRequest.UserProtocol**| **Yes** | User |
| **createUserIfNotExist** | **Boolean** | **No** | Whether to create if the user doesn't exist at the time of the request |

##### PutUserScopeRequest.UserProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | User description  |
|   **roleRelations** | **List&lt;UserScopeRoleRelationProtocol>**| **No** | Roles related to users  |


##### UserScopeRoleRelationProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | Role condition attributes |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | Role ID |

##### ConditionProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Attributre value  |

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
## User authentication { #user-authentication }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources**](#checkResource) | Check if a user is authorized to access a resource |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles**](#checkRole) | Check if a user has access to a role |


<a name="checkResource"></a>
<a id="check-if-a-user-is-authorized-to-access-a-resource"></a>
### **Check if a user is authorized to access a resource** { #check-if-a-user-is-authorized-to-access-a-resource }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources"

<a id="check-if-a-user-is-authorized-to-access-a-resource-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey   | 
|  Path |**appKey** | **String**| **Yes** | Appkey          | 
|  Path |**userId** | **String**| **Yes** | User ID      | 
| Request Body | **PostAuthorizationResource.Request** | **PostAuthorizationResource.Request**| **Yes** | Resource List      | |






##### PostAuthorizationResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **Resources** | **List<PostAuthorizationResource.ResourceProtocol>**| **Yes** | Resource List      |

##### PostAuthorizationResource.ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List<PostAuthorizationResource.AttributeProtocol>**| **No** | Condition attribute ID  |
|   **authRequestId** | **String**| **No** | Request Authentication Identification Key  |
|   **operationId** | **String**| **Yes** | Operation ID  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **resourcePath** | **String**| **No** | Resource Path  |
|   **scopeId** | **String**| **No** | Scope ID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeValue** | **String**| **Yes** | Condition attribute value  |























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
|   **authorizations** | **List<PostAuthorizationResource.AuthorizationWithResourceProtocol>**| **No** | List of permission check results  |

##### PostAuthorizationResource.AuthorizationWithResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List<PostAuthorizationResource.AttributeProtocol>**| **Yes** | Condition attribute ID  |
|   **authRequestId** | **String**| **No** | Request Authentication Identification Key  |
|   **operationId** | **String**| **Yes** | Operation ID  |
|   **permission** | **Boolean**| **Yes** | Authorization status  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **resourcePath** | **String**| **No** | Resource Path  |
|   **scopeId** | **String**| **Yes** | Scope ID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeValue** | **String**| **Yes** | Condition attribute value  |

























<a name="checkRole"></a>
<a id="check-if-a-user-has-access-to-a-role"></a>
### **Check if a user has access to a role** { #check-if-a-user-has-access-to-a-role }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles"

<a id="check-if-a-user-has-access-to-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**userId** | **String**| **Yes** | User ID | 
| Request Body | **PostAuthorizationRole.Request** | **PostAuthorizationRole.Request**| **Yes** |  | |






##### PostAuthorizationRole.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List<PostAuthorizationRole.AuthRoleProtocol>**| **Yes** | Authentication request list  |

##### PostAuthorizationRole.AuthRoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List<PostAuthorizationRole.AttributeProtocol>**| **No** | Condition attribute  |
|   **authRequestId** | **String**| **No** | Authentication request identification key  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **scopeId** | **String**| **No** | Scope ID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeValue** | **String**| **Yes** | Condition attribute value  |





















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
|   **authorizations** | **List<PostAuthorizationRole.AuthorizationProtocol>**| **No** | List of permission check results  |

##### PostAuthorizationRole.AuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List<PostAuthorizationRole.AttributeProtocol>**| **Yes** | Condition attribute ID  |
|   **authRequestId** | **String**| **No** | Request Authentication Identification Key  |
|   **permission** | **Boolean**| **Yes** | Authorization status  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **scopeId** | **String**| **Yes** | Scope ID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeValue** | **String**| **Yes** | Condition attribute value  |






















<a id="roles"></a>
## Roles { #roles }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles**](#createRole) | Create a role |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#deleteRole) | Delete a role |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles**](#deleteRoles) | Delete roles |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable**](#getDeniable) | Whether the role is enabled or can be changed to DENY (not enabled) |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#getRole) | Single role lookup |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/id**](#searchAllRoleIds) | Get a list of all role IDs |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search**](#searchAttributesByRoleId) | Get a list of all condition attributes that can be set in a role |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/containing-roles/search**](#searchContainingRoles) | 특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/search**](#searchRoles) | Get a list of roles |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#updateRole) | Modify roles |


<a name="createRole"></a>
<a id="create-a-role"></a>
### **Create a role** { #create-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles"

<a id="create-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateRoleRequest** | **CreateRoleRequest**| **Yes** |  | |





##### CreateRoleRequest


| Name | Type | Required | Description      | 
|------------ | ------------- | ------------- |------------------|
|   **role** | **RoleProtocol**| **No** | Roles |
|   **roleRelations** | **List<CreateRoleRequest.RoleRelationProtocol>**| **No** | List of role IDs associated with the condition attribute |
|   **roleTags** | **List<CreateRoleRequest.RoleTagProtocol>**| **No** | Role tag list         |

##### RoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |










##### CreateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionProtocol>**| **No** | Role Condition Attributes  |
|   **relatedRoleId** | **String**| **Yes** | Role ID associated with the condition attribute  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |














##### CreateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | Role Tag ID  |













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







<a name="deleteRole"></a>
<a id="deleting-roles"></a>
### **Deleting roles** { #deleting-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="deleting-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**roleId** | **String**| **Yes** | Role ID | 









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


<a name="deleteRoles"></a>
<a id="delete-roles"></a>
### **Delete roles** { #delete-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="delete-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**roleIds** |  **List&lt;String>**| **Yes** | Role IDs |


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




<a name="getDeniable"></a>
<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled"></a>
### **Whether the role is enabled or can be changed to DENY (not enabled)** { #whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable"

<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**roleId** | **String**| **Yes** | Role ID | 









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
|   **deniable** | **Boolean**| **No** | Whether the role is enabled or can be changed to DENY (not enabled)  |










<a name="getRole"></a>
<a id="single-role-lookup"></a>
### **Single role lookup** { #single-role-lookup }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="single-role-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**roleId** | **String**| **Yes** | Role ID | 









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
|   **role** | **RoleBundleProtocol**| **Yes** | Roles |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | Appkey  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | Condition attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regDateTime** | **Date**| **Yes** | When the role was created  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleRelations** | **List<RoleBundleProtocol.RoleRelationProtocol>**| **No** | List of relations roles  |
|   **roleTags** | **List<RoleBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionBundleProtocol>**| **No** | Role Condition Attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **regDateTime** | **Date**| **Yes** | When the role was created  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | Condition attribute                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | Condition attribute ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | Role Tag ID  |

















<a name="searchAllRoleIds"></a>
<a id="get-a-list-of-all-role-ids"></a>
### **Get a list of all role IDs** { #get-a-list-of-all-role-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/id"

<a id="get-a-list-of-all-role-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**roleIdPreLike** | **String**| **No** | Role ID (forward match) |
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.roleId,ASC`)|











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
|   **roleIds** | **List&lt;String>**| **Yes** | List of role IDs  |
|   **totalItems** | **Long**| **Yes** | Total number  |











<a name="searchAttributesByRoleId"></a>
<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **Get a list of all condition attributes that can be set in a role** { #get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search"

<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**roleId** | **String**| **Yes** | Role ID | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `attributeCreationTypeCode,ASC&quot;,&quot;id.attributeId,ASC&quot`)|
| Request Body | **SearchRoleAttributes.Request** | **SearchRoleAttributes.Request**| **Yes** |  | |






##### SearchRoleAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeNameLike** | **String**| **No** | Condition attribute name (partial match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |













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
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | List of condition attributes that can be assigned to roles  |
|   **totalItems** | **Long**| **Yes** | Total number  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | Condition attribute description  |



















<a name="searchContainingRoles"></a>
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










<a name="searchRoles"></a>
<a id="get-a-list-of-roles"></a>
### **Get a list of roles** { #get-a-list-of-roles }
> POST "/role/v3.0/appkeys/{appKey}/roles/search"

<a id="get-a-list-of-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `exposureOrder,ASC&quot;,&quot;id.roleId,ASC&quot`)|
| Request Body | **GetRoles.Request** | **GetRoles.Request**| **Yes** |  | |





##### GetRoles.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |
|   **descriptionLike** | **String**| **No** | Role description (partial match)  |
|   **needAttributes** | **Boolean**| **No** | Whether to include condition attribute information in the response  |
|   **needRoleRelations** | **Boolean**| **No** | Whether to include a list of role association IDs in the response  |
|   **needRoleTags** | **Boolean**| **No** | Whether to include a list of role tag IDs in the response  |
|   **relatedRoleIds** | **List&lt;String>**| **No** | List of related role IDs (exact match)  |
|   **roleGroup** | **String**| **No** | Role group(exact match)  |
|   **roleGroupLike** | **String**| **No** | Role groups (partial match)  |
|   **roleIdPreLike** | **String**| **No** | Role ID (forward match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role ID list (exact match)  |
|   **roleNameLike** | **String**| **No** | Role name (partial match)  |
|   **roleTagIdExpr** | **String**| **No** | Role tag conditions (separator ';':OR, ',':AND)  |
|   **roleTagIds** | **List&lt;String>**| **No** | List of role tag IDs (exact match)  |























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
|   **roles** | **List<RoleBundleProtocol>**| **Yes** | Roles list  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | Appkey  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | Condition attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regDateTime** | **Date**| **Yes** | When the role was created  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleRelations** | **List<RoleBundleProtocol.RoleRelationProtocol>**| **No** | List of related roles  |
|   **roleTags** | **List<RoleBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionBundleProtocol>**| **No** | Role Condition Attributes  |
|   **description** | **String**| **No** | Role descriptions  |
|   **regDateTime** | **Date**| **Yes** | When the role was created  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | Role tag list  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | Condition attribute                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | Condition attribute ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | Role Tag ID  |


















<a name="updateRole"></a>
<a id="modify-roles"></a>
### **Modify roles** { #modify-roles }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="modify-roles-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**roleId** | **String**| **Yes** | Role ID | 
| Request Body | **UpdateRoleRequest** | **UpdateRoleRequest**| **Yes** |  | |






##### UpdateRoleRequest


| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **role** | **RoleMetadataProtocol**| **No** | Roles                  |
|   **roleRelations** | **List<UpdateRoleRequest.RoleRelationProtocol>**| **No** | List of role IDs associated with the condition attribute |
|   **roleTags** | **List<UpdateRoleRequest.RoleTagProtocol>**| **No** | Role tag list            |

##### RoleMetadataProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleName** | **String**| **No** | Role name  |









##### UpdateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionProtocol>**| **No** | Role Condition Attributes  |
|   **relatedRoleId** | **String**| **Yes** | Role ID associated with the condition attribute  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |














##### UpdateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | Role Tag ID  |













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
## Role tags { #role-tags }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/tags/id**](#getAllRoleTagIds) | Get a list of all role tag IDs |


<a name="getAllRoleTagIds"></a>
<a id="get-a-list-of-all-role-tag-ids"></a>
### **Get a list of all role tag IDs** { #get-a-list-of-all-role-tag-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/tags/id"

<a id="get-a-list-of-all-role-tag-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**roleTagIdPreLike** | **String**| **No** | Role Tag ID (forward match) |
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.roleTagId,ASC`)|











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
|   **roleTagIds** | **List&lt;String>**| **No** | List of role tag IDs  |
|   **totalItems** | **Long**| **Yes** | Total number  |




<a id="role-related-relations"></a>
## Role-related relations { #role-related-relations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#createRoleRelations) | Create role-related relations |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#deleteRoleRelations) | Delete role-related relations |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#updateRoleRelations) | Edit role-related relations |

<a name="createRoleRelations"></a>
<a id="create-role-related-relations"></a>
### **Create role-related relations** { #create-role-related-relations }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="create-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
|  Path |**roleId** | **String**| **Yes** | Role ID |
| Request Body | **CreateRoleRelationRequest** | **CreateRoleRelationRequest**| **Yes** |  | |

##### CreateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | role-related relations |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | Role condition attribute  |
|   **relatedRoleId** | **String**| **Yes** | Role ID related to condition attribute  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |


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

<a name="deleteRoleRelations"></a>
<a id="delete-role-realated-relations"></a>
### **Delete role realated relations** { #delete-role-realated-relations }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="delete-role-realated-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
|  Path |**roleId** | **String**| **Yes** | Role ID |
| Request Body | **DeleteRoleRelationRequest** | **DeleteRoleRelationRequest**| **Yes** |  | |

##### DeleteRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **relatedRoleIds** | **List&lt;String>**| **Yes** | Role-related relation IDs |


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


<a name="updateRoleRelations"></a>
<a id="edit-role-related-relations"></a>
### **Edit role-related relations** { #edit-role-related-relations }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="edit-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
|  Path |**roleId** | **String**| **Yes** | Role ID |
| Request Body | **UpdateRoleRelationRequest** | **UpdateRoleRelationRequest**| **Yes** |  | |

##### UpdateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | Role-related realtions |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | Role condition attribute  |
|   **relatedRoleId** | **String**| **Yes** | Role ID related to condition attribute  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |


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
## Scope { #scope }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](#createScope) | Create a scope |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#deleteScope) | Delete a scope |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes**](#deleteScopes) | Delete scopes |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](#getAllScopeIds) | Get a list of all scope IDs |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#getScope) | Get a single scope |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](#postSearchScopes) | Get a list of scopes |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#updateScope) | Modify scope |


<a name="createScope"></a>
<a id="create-a-scope"></a>
### **Create a scope** { #create-a-scope }
> POST "/role/v3.0/appkeys/{appKey}/scopes"

<a id="create-a-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateScope.Request** | **CreateScope.Request**| **Yes** |  | |





##### CreateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Scope description  |
|   **scopeId** | **String**| **Yes** | Scope ID  |










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







<a name="deleteScope"></a>
<a id="delete-a-scope"></a>
### **Delete a scope** { #delete-a-scope }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="delete-a-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**scopeId** | **String**| **Yes** |  | 









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


<a name="deleteScopes"></a>
<a id="delete-scopes"></a>
### **Delete scopes** { #delete-scopes }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes"

<a id="delete-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**scopeIds** |  **List&lt;String>**| **Yes** | Scope IDs |


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




<a name="getAllScopeIds"></a>
<a id="get-a-list-of-all-scope-ids"></a>
### **Get a list of all scope IDs** { #get-a-list-of-all-scope-ids }
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"

<a id="get-a-list-of-all-scope-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Query |**scopeIdPreLike** | **String**| **No** | Scope ID (forward match) |
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.scopeId,ASC`)|











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
|   **scopeIds** | **List&lt;String>**| **No** | List of Scope IDs  |
|   **totalItems** | **Long**| **Yes** | Total number  |











<a name="getScope"></a>
<a id="get-a-single-scope"></a>
### **Get a single scope** { #get-a-single-scope }
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="get-a-single-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**scopeId** | **String**| **Yes** |  | 









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
|   **scope** | **ScopeProtocol**| **No** | Scope          |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Scope description  |
|   **scopeId** | **String**| **Yes** | Scope ID  |














<a name="postSearchScopes"></a>
<a id="get-a-list-of-scopes"></a>
### **Get a list of scopes** { #get-a-list-of-scopes }
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"

<a id="get-a-list-of-scopes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.scopeId,ASC`)|
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**| **Yes** |  | |





##### PostSearchScopes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | Scope description (partial match)  |
|   **scopeIdPreLike** | **String**| **No** | Scope ID (forward match)  |
|   **scopeIds** | **List&lt;String>**| **No** | List of scope IDs (exact match)  |













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
|   **scopes** | **List<ScopeProtocol>**| **No** | Scope list  |
|   **totalItems** | **Long**| **No** | Total number of scopes  |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Scope description  |
|   **scopeId** | **String**| **Yes** | Scope ID  |















<a name="updateScope"></a>
<a id="modify-scope"></a>
### **Modify scope** { #modify-scope }
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="modify-scope-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**scopeId** | **String**| **Yes** |  | 
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**| **Yes** |  | |






##### UpdateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Description  |









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
## Resource { #resource }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources**](#createResource) | Create Resources |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#deleteResource) | Delete Resource |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources**](#deleteResources) | Delete Resources |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#getResource) | Single resource lookup |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/id**](#getResourceIds) | Get a list of resource IDs |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/attributes/search**](#searchAttributesByResource) | Get a list of all condition attributes that can be set in a role |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/search**](#searchResources) | Get a list of resources |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#updateResource) | Modify Resources |


<a name="createResource"></a>
<a id="create-resources"></a>
### **Create Resources** { #create-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources"

<a id="create-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateResource.Request** | **CreateResource.Request**| **Yes** |  | |





##### CreateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |















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







<a name="deleteResource"></a>
<a id="delete-resource"></a>
### **Delete Resource** { #delete-resource }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="delete-resource-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 









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


<a name="deleteResources"></a>
<a id="delete-resources"></a>
### **Delete resources** { #delete-resources }
> DELETE "/role/v3.0/appkeys/{appKey}/resources"

<a id="delete-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**resourceIds** |  **List&lt;String>**| **Yes** | Resource IDs |


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




<a name="getResource"></a>
<a id="single-resource-lookup"></a>
### **Single resource lookup** { #single-resource-lookup }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="single-resource-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 









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
|   **resource** | **ResourceProtocol**| **No** | Resource         |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |



















<a name="getResourceIds"></a>
<a id="get-a-list-of-resource-ids"></a>
### **Get a list of resource IDs** { #get-a-list-of-resource-ids }
> POST "/role/v3.0/appkeys/{appKey}/resources/id"

<a id="get-a-list-of-resource-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.resourceId,ASC`)|
| Request Body | **GetAllResourceIds.Request** | **GetAllResourceIds.Request**| **Yes** |  | |





##### GetAllResourceIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operationIds** | **List&lt;String>**| **No** | Resource ID (forward matching)      |
|   **resourceIdPreLike** | **String**| **No** | User IDs that have access to the resource      |
|   **roleIds** | **List&lt;String>**| **No** | Role ID assigned to the resource      |
|   **userIds** | **List&lt;String>**| **No** | Operation ID assigned to the resource      |














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
|   **resourceIds** | **List&lt;String>**| **Yes** | Resource ID list  |
|   **totalItems** | **Long**| **Yes** | Total number  |











<a name="searchAttributesByResource"></a>
<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **Get a list of all condition attributes that can be set in a role** { #resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/resources/attributes/search"

<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.attributeId,ASC`)|
| Request Body | **SearchResourceAttributes.Request** | **SearchResourceAttributes.Request**| **Yes** |  | |





##### SearchResourceAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | Operation ID  |
|   **resourceId** | **String**| **No** | Resource ID, or by ID only if both ID and Path are present  |
|   **resourcePath** | **String**| **No** | Resource Path  |













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
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | List of condition attributes that can be assigned to resources  |
|   **totalItems** | **Long**| **Yes** | Total number  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **description** | **String**| **No** | Condition attribute description  |



















<a name="searchResources"></a>
<a id="get-a-list-of-resources"></a>
### **Get a list of resources** { #get-a-list-of-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources/search"

<a id="get-a-list-of-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `uiPath,ASC`)|
| Request Body | **PostSearchResources.Request** | **PostSearchResources.Request**| **Yes** |  | |





##### PostSearchResources.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **No** | List of Operation IDs assigned to resources  |
|   **resourceIdPreLike** | **String**| **No** | Resource ID (forward matching)  |
|   **resourceIds** | **List&lt;String>**| **No** | Resource ID list  |
|   **resourcePath** | **String**| **No** | Resource Path (exact match)  |
|   **resourcePathLike** | **String**| **No** | Resource Path (forward matching)  |
|   **resourcePaths** | **List&lt;String>**| **No** | Resource Path list (exact match)  |
|   **resourceUiPath** | **String**| **No** | Resource UI Path (exact match)  |
|   **resourceUiPaths** | **List&lt;String>**| **No** | Resource UI Path list (exact match)  |
|   **roleIds** | **List&lt;String>**| **No** | List of role IDs assigned to the resource  |
|   **scopeIds** | **List&lt;String>**| **No** | List of scope IDs accessible to the resource  |
|   **searchRoleOptionCode** | **String**| **No** | How to retrieve the list of accessible roles DIRECT_ROLE, INDIRECT_ROLE |
|   **userIds** | **List&lt;String>**| **No** | List of user IDs that have access to the resource  |






















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
|   **Resources** | **List<ResourceProtocol>**| **Yes** | Resource List  |
|   **totalItems** | **Long**| **Yes** | Total number  |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |




















<a name="updateResource"></a>
<a id="modify-resources"></a>
### **Modify Resources** { #modify-resources }
> PUT "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="modify-resources-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 
| Request Body | **UpdateResource.Request** | **UpdateResource.Request**| **Yes** |  | |






##### UpdateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **newResourceId** | **String**| **No** | Resource ID to change  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |















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
## Resource hierarchy { #resource-hierarchy }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources**](#getSubResources) | Viewing child resource pages on a UI PATH |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/hierarchy/search**](#searchAllResourceHierarchy) | Get Resource Hierarchy |


<a name="getSubResources"></a>
<a id="viewing-child-resource-pages-on-a-ui-path"></a>
### **Viewing child resource pages on a UI PATH** { #viewing-child-resource-pages-on-a-ui-path }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources"

<a id="viewing-child-resource-pages-on-a-ui-path-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 
|  Query |**userId** | **String**| **No** | User ID |
|  Query |**roleId** | **String**| **No** | Role ID |
|  Query |**operationId** | **String**| **No** | Operation ID |
|  Query |**scopeId** | **String**| **No** | Scope ID |
|  Query |**depth** | **Integer**| **No** | Hierarchy depth of children in the Resource UI Path |
|  Query |**limit** | **Integer**| **No** | The position of the list to return. default: INT_MAX |
|  Query |**offset** | **Integer**| **No** | The starting position of the list to return. default: 0 |
















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
|   **Resources** | **List<ResourceProtocol>**| **No** | Resource List  |
|   **totalItemCount** | **Long**| **No** | Total number of resources  |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |




















<a name="searchAllResourceHierarchy"></a>
<a id="get-resource-hierarchy"></a>
### **Get Resource Hierarchy** { #get-resource-hierarchy }
> POST "/role/v3.0/appkeys/{appKey}/resources/hierarchy/search"

<a id="get-resource-hierarchy-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **SearchResourceHierarchy.Request** | **SearchResourceHierarchy.Request**| **Yes** |  | |






##### SearchResourceHierarchy.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operationIds** | **List&lt;String>**| **No** | List of user IDs that have access to the resource            |
|   **resourceIds** | **List&lt;String>**| **No** | List of role IDs assigned to the resource            |
|   **resourcePath** | **String**| **No** | List of operation IDs assigned to the resource            |
|   **resourceUiPath** | **String**| **No** | List of scope IDs assigned to the user            |
|   **roleIds** | **List&lt;String>**| **No** | List of Root Resource IDs in the hierarchy            |
|   **scopeIds** | **List&lt;String>**| **No** | Root Resource Path in the hierarchy            |
|   **userIds** | **List&lt;String>**| **No** | Root Resource Ui Path in Hierarchy            |















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
|   **Resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | Resource hierarchy list  |


##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **Resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | List of resources in the child hierarchy  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **Resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | List of resources in the child hierarchy  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **Resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | List of resources in the child hierarchy  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Resource descriptions  |
|   **metadata** | **String**| **No** | Metadata  |
|   **name** | **String**| **No** | Resource name  |
|   **path** | **String**| **Yes** | Resource Path  |
|   **priority** | **Integer**| **Yes** | Priority  |
|   **resourceId** | **String**| **No** | Resource ID  |
|   **Resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | List of resources in the child hierarchy  |
|   **uiPath** | **String**| **Yes** | Resource UI Path  |







(../Models/SearchResourceHierarchy.ResourceHierarchyProtocol.md)


























<a id="user-related-role"></a>
## User-related Role { #user-related-role }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#addAuthorization) | Add a resource role relation |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#getAuthorizations) | Get a list of resource role relations |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#removeAuthorization) | Delete a resource role relation |


<a name="addAuthorization"></a>
<a id="add-a-resource-role-relation"></a>
### **Add a resource role relation** { #add-a-resource-role-relation }
> POST "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="add-a-resource-role-relation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 
| Request Body | **AddAuthorization.Request** | **AddAuthorization.Request**| **Yes** |  | |






##### AddAuthorization.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | Operation ID  |
|   **propagation** | **Boolean**| **No** | Whether to apply the specified role equally to all parent paths except Root.  |
|   **roleId** | **String**| **Yes** | Role ID  |











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







<a name="getAuthorizations"></a>
<a id="get-a-list-of-resource-role-relations"></a>
### **Get a list of resource role relations** { #get-a-list-of-resource-role-relations }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="get-a-list-of-resource-role-relations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 









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
|   **authorizations** | **List<ResourceAuthorizationProtocol>**| **No** | Resource role relation list  |

##### ResourceAuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | Operation ID  |
|   **resourceId** | **String**| **Yes** | Resource ID  |
|   **roleId** | **String**| **Yes** | Role Id  |
















<a name="removeAuthorization"></a>
<a id="delete-a-resource-role-relation"></a>
### **Delete a resource role relation** { #delete-a-resource-role-relation }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="delete-a-resource-role-relation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**resourceId** | **String**| **Yes** | Resource ID | 
|  Query |**operationId** | **String**| **Yes** | Operation ID | 
|  Query |**roleId** | **String**| **Yes** | Role ID | 











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
## Operations { #operations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations**](#createOperation) | Create an operation |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#deleteOperation) | Delete operations |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations**](#deleteOperations) | Delete operations |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#getOperation) | Single operation lookup |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/id**](#getOperationIdByPageable) | Get all operation IDs |
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations/search**](#postSearchOperation) | Get Operations List (Conditions/Paging) |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#updateOperation) | Modifying operations |


<a name="createOperation"></a>
<a id="create-an-operation"></a>
### **Create an operation** { #create-an-operation }
> POST "/role/v3.0/appkeys/{appKey}/operations"

<a id="create-an-operation-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateOperation.Request** | **CreateOperation.Request**| **Yes** |  | |





##### CreateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Operation description  |
|   **operationId** | **String**| **Yes** | Operation ID  |










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







<a name="deleteOperation"></a>
<a id="delete-operations"></a>
### **Delete operations** { #delete-operations }
> DELETE "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="delete-operations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**operationId** | **String**| **Yes** |  | 









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


<a name="deleteOperations"></a>
<a id="delete-operatios"></a>
### **Delete operatios** { #delete-operatios }
> DELETE "/role/v3.0/appkeys/{appKey}/operations"

<a id="delete-operatios-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secretkey |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**operationIds** |  **List&lt;String>**| **Yes** | Operation IDs |


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




<a name="getOperation"></a>
<a id="single-operation-lookup"></a>
### **Single operation lookup** { #single-operation-lookup }
> GET "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="single-operation-lookup-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**operationId** | **String**| **Yes** |  | 









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
|   **operation** | **OperationResponseProtocol**| **Yes** | Operations       |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | Appkey  |
|   **description** | **String**| **No** | Operation description  |
|   **operationId** | **String**| **Yes** | Operation ID  |















<a name="getOperationIdByPageable"></a>
<a id="get-all-operation-ids"></a>
### **Get all operation IDs** { #get-all-operation-ids }
> GET "/role/v3.0/appkeys/{appKey}/operations/id"

<a id="get-all-operation-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Query |**operationIdPreLike** | **String**| **No** | Operation ID (forward matching) |
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.operationId,ASC`)|











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
|   **operationIds** | **List&lt;String>**| **Yes** | Operation ID list  |
|   **totalItems** | **Long**| **Yes** | Total number  |











<a name="postSearchOperation"></a>
<a id="get-operations-list-conditionspaging"></a>
### **Get Operations List (Conditions/Paging)** { #get-operations-list-conditionspaging }
> POST "/role/v3.0/appkeys/{appKey}/operations/search"

<a id="get-operations-list-conditionspaging-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.operationId,ASC`)|
| Request Body | **PostSearchOperations.Request** | **PostSearchOperations.Request**| **Yes** |  | |





##### PostSearchOperations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | Operation description (partial match)  |
|   **operationIdPreLike** | **String**| **No** | Operation ID (forward matching)  |
|   **operationIds** | **List&lt;String>**| **No** | Operation ID list (exact match)  |













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
|   **operations** | **List<OperationResponseProtocol>**| **Yes** | List of operations  |
|   **totalItems** | **Long**| **Yes** | Total number  |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | Appkey  |
|   **description** | **String**| **No** | Operation description  |
|   **operationId** | **String**| **Yes** | Operation ID  |
















<a name="updateOperation"></a>
<a id="modifying-operations"></a>
### **Modifying operations** { #modifying-operations }
> PUT "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="modifying-operations-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** |  | 
|  Path |**operationId** | **String**| **Yes** |  | 
| Request Body | **UpdateOperation.Request** | **UpdateOperation.Request**| **Yes** |  | |






##### UpdateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | Operation description  |









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
## Condition attribute { #condition-attribute }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes**](#createAttribute) | Create condition attribute |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#deleteAttribute) | Delete condition attribute |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes**](#deleteAttributes) | Delete condition attributes |
| **GET** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#getAttribute) | Single lookup of condition attribute |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/id**](#searchAttributeIds) | Get a list of condition attributes IDs |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/search**](#searchAttributes) | Get a list of condition attributes |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#updateAttribute) | Modify condition attributes |


<a name="createAttribute"></a>
<a id="create-condition-attribute"></a>
### **Create condition attribute** { #create-condition-attribute }
> POST "/role/v3.0/appkeys/{appKey}/attributes"

<a id="create-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **CreateAttribute.Request** | **CreateAttribute.Request**| **Yes** |  | |





##### CreateAttribute.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | List of role IDs associated with the condition attribute  |
|   **attributeTagIds** | **List&lt;String>**| **No** | List of condition attribute tag IDs  |
|   **description** | **String**| **No** | Condition attribute description  |














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







<a name="deleteAttribute"></a>
<a id="delete-condition-attribute"></a>
### **Delete condition attribute** { #delete-condition-attribute }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="delete-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
|  Query |**forceDelete** | **Boolean**| **No** | Force delete, default (false) |










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


<a name="deleteAttributes"></a>
<a id="delete-condition-attributes"></a>
### **Delete condition attributes** { #delete-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes"

<a id="delete-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | Secret key |
|  Path |**appKey** | **String**| **Yes** | Appkey |
| Request Body |**attributeIds** |  **List&lt;String>**| **Yes** | Condition attribute IDs |
| Request Body |**forceDelete** | **Boolean**| **No** | Force delete, default value (false) |

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




<a name="getAttribute"></a>
<a id="single-lookup-of-condition-attribute"></a>
### **Single lookup of condition attribute** { #single-lookup-of-condition-attribute }
> GET "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="single-lookup-of-condition-attribute-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 









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
|   **attribute** | **AttributeBundleProtocol**| **Yes** | Condition attribute       |
|   **attributeInUse** | **Boolean**| **Yes** | Whether to use condition attributes |

##### AttributeBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | List of role IDs associated with the condition attribute  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | Condition attribute tag ID  |
|   **description** | **String**| **No** | Condition attribute description  |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regYmdt** | **Date**| **Yes** | When the role ID associated with the condition attribute was created  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |












##### AttributeTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeTagId** | **String**| **Yes** | Condition attribute tag ID  |
|   **regYmdt** | **Date**| **Yes** | When the condition attribute tag was created  |






















<a name="searchAttributeIds"></a>
<a id="get-a-list-of-condition-attribute-ids"></a>
### **Get a list of condition attribute IDs** { #get-a-list-of-condition-attribute-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/id"

<a id="get-a-list-of-condition-attribute-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | Condition attribute creation types  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | Condition attribute data types  |
|   **attributeIdPreLike** | **String**| **No** | Condition attribute tag ID list (front match)  |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |
|   **descriptionLike** | **String**| **No** | Condition attribute descriptions (partial match)  |
|   **roleIdPreLike** | **String**| **No** | Role ID (forward match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role ID list (exact match)  |


















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
|   **attributeIds** | **List&lt;String>**| **Yes** | Condition attribute IDs  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |











<a name="searchAttributes"></a>
<a id="get-a-list-of-condition-attributes"></a>
### **Get a list of condition attributes** { #get-a-list-of-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/search"

<a id="get-a-list-of-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | Condition attribute creation types  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | Condition attribute data types  |
|   **attributeIdPreLike** | **String**| **No** | Condition attribute tag ID list (front match)  |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |
|   **descriptionLike** | **String**| **No** | Condition attribute descriptions (partial match)  |
|   **roleIdPreLike** | **String**| **No** | Role ID (forward match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role ID list (exact match)  |


















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
|   **attributes** | **List&lt;AttributeBundleProtocol>**| **Yes** | Condition attributes  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |

##### AttributeBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | List of role IDs associated with the condition attribute  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | Condition attribute tag ID  |
|   **description** | **String**| **No** | Condition attribute description  |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regYmdt** | **Date**| **Yes** | When the role ID associated with the condition attribute was created  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |












##### AttributeTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeTagId** | **String**| **Yes** | Condition attribute tag ID  |
|   **regYmdt** | **Date**| **Yes** | When the condition attribute tag was created  |






















<a name="updateAttribute"></a>
<a id="modify-condition-attributes"></a>
### **Modify condition attributes** { #modify-condition-attributes }
> PUT "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="modify-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
| Request Body | **UpdateAttribute.Request** | **UpdateAttribute.Request**| **Yes** |  | |






##### UpdateAttribute.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeName** | **String**| **No** | Condition attribute name  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | List of role IDs associated with the condition attribute  |
|   **attributeTagIds** | **List&lt;String>**| **No** | List of condition attribute tag IDs  |
|   **description** | **String**| **No** | Condition attribute description  |













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
## Condition attribute data types { #condition-attribute-data-types }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/data-types**](#getAttributeDataType) | Get condition attribute data types |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/condition/validate**](#validateConditionValues) | Validating condition values |


<a name="getAttributeDataType"></a>
<a id="get-condition-attribute-data-types"></a>
### **Get condition attribute data types** { #get-condition-attribute-data-types }
> POST "/role/v3.0/appkeys/{appKey}/attributes/data-types"

<a id="get-condition-attribute-data-types-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 








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
|   **dataTypes** | **List&lt;GetAttributeDataTypeResponse.AttributeDataTypeProtocol>**| **Yes** | Condition attribute data types  |

##### GetAttributeDataTypeResponse.AttributeDataTypeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **dataType** | **String**| **Yes** | Condition attribute data types  |
|   **operators** | **List&lt;GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol>**| **Yes** | Operators available for condition attributes  |


##### GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **max** | **Integer**| **Yes** | Maximum number of values an operator can take  |
|   **min** | **Integer**| **Yes** | Minimum number of values an operator can take  |
|   **operatorTypeCode** | **String**| **Yes** | Operator  |




















<a name="validateConditionValues"></a>
<a id="validating-condition-values"></a>
### **Validating condition values** { #validating-condition-values }
> POST "/role/v3.0/appkeys/{appKey}/attributes/condition/validate"

<a id="validating-condition-values-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **ValidateConditionValuesRequest** | **ValidateConditionValuesRequest**| **Yes** |  | |





##### ValidateConditionValuesRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List<ConditionProtocol>**| **Yes** | Role Condition Attributes  |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | Condition attribute value  |















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
## Condition attribute role associations { #condition-attribute-role-associations }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#createAttributeRoleRelations) | Create multiple roles associated with condition attributes |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#deleteAttributeRoleRelations) | Delete multiple roles associated with condition attributes |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search**](#searchAttributeRoleRelations) | Get roles associated with condition attributes |


<a name="createAttributeRoleRelations"></a>
<a id="create-multiple-roles-associated-with-condition-attributes"></a>
### **Create multiple roles associated with condition attributes** { #create-multiple-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="create-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
| Request Body | **CreateAttributeRoleRelations.Request** | **CreateAttributeRoleRelations.Request**| **Yes** |  | |






##### CreateAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | List of role IDs associated with the condition attribute  |









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







<a name="deleteAttributeRoleRelations"></a>
<a id="delete-multiple-roles-associated-with-condition-attributes"></a>
### **Delete multiple roles associated with condition attributes** { #delete-multiple-roles-associated-with-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="delete-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
| Request Body | **DeleteAttributeRoleRelations.Request** | **DeleteAttributeRoleRelations.Request**| **Yes** |  | |






##### DeleteAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | List of role IDs associated with the condition attribute  |









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







<a name="searchAttributeRoleRelations"></a>
<a id="get-roles-associated-with-condition-attributes"></a>
### **Get roles associated with condition attributes** { #get-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search"

<a id="get-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `attribute.id.attributeId,ASC`)|
| Request Body | **SearchAttributeRoleRelations.Request** | **SearchAttributeRoleRelations.Request**| **Yes** |  | |






##### SearchAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIdPreLike** | **String**| **No** | Role ID associated with condition attributes (front match)  |
|   **roleIds** | **List&lt;String>**| **No** | Role IDs associated with condition attributes (exact match)  |
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
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | Roles associated with condition attributes  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |

##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **description** | **String**| **No** | Role descriptions  |
|   **exposureOrder** | **Integer**| **Yes** | Exposure order  |
|   **regYmdt** | **Date**| **Yes** | When the role ID associated with the condition attribute was created  |
|   **roleGroup** | **String**| **No** | Role group  |
|   **roleId** | **String**| **Yes** | Role ID  |
|   **roleName** | **String**| **No** | Role name  |



















<a id="condition-attribute-tag"></a>
## Condition Attribute Tag { #condition-attribute-tag }


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#createAttributeTags) | Create condition attribute tag |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#deleteAttributeTags) | Delete condition attribute tag |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/id**](#searchAttributeTagIds) | Get a list of condition attribute tag IDs |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/search**](#searchAttributeTags) | Get a list of condition attribute tags |


<a name="createAttributeTags"></a>
<a id="create-condition-attribute-tag"></a>
### **Create condition attribute tag** { #create-condition-attribute-tag }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="create-condition-attribute-tag-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
| Request Body | **CreateAttributeTags.Request** | **CreateAttributeTags.Request**| **Yes** |  | |






##### CreateAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | List of condition attribute tag IDs  |









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







<a name="deleteAttributeTags"></a>
<a id="delete-condition-attribute-tag"></a>
### **Delete condition attribute tag** { #delete-condition-attribute-tag }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="delete-condition-attribute-tag-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Path |**attributeId** | **String**| **Yes** | Condition attribute ID | 
| Request Body | **DeleteAttributeTags.Request** | **DeleteAttributeTags.Request**| **Yes** |  | |






##### DeleteAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | List of condition attribute tag IDs  |









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







<a name="searchAttributeTagIds"></a>
<a id="get-a-list-of-condition-attribute-tag-ids"></a>
### **Get a list of condition attribute tag IDs** { #get-a-list-of-condition-attribute-tag-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/id"

<a id="get-a-list-of-condition-attribute-tag-ids-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTagIds.Request** | **SearchAttributeTagIds.Request**| **Yes** |  | |





##### SearchAttributeTagIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | Condition attribute tag ID list (front match)  |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeTagIdPreLike** | **String**| **No** | Condition attribute tag ID (front match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |














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
|   **attributeTagIds** | **List&lt;String>**| **Yes** | List of condition attribute tag IDs  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |











<a name="searchAttributeTags"></a>
<a id="get-a-list-of-condition-attribute-tags"></a>
### **Get a list of condition attribute tags** { #get-a-list-of-condition-attribute-tags }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/search"

<a id="get-a-list-of-condition-attribute-tags-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
|  Query |**page** | **Integer**| **No** | The page number you want to search (default 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | Number of searches per page for which you want results (default 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | Sort order (default `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTags.Request** | **SearchAttributeTags.Request**| **Yes** |  | |





##### SearchAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | Condition attribute tag ID list (front match)  |
|   **attributeIds** | **List&lt;String>**| **No** | Condition attribute ID list (exact match)  |
|   **attributeTagIdPreLike** | **String**| **No** | Condition attribute tag ID (front match)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | Condition attribute tag ID list (exact match)  |














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
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | Condition Attribute Tag List  |
|   **totalItems** | **Long**| **Yes** | Total number of roles  |

##### AttributeTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | Condition attribute ID  |
|   **attributeTagId** | **String**| **Yes** | Condition attribute tag ID  |
|   **regYmdt** | **Date**| **Yes** | When the condition attribute tag was created  |















<a id="settings"></a>
## Settings { #settings }


| Method | HTTP request | Description                            |
|------------- | ------------- |----------------------------------------|
| **PUT** |[**/role/v3.0/appkeys/{appKey}/config/cache-evict**](#deleteCache) | Purge the cache of the Role Service server and the Client SDK |
| **GET** |[**/role/v3.0/appkeys/{appKey}/config**](#getConfiguration) | Get settings                                  |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/config**](#updateConfig) | Modify settings                                  |


<a name="deleteCache"></a>
<a id="purge-the-cache-of-the-server-and-client-sdks"></a>
### **Purge the cache of the server and client SDKs** { #purge-the-cache-of-the-server-and-client-sdks }
> PUT "/role/v3.0/appkeys/{appKey}/config/cache-evict"

<a id="purge-the-cache-of-the-server-and-client-sdks-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 








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







<a name="getConfiguration"></a>
<a id="get-settings"></a>
### **Get settings** { #get-settings }
> GET "/role/v3.0/appkeys/{appKey}/config"

<a id="get-settings-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 








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










<a name="updateConfig"></a>
<a id="modify-settings"></a>
### **Modify settings** { #modify-settings }
> PUT "/role/v3.0/appkeys/{appKey}/config"

<a id="modify-settings-parameters"></a>
#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | SecretKey | 
|  Path |**appKey** | **String**| **Yes** | Appkey | 
| Request Body | **UpdateConfig.Request** | **UpdateConfig.Request**| **Yes** |  | |





##### UpdateConfig.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **cacheSize** | **Integer**| **No** | Authentication cache size based on resource identity  |
|   **cacheSizeByPath** | **Integer**| **No** | Resource Hierarchy Lookup Cache Size  |
|   **cacheSizeTree** | **Integer**| **No** | Resource Path-based authentication cache size  |
|   **cacheTtl** | **Integer**| **No** |  Cache data retention time (in seconds) |
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
