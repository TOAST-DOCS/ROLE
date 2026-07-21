<!-- pre-align:aligned sig=812d7e85772e -->

<a id="application-service-role-api-v3-guide"></a>
## Application Service > ROLE > API v3 가이드 { #application-service-role-api-v3-guide }

> ROLE 서비스를 사용해 권한을 확인하려면
> RESTful API를 호출하거나, 클라이언트 SDK를 사용해야 합니다.

<a id="authentication-and-authorization"></a>
## 인증 및 권한 { #authentication-and-authorization }

ROLE API를 사용하려면 Appkey와 SecretKey가 필요합니다.
Appkey는 API 호출 시 요청 URL에 포함하여 특정 리소스를 가리키고 식별하는 데 사용되며, SecretKey는 API에 대한 접근을 제어하는 비밀 키입니다.
Appkey 및 SecretKey 확인 및 사용에 대한 자세한 내용은 [Appkey](/nhncloud/ko/public-api/appkey)를 참고하세요.
Appkey 대신 프로젝트 통합 Appkey를 사용할 수도 있습니다. 프로젝트 통합 Appkey에 대한 자세한 내용은 [프로젝트 통합 Appkey](/nhncloud/ko/public-api/project-integrated-appkey)를 참고하세요.

<a id="restful-api-guide"></a>
## RESTful API 가이드 { #restful-api-guide }

<a id="common-response-body"></a>
### Common Response Body { #common-response-body }

모든 API 요청에 HTTP 응답 코드 200을 반환합니다.
자세한 응답 결과는 Response Body의 Header 항목을 참고합니다.

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
| header               |  Object  | 응답 헤더                                  |
| header.isSuccessful  |  boolean | 성공 여부                                  |
| header.resultCode    |  int     | 응답 코드. 성공 시 0, 실패 시 오류 코드 반환           |
| header.resultMessage |  String  | 응답 메시지. 성공 시 "SUCCESS", 실패 시 오류 메시지 반환 |
| cache                | Object   | 캐시                                     |
| cache.cacheFlushTime | String   | 캐시 삭제 시간                               | 
| cache.size | int      | 리소스 ID 기반 인증 캐시 크기                     |
| cache.sizeByPath | int      | 리소스 Path 기반 인증 캐시 크기                   |
| cache.sizeTree | int      | 리소스 Hierarchy 조회 캐시 크기                 |
| cache.ttl | int      | 캐시 데이터 유지 시간(초 단위)                     |

<a id="user"></a>
## 사용자 { #user }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users**](#createUsers) | 사용자 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#deleteUser) | 사용자 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users**](#deleteUsers) | 사용자 다건 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/id**](#getAllUsers) | 모든 사용자 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#getUser) | 사용자 정보 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/histories**](#getUserRoleHistories) | 사용자에게 할당된 역할의 변경 내역 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/search**](#getUsers) | 사용자 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#updateUser) | 사용자 수정 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}**](#updateUserScope) | 사용자 범위 한정 수정 |

<a name="createUsers"></a>
<a id="create-a-user"></a>
### **사용자 생성** { #create-a-user }
> POST "/role/v3.0/appkeys/{appKey}/users"

<a id="create-a-user-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateUserRequest** | **CreateUserRequest**| **Yes** |  | |

##### CreateUserRequest

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **users** | **List&lt;CreateUserRequest.UserProtocol>**| **Yes** | 사용자 목록  |

##### CreateUserRequest.UserProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | 사용자 연관 역할  |
|   **userId** | **String**| **Yes** | 사용자 ID  |

##### UserRoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID |
|   **scopeId** | **String**| **Yes** |  범위 ID    |

##### ConditionProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
### **사용자 삭제** { #deleting-a-user }
> DELETE "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="deleting-a-user-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 

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
### **사용자 다건 삭제** { #delete-users }
> DELETE "/role/v3.0/appkeys/{appKey}/users"

<a id="delete-users-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**userIds** |  **List&lt;String>**| **Yes** | 사용자 ID 목록 |

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
### **모든 사용자 ID 목록 조회** { #get-a-list-of-all-user-ids }
> POST "/role/v3.0/appkeys/{appKey}/users/id"

<a id="get-a-list-of-all-user-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |

##### SearchUser.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 사용자 설명(부분 일치)  |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 여부(기본값: true)  |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 시 역할 태그 포함 여부(기본값: false)  |
|   **needRoleCount** | **Boolean**| **No** | 응답 시 사용자가 가진 역할 개수 포함 여부(기본값: false)        |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)  |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | 사용자 ID(전방 일치)  |
|   **userIds** | **List&lt;String>**| **No** | 사용자 ID 목록(완전 일치)  |

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
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **userIds** | **List&lt;String>**| **Yes** | 사용자 목록  |

<a name="getUser"></a>
<a id="get-user-information"></a>
### **사용자 정보 조회** { #get-user-information }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="get-user-information-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
|  Query |**searchRoleOptionCode** | **String**| **No** | 접근 가능한 역할 목록 검색 방식 | [optional] [default to null] [enum: DIRECT_ROLE, INDIRECT_ROLE] |
|  Query |**roleIds** |  **List&lt;String>**| **No** | 연관 관계 역할 ID |
|  Query |**scopeIds** |  **List&lt;String>**| **No** | 연관 관계 범위 ID |

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
|   **user** | **UserBundleProtocol**| **Yes** | 사용자         |

##### UserBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 설명  |
|   **regYmdt** | **Date**| **No** | 사용자 생성 일시  |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | 사용자에 할당된 역할 목록  |
|   **userId** | **String**| **Yes** | 사용자 ID  |

##### UserBundleProtocol.UserRoleRelationBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **No** | 등록일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### ConditionBundleProtocol

| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### UserBundleProtocol.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |

