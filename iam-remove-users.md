---

copyright:

  years: 2018, 2021

lastupdated: "2021-04-13"

keywords: remove user, delete user

subcollection: account

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'}

# Removing users from an account
{: #remove}

When you remove a user from an account, the user can no longer log in to the console, switch to your account, or access account resources. Removing a user from an account doesn't delete the IBMid for the user.
{:shortdesc}

Only account owners or users with the following access can remove users from an account:

* An Identity and access management (IAM) policy for the User management account management service with the Administrator role assigned and be the Cloud Foundry org manager if the user belongs to a Cloud Foundry org.
* If you have classic infrastructure in your account, a user must have an IAM policy for the User management account management service with the Administrator role assigned, be the Cloud Foundry org manager if the user belongs to a Cloud Foundry org, and be an ancestor of the user in the classic infrastructure user hierarchy with the Manage user classic infrastructure permission assigned.

Only users with the correct access can remove others. If you use a service ID to authenticate, you can't remove users from the account.
{: note}

## Removing a user from an account in the console
{: remove-user-acount-ui}
{: ui}

To remove a user from an account, complete the following steps:

1. In the {{site.data.keyword.cloud}} console, click **Manage** &gt; **Access (IAM)**, and select **Users**.
2. From the row of the user that you want to remove, select **Remove user** from the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu.

Any resources that are created by the user remain in the account, but any {{site.data.keyword.cloud_notm}} API keys that the user created are removed. The user no longer has access to work with the resources they created. The account owner or an administrator for the service or service instance can assign other users to work with the resources, or delete them from the account.

If you get an error message that states a classic infrastructure user can't be removed, make sure that any descendants in the user hierarchy for that user are [assigned a new parent](/docs/account?topic=account-iam-user-setting#update-parent), [disabled in the account](/docs/account?topic=account-status), or deleted. Then, you can try again.
{: tip}

As an alternative to removing a user from your account, you can move the user to a suspended state. The suspended state is used as a temporary way of restricting access to account resources. The user can log in to the console and view the account in their account list, but can no longer access resources in the account. All information and access policies that are associated with the account are saved. For more information, see [Updating a user's status](/docs/account?topic=account-status).

## Removing a user from an account by using the CLI
{: remove-user-acount-cli}
{: cli}

To remove a user from an account, run the following command:

```
ibmcloud account user-remove USER_ID [-c ACCOUNT_ID] [-f, --force]
```
{: codeblock}

For command options, see [Managing accounts, users, and Cloud Foundry orgs](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_account#ibmcloud_account_user_remove) CLI commands. 

## Removing a user from an account by using the API
{: remove-user-acount-api}
{: api}

To remove a user from an account, call the [User Management API](https://cloud.ibm.com/apidocs/user-management?code=java#remove-user) as shown in the following sample request. Replace variables with the user's IAM ID. You must use a user token for authorization. Service IDs can't remove users from an account.

```bash
curl -X DELETE https://user-management.cloud.ibm.com/v2/accounts/987d4cfd77b04e9b9e1a6asdcc861234/users/IBMid-1000000000 -H 'Authorization: Bearer <IAM_TOKEN>' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```java
RemoveUserOptions removeUserOptions = new RemoveUserOptions.Builder()
  .accountId(accountId)
  .iamId(deleteUserId)
  .build();

service.removeUser(removeUserOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
  accountId: accountId,
  iamId: deleteUserId,
};

userManagementAdminService.removeUser(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
response = user_management_admin_service.remove_user(
  account_id=account_id,
  iam_id=delete_user_id,
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
removeUserOptions := userManagementService.NewRemoveUserOptions(
  accountID,
  deleteUserID,
)

response, err := userManagementAdminService.RemoveUser(removeUserOptions)
if err != nil {
  panic(err)
}
```
{: codeblock}
{: go}
