<!-- pre-align:aligned sig=cbece13ec7df -->

<a id="application-service-role-release-note"></a>
## Application Service > ROLE > Release note { #application-service-role-release-note }

<a id="04-28"></a>
## 2026. 04. 28. { #04-28 }
<a id="04-28-1"></a>
### 기능 추가 { #04-28-1 }
* [RESTful API] 특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록을 조회하는 API가 추가되었습니다.
    * POST /role/v3.0/appkeys/{appKey}/roles/{roleId}/containing-roles/search
        * 자세한 사항은 매뉴얼 참고: [링크](./api-v3-guide/#searchContainingRoles)
* [RESTful API] 역할에서 설정 가능한 모든 조건 속성 목록 조회 API 응답에 `attributeTagIds`(조건 속성 태그 ID 목록) 필드가 추가되었습니다.
    * POST /role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search
        * 자세한 사항은 매뉴얼 참고: [링크](./api-v3-guide/#searchAttributesByRoleId)
* [SDK] 2.0.7로 릴리스되었습니다.
    * 신규 API(특정 역할의 하위 역할/권한을 모두 포함하는 역할 목록 조회)가 반영되었습니다.

<a id="april-23-2024"></a>
## April 23, 2024 { #april-23-2024 }
<a id="added-features"></a>
### Added Features { #added-features }
* [RESTful API] Extended The APIs to retrieve role lists and a single role.
  * Added role tag lists to the list of related roles.
    * POST /role/v3.0/appkeys/{appKey}/roles/search: Retrieve roles
        * For more details, see: [link](./api-v3-guide/#searchRoles)
    * GET /role/v3.0/appkeys/{appKey}/roles/{roleId}: Retrieve a role
        * For more details, see: [link](./api-v3-guide/#getRole)

<a id="bug-fixes"></a>
### Bug Fixes { #bug-fixes }
* [RESTful API] Fixed an error where the roleApplyPolicyCode (role enabled or disabled) entry would not be reflected when requesting the Create role and Modify role API.
* [RESTful API] Fixed an error where validations of some conditions (role condition attributes) failed when requesting the Create role and Modify role API.

<a id="march-26-2024"></a>
## March 26, 2024 { #march-26-2024 }
<a id="march-26-2024-added-features"></a>
### Added Features { #march-26-2024-added-features }
* [RESTful API] Changed the Get a list of users API.
    * POST /role/v3.0/appkeys/{appKey}/users/search: Get a list of users
        * For more information, see: [link](./api-v3-guide/#get-a-list-of-users)

<a id="january-23-2024"></a>
## January 23, 2024. { #january-23-2024 }
<a id="january-23-2024-added-features"></a>
### Added Features { #january-23-2024-added-features }
* Added attribute-based access control (ABAC) feature. 
  * [Console] Applied a new design and added condition attribute feature.
      * You can add the condition attribute feature to a role.
      * Added the 'Condition attribute' tab.
      * Added the ‘Manage’ tab.
  * [RESTFUL API] Public API v3 has been launched.
      * Provides role-based access control (RBAC) and attribute-based access control (ABAC) for roles.
      * Allows for diverse and fine-grained user access control by enabling specific roles or conditions.
  * [SDK] 2.0.0 was released.
      * Applied to v3 newly provided by RESTFUL API.

<a id="end-of-support"></a>
### End of Support { #end-of-support }
* [Console] Excel upload/Excel download feature is not supported.
* [RESTFUL API] Public API v1 does not support the validation period setting feature when giving Role to User.
	* This feature is provided by attribute-based access control (ABAC).
* [SDK] The 1.x version does not support the validation period setting feature when giving Role to User.
	* This feature is provided by attribute-based access control (ABAC).

<a id="september-26-2023"></a>
## September 26, 2023 { #september-26-2023 }
<a id="feature-updates"></a>
### Feature Updates { #feature-updates }
* Role ID naming rules changed when creating a role.
    * The maximum number of characters in RoleID has increased from 32 to 128.
    * Special characters `.` and `:` have also been added to allow, and previously only allowed `_` and `-`.

<a id="november-26-2019"></a>
## November 26, 2019 { #november-26-2019 }
<a id="november-26-2019-added-features"></a>
### Added Features { #november-26-2019-added-features }
* When granting Role to User, you can set the validation period.
    * Role granted to the User is valid only within the validation period that you set; conversely, the privileges granted are always valid unless set. 
    * The validation period can be set in 'day' unit. 
    * Start date of the validation period can be set beyond the current point, and the permissions granted are valid from the date you set.
    * The validation period can only be set to the start date, in which case the granted permissions are always valid from the set date.
* You can set the order of exposure, tag in the role.
    * When searching for a role list, it sorts in the order of exposure.
    * Multiple tags are configurable and can be used as search keywords. 

<a id="june-25-2019"></a>
## June 25, 2019 { #june-25-2019 }
<a id="june-25-2019-added-features"></a>
### Added Features { #june-25-2019-added-features }
* [Console] You can set the Trailing Slash setting for Resource Path.
    * When setting up Non-Identical Path, "/admin" and "/admin/" are recognized as different paths.
    * When setting up Identical Path, "/admin" and "/admin/" are recognized as the same path.

<a id="february-22-2018"></a>
## February 22, 2018 { #february-22-2018 }
<a id="february-22-2018-added-features"></a>
### Added Features { #february-22-2018-added-features }
* [Console] Among the resource entries, path supports antiPathPattern. 
    * When setting "/admin/\*\*", you can support authorization checks with the resource path under admin.
* [Console] For ease of management, RoleName and RoleGroup have been added among Role items.
    * RoleName: You can give the Role a meaningful name to manage it.
    * RoleGroup: You can specify a group and manage it through a group-by-group search.
* [Console] Resource ID of the resource has been increased to 64 characters in length.
* [RESTFUL API] Among the Role entries, RoleName and RoleGroup have been added to extend the Role-related API.
    * For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#role)
* [SDK] Released as 1.1.7 .
    * Commons-collection 3.2.2 was applied to enhance security.
    

<a id="01"></a>
## 1.0.1 { #01 }
<a id="01-added-features"></a>
### Added Features { #01-added-features }
* [RESTFUL API] Added the API to look up the list of each component.
	* GET /role/v1.0/appkeys/{appKey}/roles: role list look up
		* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#role)
	* GET /role/v1.0/appkeys/{appKey}/resources: resource list lookup
		* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#resource)
	* GET /role/v1.0/appkeys/{appKey}/scopes: scope list lookup
		* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#scope)
	* GET /role/v1.0/appkeys/{appKey}/operations: operation list lookup
		* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#operation)

<a id="01-feature-updates"></a>
### Feature Updates { #01-feature-updates }
* [Console] You can enter Korean in Resource name. All characters can be entered except '/' characters.
* [Console] When entering and correcting Resource, Role, User, Scope, the message that shows when field validation fails has been modified.
	* The resource priority, description, has been improved to show phrases other than 4XX and 5XX errors when validating them.
		* Priority validation failure statement: 'Priority can only be entered with numbers (-32768 to 32767)'
		* description validation failed phrase: 'This is an invalid type of description.'
	* If the description of Scope, Role, and User is more than 128 characters, it has been improved to show phrases other than 4XX and 5XX errors.
		* description validation failed phrase: 'This is an invalid type of description.'
	* If you enter the Role or Scope that is not in the User fix screen, it has been improved to show the phrase other than the 11001 or 13001 error.
		* When entering a missing Scope, phrase: 'Scope ID not found'
		* When entering a missing Role, phrase: 'Role ID not found'
* [Console] Added the auto complete feature to the Operation field in Resource search screen.
* [Console] To prevent misuse of the Migration feature, a cautionary note has been added to the screen.
	* a cautionary note: '※ caution: Copy the current project's Resources, Role, Operation to the selected project. Delete existing Resources, Role, Operation for the selected project.'
* [RESTFUL API] API constraints has changed.
	* GET /role/v1.0/appkeys/{appKey}/resources/hierarchy The API has been changed to give full results without having to give users or roles as factors..
		* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#resource)

<a id="01-bug-fixes"></a>
### Bug Fixes { #01-bug-fixes }
* [Console] On Resource Modification screen, an error that causes a 5XX error when changing the name of a resource with a sub-resource has been fixed. 
	* The normally changed UiPath is also reflected in the child resource. 
* [Console]  Fixed an error in the Resource search screen that caused all resources to be searched when searching for a user that does not exist. 
	* There are no resources shown in Resource Tree.
* [Console] Fixed an error when modifying a resource with a duplicate name that appeared to be the value applied when canceling after a modification request. 
	* It appears to be in the previous state.
* [Console]  Fixed an error in which the Total value was not changed properly when searching with the keyword description on the Role search screen.
	* The total value is reflected as the number of retrieved values.
* [Console] Fixed an error in which adding or deleting a role while being searched on the user screen was not reflected in the immediately searched list screen.
	* It is immediately reflected in the User List screen.
* [Console] On User Modification screen, the title has been modified to appear from 'Add User' to 'Modify User'.
 

<a id="july-20-2017"></a>
## July 20, 2017 { #july-20-2017 }
<a id="july-20-2017-bug-fixes"></a>
### Bug Fixes { #july-20-2017-bug-fixes }
* [Console] Failure warning window is displayed on the screen without reflecting when registering/modifying to the names of Resource, Role, and Scope that are already in use.  
	
<a id="may-25-2017"></a>
## May 25, 2017 { #may-25-2017 }
<a id="may-25-2017-bug-fixes"></a>
### Bug Fixes { #may-25-2017-bug-fixes }
* [Console] Fixed an issue in which [ Excel Upload] feature on the Resource tab does not work 

<a id="april-20-2017"></a>
## April 20, 2017 { #april-20-2017 }
<a id="april-20-2017-bug-fixes"></a>
### Bug Fixes { #april-20-2017-bug-fixes }
* Fixed a bug that returns error if value of userId(key) contains '.', '@'

<a id="december-22-2016"></a>
## December 22, 2016 { #december-22-2016 }
<a id="december-22-2016-feature-updates"></a>
### Feature Updates { #december-22-2016-feature-updates }
* Added bulk user list lookup API
* Added the association lookup API linked to Scope

<a id="december-22-2016-bug-fixes"></a>
### Bug Fixes { #december-22-2016-bug-fixes }
* Fixed an issue with role associations not functioning properly
* Fixed an issue where ScopeID ALL is not detected when searching for a user
* Fixed a bug that caused the hierarchy tree to return abnormally if there was an invalid resource tree

<a id="01-2"></a>
## 1.0.1 { #01-2 }
<a id="01-2-feature-updates"></a>
### Feature Updates { #01-2-feature-updates }
* Added a feature to remove Cache from Client SDK and servers

<a id="01-2-bug-fixes"></a>
### Bug Fixes { #01-2-bug-fixes }
* Modified Resource Path so that it cannot be modified to an invalid path that does not start with '/' when modifying it
	* Data input using Excel may not be possible if there is an incorrect path

<a id="01-3"></a>
## 1.0.1 { #01-3 }
<a id="01-3-feature-updates"></a>
### Feature Updates { #01-3-feature-updates }
* Added the option to return users with associated Role when viewing user lists
	* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#user)

<a id="01-4"></a>
## 1.0.1 { #01-4 }
<a id="01-4-feature-updates"></a>
### Feature Updates { #01-4-feature-updates }
* Added API to delete existing registered roles with the same scope when granting a new role to a user
* Added a User to a Role Add an option to create a User if it doesn't exist in the API
	* For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/api-guide/#role)

<a id="01-5"></a>
## 1.0.1 { #01-5 }
<a id="01-5-feature-updates"></a>
### Feature Updates { #01-5-feature-updates }
* Polling API support is deprecated due to low usability
* Added feature to migrate data between projects using Role products
    * For more information, refer to the manual: [link](/ko/Application%20Service/ROLE/ko/console-guide/#migration)

<a id="01-5-bug-fixes"></a>
### Bug Fixes { #01-5-bug-fixes }
* Fixed a bug that when you deleted a Role, but the association information for another Role was deleted
* Fixed a bug that causes intermittent permission checks to fail