<a name="getUserRoleHistories"></a>
<a id="view-a-list-of-changes-to-roles-assigned-to-a-user"></a>
### **사용자에게 할당된 역할의 변경 내역 목록 조회** { #view-a-list-of-changes-to-roles-assigned-to-a-user }
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}/histories"

<a id="view-a-list-of-changes-to-roles-assigned-to-a-user-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
|  Query |**roleId** | **String**| **No** | 역할 ID |
|  Query |**scopeId** | **String**| **No** | 범위 ID |
|  Query |**fromDateTime** | **Date**| **No** | 변경 시작일시 |
|  Query |**toDateTime** | **Date**| **No** | 변경 종료일시 |
|  Query |**historyType** |  **List&lt;String>**| **No** | 변경 유형 | [optional] [default to null] [enum: USER_ADD, USER_REMOVE, ADD, REMOVE] |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `seq,DESC`)|

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
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **userHistory** | **List&lt;UserHistoryProtocol>**| **Yes** | 사용자 변경 내역 목록  |

##### UserHistoryProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **command** | **String**| **Yes** |   USER_ADD, USER_REMOVE, ADD, REMOVE |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **executionTime** | **Date**| **Yes** | 변경 일시  |
|   **operatorUuid** | **String**| **No** | 작업자 UUID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |
|   **roleId** | **String**| **No** | 역할 ID  |
|   **scopeId** | **String**| **No** | 범위 ID  |
|   **userHistorySeq** | **Long**| **Yes** | 사용자 변경 이력 일렬번호  |
|   **userId** | **String**| **Yes** | 사용자 ID  |

##### ConditionBundleProtocol

| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

<a name="getUsers"></a>
<a id="get-a-list-of-users"></a>
### **사용자 목록 조회** { #get-a-list-of-users }
> POST "/role/v3.0/appkeys/{appKey}/users/search"

<a id="get-a-list-of-users-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |

##### SearchUser.Request

| Name | Type | Required | Description                                  | 
|------------ | ------------- | ------------- |----------------------------------------------|
|   **descriptionLike** | **String**| **No** | 사용자 설명(부분 일치)                                |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 여부(기본값: true)            |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 시 역할 태그 포함 여부(기본값: false) |
|   **needRoleCount** | **Boolean**| **No** | 응답 시 사용자가 가진 역할 개수 포함 여부(기본값: false)        |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)                                 |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)                              |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)                                 |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)                              |
|   **searchRoleOptionCode** | **String**| **No** | DIRECT_ROLE, INDIRECT_ROLE                   |
|   **userIdPreLike** | **String**| **No** | 사용자 ID(전방 일치)                                |
|   **userIds** | **List&lt;String>**| **No** | 사용자 ID 목록(완전 일치)                             |

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
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **users** | **List&lt;UserBundleProtocol>**| **Yes** | 사용자 목록  |

##### UserBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- |----------| ------------ |
|   **description** | **String**| **No**   | 설명  |
|   **regYmdt** | **Date**| **No**   | 사용자 생성 일시  |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No**   | 사용자에 할당된 역할 목록  |
|   **userId** | **String**| **Yes**  | 사용자 ID  |
|   **roleCounts** | **List&lt;UserRoleCountProtocol>**| **No**   | 사용자에 할당된 역할 개수  |

##### UserBundleProtocol.UserRoleRelationBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **No** | 등록일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### UserBundleProtocol.UserRoleCountProtocol

| Name | Type | Required | Description | 
|------------ | ------------ | ------------- | ------------ |
|   **scopeId** | **String**| **Yes** | 범위 ID  |
|   **roleCount** | **Long**| **Yes** | 범위 ID별 역할 개수  |

##### ConditionBundleProtocol

| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### UserBundleProtocol.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |

<a name="updateUser"></a>
<a id="edit-users"></a>
### **사용자 수정** { #edit-users }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}"

<a id="edit-users-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키          | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID      | 
| Request Body | **PutUserRequest** | **PutUserRequest**| **Yes** | 사용자         |

##### PutUserRequest

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserRequest.UserProtocol**| **Yes** | 사용자         |
| **createUserIfNotExist** | **Boolean** | **No** | 요청 시 존재하지 않는 사용자일 경우 생성 여부 |

##### PutUserRequest.UserProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | 사용자 연관 역할  |

##### UserRoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성    |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID       |
|   **scopeId** | **String**| **Yes** | 범위 ID       |

##### ConditionProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
### **사용자 범위 한정 수정** { #edit-user-scopes }
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}"

<a id="edit-user-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description |
|------------- |------------- | ------------- | ------------- |-------------|
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**userId** | **String**| **Yes** | 사용자 ID |
|  Path |**scopeId** | **String**| **Yes** | 범위 ID |
| Request Body | **putUserScopeRequest** | **PutUserScopeRequest**| **Yes** | 사용자 |

##### PutUserScopeRequest

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserScopeRequest.UserProtocol**| **Yes** | 사용자 |
| **createUserIfNotExist** | **Boolean** | **No** | 요청 시 존재하지 않는 사용자일 경우 생성 여부 |

##### PutUserScopeRequest.UserProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserScopeRoleRelationProtocol>**| **No** | 사용자 연관 역할  |

##### UserScopeRoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
## 사용자 인증 { #user-authentication }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources**](#checkResource) | 사용자가 리소스에 접근 권한이 있는지 검사 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles**](#checkRole) | 사용자가 역할에 대한 접근 권한이 있는지 검사 |

<a name="checkResource"></a>
<a id="check-if-a-user-is-authorized-to-access-a-resource"></a>
### **사용자가 리소스에 접근 권한이 있는지 검사** { #check-if-a-user-is-authorized-to-access-a-resource }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources"

<a id="check-if-a-user-is-authorized-to-access-a-resource-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키          | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID      | 
| Request Body | **PostAuthorizationResource.Request** | **PostAuthorizationResource.Request**| **Yes** | 리소스 목록      | |

##### PostAuthorizationResource.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **resources** | **List&lt;PostAuthorizationResource.ResourceProtocol>**| **Yes** | 리소스 목록      |

##### PostAuthorizationResource.ResourceProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **No** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |
|   **scopeId** | **String**| **No** | 범위 ID  |

##### PostAuthorizationResource.AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |

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
|   **authorizations** | **List&lt;PostAuthorizationResource.AuthorizationWithResourceProtocol>**| **No** | 권한 확인 결과 목록  |

##### PostAuthorizationResource.AuthorizationWithResourceProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **Yes** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **permission** | **Boolean**| **Yes** | 권한 여부  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### PostAuthorizationResource.AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |

<a name="checkRole"></a>
<a id="check-if-a-user-has-access-to-a-role"></a>
### **사용자가 역할에 대한 접근 권한이 있는지 검사** { #check-if-a-user-has-access-to-a-role }
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles"

<a id="check-if-a-user-has-access-to-a-role-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
| Request Body | **PostAuthorizationRole.Request** | **PostAuthorizationRole.Request**| **Yes** |  | |

##### PostAuthorizationRole.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List&lt;PostAuthorizationRole.AuthRoleProtocol>**| **Yes** | 인증 요청 목록  |

##### PostAuthorizationRole.AuthRoleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **No** | 조건 속성  |
|   **authRequestId** | **String**| **No** | 인증 요청 식별키  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **scopeId** | **String**| **No** | 범위 ID  |

##### PostAuthorizationRole.AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |

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
|   **authorizations** | **List&lt;PostAuthorizationRole.AuthorizationProtocol>**| **No** | 권한 확인 결과 목록  |

##### PostAuthorizationRole.AuthorizationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **Yes** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **permission** | **Boolean**| **Yes** | 권한 여부  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### PostAuthorizationRole.AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |

<a id="roles"></a>
## 역할 { #roles }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles**](#createRole) | 역할 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#deleteRole) | 역할 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles**](#deleteRoles) | 역할 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable**](#getDeniable) | 역할 사용여부 DENY(미사용)로 변경가능 여부 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#getRole) | 역할 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/id**](#searchAllRoleIds) | 모든 역할 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search**](#searchAttributesByRoleId) | 역할에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/containing-roles/search**](#searchContainingRoles) | 특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/search**](#searchRoles) | 역할 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#updateRole) | 역할 수정 |

<a name="createRole"></a>
<a id="create-a-role"></a>
### **역할 생성** { #create-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles"

<a id="create-a-role-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateRoleRequest** | **CreateRoleRequest**| **Yes** |  | |

##### CreateRoleRequest

| Name | Type | Required | Description      | 
|------------ | ------------- | ------------- |------------------|
|   **role** | **RoleProtocol**| **No** | 역할 |
|   **roleRelations** | **List&lt;CreateRoleRequest.RoleRelationProtocol>**| **No** | 조건 속성과 연관된 역할 ID 목록 |
|   **roleTags** | **List&lt;CreateRoleRequest.RoleTagProtocol>**| **No** | 역할 태그 목록         |

##### RoleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### CreateRoleRequest.RoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

##### CreateRoleRequest.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | 역할 태그 ID  |

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
### **역할 삭제** { #deleting-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="deleting-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 

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
### **역할 다건 삭제** { #delete-roles }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="delete-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**roleIds** |  **List&lt;String>**| **Yes** | 역할 ID 목록 |

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
### **역할 사용여부 DENY(미사용)로 변경가능 여부** { #whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable"

<a id="whether-the-role-is-enabled-or-can-be-changed-to-deny-not-enabled-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 

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
|   **deniable** | **Boolean**| **No** | 역할 사용여부 DENY(미사용)로 변경가능 여부  |

<a name="getRole"></a>
<a id="single-role-lookup"></a>
### **역할 단건 조회** { #single-role-lookup }
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="single-role-lookup-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 

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
|   **role** | **RoleBundleProtocol**| **Yes** | 역할 |

##### RoleBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | 앱키  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 조건 속성 목록  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 연관 관계 역할 목록  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### RoleBundleProtocol.RoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |

##### ConditionBundleProtocol

| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### RoleBundleProtocol.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |

<a name="searchAllRoleIds"></a>
<a id="get-a-list-of-all-role-ids"></a>
### **모든 역할 ID 목록 조회** { #get-a-list-of-all-role-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/id"

<a id="get-a-list-of-all-role-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.roleId,ASC`)|

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
|   **roleIds** | **List&lt;String>**| **Yes** | 역할 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

<a name="searchAttributesByRoleId"></a>
<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **역할에서 설정 가능한 모든 조건 속성 목록 조회** { #get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search"

<a id="get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description                                                | 
|------------- |------------- | ------------- | ------------- |------------------------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키                                                  |
|  Path |**appKey** | **String**| **Yes** | 앱키                                                         | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID                                                      | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1)                                      | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10)                                 |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서 (기본값 `attributeCreationTypeCode,ASC&quot;,&quot;id.attributeId,ASC`)|
| Request Body | **SearchRoleAttributes.Request** | **SearchRoleAttributes.Request**| **Yes** |                                                            | |

##### SearchRoleAttributes.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeNameLike** | **String**| **No** | 조건 속성 이름(부분 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |

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
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | 역할에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 조건 속성 설명  |

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
### **역할 목록 조회** { #get-a-list-of-roles }
> POST "/role/v3.0/appkeys/{appKey}/roles/search"

<a id="get-a-list-of-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description                                   | 
|------------- |------------- | ------------- | ------------- |-----------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키                                     |
|  Path |**appKey** | **String**| **Yes** | 앱키                                            | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1)                         | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10)                    |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서 (기본값 `exposureOrder,ASC&quot;,&quot;id.roleId,ASC`)|
| Request Body | **GetRoles.Request** | **GetRoles.Request**| **Yes** |                                               | |

##### GetRoles.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 역할 설명(부분 일치)  |
|   **needAttributes** | **Boolean**| **No** | 응답 시 조건 속성 정보 포함 여부  |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 ID 목록 포함 여부  |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 태그 ID 목록 포함 여부  |
|   **relatedRoleIds** | **List&lt;String>**| **No** | 연관 관계 역할 ID 목록(완전 일치)  |
|   **roleGroup** | **String**| **No** | 역할 그룹(완전 일치)  |
|   **roleGroupLike** | **String**| **No** | 역할 그룹(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |
|   **roleNameLike** | **String**| **No** | 역할 이름(부분 일치)  |
|   **roleTagIdExpr** | **String**| **No** | 역할 태그 조건(구분자 &#39;;&#39;:OR, &#39;,&#39;:AND)  |
|   **roleTagIds** | **List&lt;String>**| **No** | 역할 태그 ID 목록(완전 일치)  |

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
|   **roles** | **List&lt;RoleBundleProtocol>**| **Yes** | 역할 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### RoleBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | 앱키  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 조건 속성 목록  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 연관 관계 역할 목록  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### RoleBundleProtocol.RoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |

##### ConditionBundleProtocol

| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### RoleBundleProtocol.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |

<a name="updateRole"></a>
<a id="modify-roles"></a>
### **역할 수정** { #modify-roles }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

<a id="modify-roles-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 
| Request Body | **UpdateRoleRequest** | **UpdateRoleRequest**| **Yes** |  | |

##### UpdateRoleRequest

| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **role** | **RoleMetadataProtocol**| **No** | 역할                  |
|   **roleRelations** | **List&lt;UpdateRoleRequest.RoleRelationProtocol>**| **No** | 조건 속성과 연관된 역할 ID 목록 |
|   **roleTags** | **List&lt;UpdateRoleRequest.RoleTagProtocol>**| **No** | 역할 태그 목록            |

##### RoleMetadataProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### UpdateRoleRequest.RoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

##### UpdateRoleRequest.RoleTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | 역할 태그 ID  |

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
## 역할 태그 { #role-tags }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/tags/id**](#getAllRoleTagIds) | 모든 역할 태그 ID 목록 조회 |

<a name="getAllRoleTagIds"></a>
<a id="get-a-list-of-all-role-tag-ids"></a>
### **모든 역할 태그 ID 목록 조회** { #get-a-list-of-all-role-tag-ids }
> GET "/role/v3.0/appkeys/{appKey}/roles/tags/id"

<a id="get-a-list-of-all-role-tag-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**roleTagIdPreLike** | **String**| **No** | 역할 태그 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.roleTagId,ASC`)|

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
|   **roleTagIds** | **List&lt;String>**| **No** | 역할 태그 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

<a id="role-related-relations"></a>
## 역할 연관 관계 { #role-related-relations }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#createRoleRelations) | 역할 연관 관계 다건 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#deleteRoleRelations) | 역할 연관 관계 다건 삭제 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#updateRoleRelations) | 역할 연관 관계 다건 수정 |

<a name="createRoleRelations"></a>
<a id="create-role-related-relations"></a>
### **역할 연관 관계 다건 생성** { #create-role-related-relations }
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="create-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **CreateRoleRelationRequest** | **CreateRoleRelationRequest**| **Yes** |  | |

##### CreateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | 역할 연관 관계 목록 |

##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
### **역할 연관 관계 다건 삭제** { #delete-role-realated-relations }
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="delete-role-realated-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **DeleteRoleRelationRequest** | **DeleteRoleRelationRequest**| **Yes** |  | |

##### DeleteRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **relatedRoleIds** | **List&lt;String>**| **Yes** | 연관 관계 역할 ID 목록 |

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
### **역할 연관 관계 다건 수정** { #edit-role-related-relations }
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

<a id="edit-role-related-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **UpdateRoleRelationRequest** | **UpdateRoleRelationRequest**| **Yes** |  | |

##### UpdateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | 역할 연관 관계 목록 |

##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
## 범위 { #scope }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](#createScope) | 범위 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#deleteScope) | 범위 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes**](#deleteScopes) | 범위 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](#getAllScopeIds) | 모든 범위 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#getScope) | 범위 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](#postSearchScopes) | 범위 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#updateScope) | 범위 수정 |

<a name="createScope"></a>
<a id="create-a-scope"></a>
### **범위 생성** { #create-a-scope }
> POST "/role/v3.0/appkeys/{appKey}/scopes"

<a id="create-a-scope-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateScope.Request** | **CreateScope.Request**| **Yes** |  | |

##### CreateScope.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

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
### **범위 삭제** { #delete-a-scope }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="delete-a-scope-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 

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
### **범위 다건 삭제** { #delete-scopes }
> DELETE "/role/v3.0/appkeys/{appKey}/scopes"

<a id="delete-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**scopeIds** |  **List&lt;String>**| **Yes** | 범위 ID 목록 |

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
### **모든 범위 ID 목록 조회** { #get-a-list-of-all-scope-ids }
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"

<a id="get-a-list-of-all-scope-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.scopeId,ASC`)|

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
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

<a name="getScope"></a>
<a id="get-a-single-scope"></a>
### **범위 단건 조회** { #get-a-single-scope }
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="get-a-single-scope-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 

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
|   **scope** | **ScopeProtocol**| **No** | 범위          |

##### ScopeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

<a name="postSearchScopes"></a>
<a id="get-a-list-of-scopes"></a>
### **범위 목록 조회** { #get-a-list-of-scopes }
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"

<a id="get-a-list-of-scopes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.scopeId,ASC`)|
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**| **Yes** |  | |

##### PostSearchScopes.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 범위 설명(부분 일치)  |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)  |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)  |

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
|   **scopes** | **List&lt;ScopeProtocol>**| **No** | 범위 목록  |
|   **totalItems** | **Long**| **No** | 범위 총 개수  |

##### ScopeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

<a name="updateScope"></a>
<a id="modify-scope"></a>
### **범위 수정** { #modify-scope }
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

<a id="modify-scope-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**| **Yes** |  | |

##### UpdateScope.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 설명  |

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
## 리소스 { #resource }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources**](#createResource) | 리소스 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#deleteResource) | 리소스 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources**](#deleteResources) | 리소스 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#getResource) | 리소스 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/id**](#getResourceIds) | 리소스 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/attributes/search**](#searchAttributesByResource) | 리소스에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/search**](#searchResources) | 리소스 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#updateResource) | 리소스 수정 |

<a name="createResource"></a>
<a id="create-resources"></a>
### **리소스 생성** { #create-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources"

<a id="create-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateResource.Request** | **CreateResource.Request**| **Yes** |  | |

##### CreateResource.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

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
### **리소스 삭제** { #delete-resource }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="delete-resource-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 

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
### **리소스 다건 삭제** { #delete-resources }
> DELETE "/role/v3.0/appkeys/{appKey}/resources"

<a id="delete-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**resourceIds** |  **List&lt;String>**| **Yes** | 리소스 ID 목록 |

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
### **리소스 단건 조회** { #single-resource-lookup }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="single-resource-lookup-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 

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
|   **resource** | **ResourceProtocol**| **No** | 리소스         |

##### ResourceProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

<a name="getResourceIds"></a>
<a id="get-a-list-of-resource-ids"></a>
### **리소스 ID 목록 조회** { #get-a-list-of-resource-ids }
> POST "/role/v3.0/appkeys/{appKey}/resources/id"

<a id="get-a-list-of-resource-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.resourceId,ASC`)|
| Request Body | **GetAllResourceIds.Request** | **GetAllResourceIds.Request**| **Yes** |  | |

##### GetAllResourceIds.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operationIds** | **List&lt;String>**| **No** | 리소스 ID(전방 일치)      |
|   **resourceIdPreLike** | **String**| **No** | 리소스에 접근 가능한 사용자 ID      |
|   **roleIds** | **List&lt;String>**| **No** | 리소스에 부여된 역할 ID      |
|   **userIds** | **List&lt;String>**| **No** | 리소스에 부여된 Operation ID      |

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
|   **resourceIds** | **List&lt;String>**| **Yes** | 리소스 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

<a name="searchAttributesByResource"></a>
<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role"></a>
### **리소스에서 설정 가능한 모든 조건 속성 목록 조회** { #resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role }
> POST "/role/v3.0/appkeys/{appKey}/resources/attributes/search"

<a id="resource-get-a-list-of-all-condition-attributes-that-can-be-set-in-a-role-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchResourceAttributes.Request** | **SearchResourceAttributes.Request**| **Yes** |  | |

##### SearchResourceAttributes.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **No** | 리소스 ID, ID와 Path 가 둘다 있을 경우 ID 기준으로만 제공  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |

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
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | 리소스에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### AttributeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |

<a name="searchResources"></a>
<a id="get-a-list-of-resources"></a>
### **리소스 목록 조회** { #get-a-list-of-resources }
> POST "/role/v3.0/appkeys/{appKey}/resources/search"

<a id="get-a-list-of-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `uiPath,ASC`)|
| Request Body | **PostSearchResources.Request** | **PostSearchResources.Request**| **Yes** |  | |

##### PostSearchResources.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **No** | 리소스에 부여된 Operation ID 목록  |
|   **resourceIdPreLike** | **String**| **No** | 리소스 ID(전방 일치)  |
|   **resourceIds** | **List&lt;String>**| **No** | 리소스 ID 목록  |
|   **resourcePath** | **String**| **No** | 리소스 Path(완전 일치)  |
|   **resourcePathLike** | **String**| **No** | 리소스 Path(전방 일치)  |
|   **resourcePaths** | **List&lt;String>**| **No** | 리소스 Path 목록(완전 일치)  |
|   **resourceUiPath** | **String**| **No** | 리소스 UI Path(완전 일치)  |
|   **resourceUiPaths** | **List&lt;String>**| **No** | 리소스 UI Path 목록(완전 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 리소스에 부여된 역할 ID 목록  |
|   **scopeIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 범위 ID 목록  |
|   **searchRoleOptionCode** | **String**| **No** | 접근 가능한 역할 목록 검색 방식  DIRECT_ROLE, INDIRECT_ROLE |
|   **userIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 사용자 ID 목록  |

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
|   **resources** | **List&lt;ResourceProtocol>**| **Yes** | 리소스 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### ResourceProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

<a name="updateResource"></a>
<a id="modify-resources"></a>
### **리소스 수정** { #modify-resources }
> PUT "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

<a id="modify-resources-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
| Request Body | **UpdateResource.Request** | **UpdateResource.Request**| **Yes** |  | |

##### UpdateResource.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **newResourceId** | **String**| **No** | 변경할 리소스 ID  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

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
## 리소스 계층 구조 { #resource-hierarchy }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources**](#getSubResources) | ui path 상의 하위 리소스 페이지 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/hierarchy/search**](#searchAllResourceHierarchy) | 리소스 Hierarchy 조회 |

<a name="getSubResources"></a>
<a id="viewing-child-resource-pages-on-a-ui-path"></a>
### **ui path 상의 하위 리소스 페이지 조회** { #viewing-child-resource-pages-on-a-ui-path }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources"

<a id="viewing-child-resource-pages-on-a-ui-path-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
|  Query |**userId** | **String**| **No** | 사용자 ID |
|  Query |**roleId** | **String**| **No** | 역할 ID |
|  Query |**operationId** | **String**| **No** | 오퍼레이션 ID |
|  Query |**scopeId** | **String**| **No** | 범위 ID |
|  Query |**depth** | **Integer**| **No** | 리소스 UI Path에서 하위의 계층 깊이 |
|  Query |**limit** | **Integer**| **No** | 반환할 목록의 위치. default: INT_MAX |
|  Query |**offset** | **Integer**| **No** | 반환할 목록의 시작 위치. default: 0 |

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
|   **resources** | **List&lt;ResourceProtocol>**| **No** | 리소스 목록  |
|   **totalItemCount** | **Long**| **No** | 리소스 전체 개수  |

##### ResourceProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

<a name="searchAllResourceHierarchy"></a>
<a id="get-resource-hierarchy"></a>
### **리소스 Hierarchy 조회** { #get-resource-hierarchy }
> POST "/role/v3.0/appkeys/{appKey}/resources/hierarchy/search"

<a id="get-resource-hierarchy-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **SearchResourceHierarchy.Request** | **SearchResourceHierarchy.Request**| **Yes** |  | |

##### SearchResourceHierarchy.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
| **operationIds** | **List&lt;String>**| **No** | 리소스에 할당된 오퍼레이션 ID 목록 |
| **resourceIds** | **List&lt;String>**| **No** | 계층 구조의 Root Resource ID 목록 |
| **resourcePath** | **String**| **No** | 계층 구조의 Root Resource Path |
| **resourceUiPath** | **String**| **No** | 계층 구조의 Root Resource Ui Path |
| **roleIds** | **List&lt;String>**| **No** | 리소스에 할당된 역할 ID 목록 |
| **scopeIds** | **List&lt;String>**| **No** | 사용자에게 할당된 범위 ID 목록 |
| **userIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 사용자 ID 목록 |

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
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 리소스 계층 구조 목록  |

##### SearchResourceHierarchy.ResourceHierarchyProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

##### SearchResourceHierarchy.ResourceHierarchyProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

##### SearchResourceHierarchy.ResourceHierarchyProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

##### SearchResourceHierarchy.ResourceHierarchyProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |

(../Models/SearchResourceHierarchy.ResourceHierarchyProtocol.md)

<a id="user-related-role"></a>
## 리소스 연관 역할 { #user-related-role }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#addAuthorization) | 리소스 역할 연관 관계 추가 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#getAuthorizations) | 리소스 역할 연관 관계 목록 조회 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#removeAuthorization) | 리소스 역할 연관 관계 삭제 |

<a name="addAuthorization"></a>
<a id="add-a-resource-role-relation"></a>
### **리소스 역할 연관 관계 추가** { #add-a-resource-role-relation }
> POST "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="add-a-resource-role-relation-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
| Request Body | **AddAuthorization.Request** | **AddAuthorization.Request**| **Yes** |  | |

##### AddAuthorization.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **propagation** | **Boolean**| **No** | Root를 제외한 모든 상위 Path에 지정한 역할을 동일하게 적용할지 여부  |
|   **roleId** | **String**| **Yes** | 역할 ID  |

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
### **리소스 역할 연관 관계 목록 조회** { #get-a-list-of-resource-role-relations }
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="get-a-list-of-resource-role-relations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 

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
|   **authorizations** | **List&lt;ResourceAuthorizationProtocol>**| **No** | 리소스 역할 연관 관계 목록  |

##### ResourceAuthorizationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **Yes** | 리소스 ID  |
|   **roleId** | **String**| **Yes** | 역할 Id  |

<a name="removeAuthorization"></a>
<a id="delete-a-resource-role-relation"></a>
### **리소스 역할 연관 관계 삭제** { #delete-a-resource-role-relation }
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

<a id="delete-a-resource-role-relation-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
|  Query |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 
|  Query |**roleId** | **String**| **Yes** | 역할 ID | 

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
## 오퍼레이션 { #operations }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations**](#createOperation) | 오퍼레이션 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#deleteOperation) | 오퍼레이션 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations**](#deleteOperations) | 오퍼레이션 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#getOperation) | 오퍼레이션 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/id**](#getOperationIdByPageable) | 모든 오퍼레이션 ID 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations/search**](#postSearchOperation) | 오퍼레이션 목록 조회(조건/페이징) |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#updateOperation) | 오퍼레이션 수정 |

<a name="createOperation"></a>
<a id="create-an-operation"></a>
### **오퍼레이션 생성** { #create-an-operation }
> POST "/role/v3.0/appkeys/{appKey}/operations"

<a id="create-an-operation-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateOperation.Request** | **CreateOperation.Request**| **Yes** |  | |

##### CreateOperation.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |

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
### **오퍼레이션 삭제** { #delete-operations }
> DELETE "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="delete-operations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 

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
### **오퍼레이션 다건 삭제** { #delete-operatios }
> DELETE "/role/v3.0/appkeys/{appKey}/operations"

<a id="delete-operatios-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**operationIds** |  **List&lt;String>**| **Yes** | 오퍼레이션 ID 목록 |

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
### **오퍼레이션 단건 조회** { #single-operation-lookup }
> GET "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="single-operation-lookup-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 

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
|   **operation** | **OperationResponseProtocol**| **Yes** | 오퍼레이션       |

##### OperationResponseProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | 앱키  |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |

<a name="getOperationIdByPageable"></a>
<a id="get-all-operation-ids"></a>
### **모든 오퍼레이션 ID 조회** { #get-all-operation-ids }
> GET "/role/v3.0/appkeys/{appKey}/operations/id"

<a id="get-all-operation-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**operationIdPreLike** | **String**| **No** | 오퍼레이션 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.operationId,ASC`)|

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
|   **operationIds** | **List&lt;String>**| **Yes** | 오퍼레이션 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

<a name="postSearchOperation"></a>
<a id="get-operations-list-conditionspaging"></a>
### **오퍼레이션 목록 조회(조건/페이징)** { #get-operations-list-conditionspaging }
> POST "/role/v3.0/appkeys/{appKey}/operations/search"

<a id="get-operations-list-conditionspaging-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.operationId,ASC`)|
| Request Body | **PostSearchOperations.Request** | **PostSearchOperations.Request**| **Yes** |  | |

##### PostSearchOperations.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 오퍼레이션 설명(부분 일치)  |
|   **operationIdPreLike** | **String**| **No** | 오퍼레이션 ID(전방 일치)  |
|   **operationIds** | **List&lt;String>**| **No** | 오퍼레이션 ID 목록(완전 일치)  |

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
|   **operations** | **List&lt;OperationResponseProtocol>**| **Yes** | 오퍼레이션 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### OperationResponseProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | 앱키  |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |

<a name="updateOperation"></a>
<a id="modifying-operations"></a>
### **오퍼레이션 수정** { #modifying-operations }
> PUT "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

<a id="modifying-operations-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 
| Request Body | **UpdateOperation.Request** | **UpdateOperation.Request**| **Yes** |  | |

##### UpdateOperation.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 오퍼레이션 설명  |

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
## 조건 속성 { #condition-attribute }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes**](#createAttribute) | 조건 속성 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#deleteAttribute) | 조건 속성 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes**](#deleteAttributes) | 조건 속성 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#getAttribute) | 조건 속성 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/id**](#searchAttributeIds) | 조건 속성 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/search**](#searchAttributes) | 조건 속성 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#updateAttribute) | 조건 속성 수정 |

<a name="createAttribute"></a>
<a id="create-condition-attribute"></a>
### **조건 속성 생성** { #create-condition-attribute }
> POST "/role/v3.0/appkeys/{appKey}/attributes"

<a id="create-condition-attribute-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateAttribute.Request** | **CreateAttribute.Request**| **Yes** |  | |

##### CreateAttribute.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 조건 속성 설명  |

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
### **조건 속성 삭제** { #delete-condition-attribute }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="delete-condition-attribute-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
|  Query |**forceDelete** | **Boolean**| **No** | 강제 삭제, 기본값(false) |

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
### **조건 속성 다건 삭제** { #delete-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes"

<a id="delete-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**attributeIds** |  **List&lt;String>**| **Yes** | 조건 속성 ID 목록 |
| Request Body |**forceDelete** | **Boolean**| **No** | 강제 삭제, 기본값(false) |

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
### **조건 속성 단건 조회** { #single-lookup-of-condition-attribute }
> GET "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="single-lookup-of-condition-attribute-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 

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
|   **attribute** | **AttributeBundleProtocol**| **Yes** | 조건 속성       |
|   **attributeInUse** | **Boolean**| **Yes** | 조건 속성 사용 여부 |

##### AttributeBundleProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 ID  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### AttributeRoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### AttributeTagProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |

<a name="searchAttributeIds"></a>
<a id="get-a-list-of-condition-attribute-ids"></a>
### **조건 속성 ID 목록 조회** { #get-a-list-of-condition-attribute-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/id"

<a id="get-a-list-of-condition-attribute-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |

##### SearchAttributes.Request

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 조건 속성 설명(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |

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
|   **attributeIds** | **List&lt;String>**| **Yes** | 조건 속성 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

<a name="searchAttributes"></a>
<a id="get-a-list-of-condition-attributes"></a>
### **조건 속성 목록 조회** { #get-a-list-of-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/search"

<a id="get-a-list-of-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |

##### SearchAttributes.Request

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 조건 속성 설명(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |

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
|   **attributes** | **List&lt;AttributeBundleProtocol>**| **Yes** | 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeBundleProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 ID  |
|   **description** | **String**| **No** | 조건 속성 설명  |

##### AttributeRoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### AttributeTagProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |

<a name="updateAttribute"></a>
<a id="modify-condition-attributes"></a>
### **조건 속성 수정** { #modify-condition-attributes }
> PUT "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

<a id="modify-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **UpdateAttribute.Request** | **UpdateAttribute.Request**| **Yes** |  | |

##### UpdateAttribute.Request

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 조건 속성 설명  |

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
## 조건 속성 데이터 타입 { #condition-attribute-data-types }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/data-types**](#getAttributeDataType) | 조건 속성 데이터 타입 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/condition/validate**](#validateConditionValues) | 조건 값 유효성 확인 |

<a name="getAttributeDataType"></a>
<a id="get-condition-attribute-data-types"></a>
### **조건 속성 데이터 타입 목록 조회** { #get-condition-attribute-data-types }
> POST "/role/v3.0/appkeys/{appKey}/attributes/data-types"

<a id="get-condition-attribute-data-types-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 

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
|   **dataTypes** | **List&lt;GetAttributeDataTypeResponse.AttributeDataTypeProtocol>**| **Yes** | 조건 속성 데이터 타입 목록  |

##### GetAttributeDataTypeResponse.AttributeDataTypeProtocol

| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **dataType** | **String**| **Yes** | 조건 속성 데이터 타입        |
|   **operators** | **List&lt;GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol>**| **Yes** | 조건 속성 사용 가능한 연산자 목록 |

##### GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **max** | **Integer**| **Yes** | 연산자가 사용할수 있는 값의 최대 개수  |
|   **min** | **Integer**| **Yes** | 연산자가 사용할수 있는 값의 최소 개수  |
|   **operatorTypeCode** | **String**| **Yes** | 연산자  |

<a name="validateConditionValues"></a>
<a id="validating-condition-values"></a>
### **조건 값 유효성 확인** { #validating-condition-values }
> POST "/role/v3.0/appkeys/{appKey}/attributes/condition/validate"

<a id="validating-condition-values-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **ValidateConditionValuesRequest** | **ValidateConditionValuesRequest**| **Yes** |  | |

##### ValidateConditionValuesRequest

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **Yes** | 역할 조건 속성  |

##### ConditionProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

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
## 조건 속성 역할 연관 관계 { #condition-attribute-role-associations }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#createAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#deleteAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search**](#searchAttributeRoleRelations) | 조건 속성과 연관된 역할 목록 조회 |

<a name="createAttributeRoleRelations"></a>
<a id="create-multiple-roles-associated-with-condition-attributes"></a>
### **조건 속성과 연관된 역할 다건 생성** { #create-multiple-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="create-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **CreateAttributeRoleRelations.Request** | **CreateAttributeRoleRelations.Request**| **Yes** |  | |

##### CreateAttributeRoleRelations.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |

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
### **조건 속성과 연관된 역할 다건 삭제** { #delete-multiple-roles-associated-with-condition-attributes }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

<a id="delete-multiple-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **DeleteAttributeRoleRelations.Request** | **DeleteAttributeRoleRelations.Request**| **Yes** |  | |

##### DeleteAttributeRoleRelations.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |

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
### **조건 속성과 연관된 역할 목록 조회** { #get-roles-associated-with-condition-attributes }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search"

<a id="get-roles-associated-with-condition-attributes-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `attribute.id.attributeId,ASC`)|
| Request Body | **SearchAttributeRoleRelations.Request** | **SearchAttributeRoleRelations.Request**| **Yes** |  | |

##### SearchAttributeRoleRelations.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIdPreLike** | **String**| **No** | 조건 속성과 연관된 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록(완전 일치)  |
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
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성 연관 관계 Role 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeRoleRelationProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

<a id="condition-attribute-tag"></a>
## 조건 속성 태그 { #condition-attribute-tag }

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#createAttributeTags) | 조건 속성 태그 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#deleteAttributeTags) | 조건 속성 태그 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/id**](#searchAttributeTagIds) | 조건 속성 태그 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/search**](#searchAttributeTags) | 조건 속성 태그 목록 조회 |

<a name="createAttributeTags"></a>
<a id="create-condition-attribute-tag"></a>
### **조건 속성 태그 생성** { #create-condition-attribute-tag }
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="create-condition-attribute-tag-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **CreateAttributeTags.Request** | **CreateAttributeTags.Request**| **Yes** |  | |

##### CreateAttributeTags.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |

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
### **조건 속성 태그 삭제** { #delete-condition-attribute-tag }
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

<a id="delete-condition-attribute-tag-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **DeleteAttributeTags.Request** | **DeleteAttributeTags.Request**| **Yes** |  | |

##### DeleteAttributeTags.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |

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
### **조건 속성 태그 ID 목록 조회** { #get-a-list-of-condition-attribute-tag-ids }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/id"

<a id="get-a-list-of-condition-attribute-tag-ids-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTagIds.Request** | **SearchAttributeTagIds.Request**| **Yes** |  | |

##### SearchAttributeTagIds.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIdPreLike** | **String**| **No** | 조건 속성 태그 ID(전방 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |

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
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

<a name="searchAttributeTags"></a>
<a id="get-a-list-of-condition-attribute-tags"></a>
### **조건 속성 태그 목록 조회** { #get-a-list-of-condition-attribute-tags }
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/search"

<a id="get-a-list-of-condition-attribute-tags-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTags.Request** | **SearchAttributeTags.Request**| **Yes** |  | |

##### SearchAttributeTags.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIdPreLike** | **String**| **No** | 조건 속성 태그 ID(전방 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |

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
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeTagProtocol

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |

<a id="settings"></a>
## 설정 { #settings }

| Method | HTTP request                                                        | Description                     |
|------------- |---------------------------------------------------------------------|---------------------------------|
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config/cache-evict**](#deleteCache) | ROLE 서비스 서버와 클라이언트 SDK의 캐시 제거 |
| **GET** | [**/role/v3.0/appkeys/{appKey}/config**](#getConfiguration)         | 설정 조회                           |
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config**](#updateConfig)             | 설정 수정                           |

<a name="deleteCache"></a>
<a id="purge-the-cache-of-the-server-and-client-sdks"></a>
### **서버와 클라이언트 SDK의 캐시 제거** { #purge-the-cache-of-the-server-and-client-sdks }
> PUT "/role/v3.0/appkeys/{appKey}/config/cache-evict"

<a id="purge-the-cache-of-the-server-and-client-sdks-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 

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
### **설정 조회** { #get-settings }
> GET "/role/v3.0/appkeys/{appKey}/config"

<a id="get-settings-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 

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
### **설정 수정** { #modify-settings }
> PUT "/role/v3.0/appkeys/{appKey}/config"

<a id="modify-settings-parameters"></a>
#### Parameters

| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **UpdateConfig.Request** | **UpdateConfig.Request**| **Yes** |  | |

##### UpdateConfig.Request

| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **cacheSize** | **Integer**| **No** | 리소스 ID 기반 인증 캐시 크기  |
|   **cacheSizeByPath** | **Integer**| **No** | 리소스 Hierarchy 조회 캐시 크기  |
|   **cacheSizeTree** | **Integer**| **No** | 리소스 Path 기반 인증 캐시 크기  |
|   **cacheTtl** | **Integer**| **No** |  캐시 데이터 유지 시간(초 단위) |
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
