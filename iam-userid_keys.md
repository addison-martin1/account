---

copyright:

  years: 2015, 2021
lastupdated: "2021-04-13"

keywords: API key, user API keys, IBM Cloud API keys, manage user keys, create API key

subcollection: account

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:note: .note}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'}

# Managing user API keys
{: #userapikey}

A federated user or non-federated user can create an API key to use in the CLI or as part of automation to log in as your user identity. You can use the console, CLI, or API to manage your {{site.data.keyword.cloud}} API keys by listing your keys, creating keys, updating keys, or deleting keys. 
{: shortdesc}

The API key inherits all assigned access for the user identity for which it is created, and the access is not limited to just the account where the API key is created because it inherits any policies assigned to the user. So, if the user has access to resources from multiple accounts, then the API key inherits the access from all accounts. Therefore, it is possible that a user's API key can be used to generate a token and access resources that a user has access to outside of the account where the API key was created. 

Because the API key associated with your user identity has all of the access you're entitled to across any account that you are a member of, you must be cautious with how you use your API key. For example, an {{site.data.keyword.cloud_notm}} service might need to act on behalf of a user or access services that are not IAM-enabled, so the service might request a user API key. In these cases, it is recommended that you create an API key associated with a functional ID that is assigned the minimum level of access that is required to work with the service. 

A functional ID is a user ID created to represent a program, application, or service. The functional ID can be invited to an account and assigned only the access for a particular purpose, such as interacting with a specific resource or application. The functional ID should be granted only the minimum level access in a single account that is needed for the specific function which it was created.


## Managing user API keys
{: #manage-user-keys}
{: ui}

To manage the {{site.data.keyword.Bluemix_notm}} API keys that are associated with your user identity or the ones that you have access to manage for other users in the account, go to **Manage** &gt; **Access (IAM)** &gt; **API keys** in the console. On the API keys page, you can create, edit, or delete {{site.data.keyword.cloud_notm}} API keys for yourself, and you can manage all [classic infrastructure API keys](/docs/account?topic=account-classic_keys) for users that you are an ancestor of in the user hierarchy. In addition, if you are the account owner or a user assigned the required access to manage other user's API keys in the account, you can use the **View** filter to list and manage those API keys too.

| Filter Options                                     | Displayed API Keys                                         | Required Access                            | Allowed Actions            |
|----------------------------------------------------|------------------------------------------------------------|--------------------------------------------|----------------------------|
| My {{site.data.keyword.cloud_notm}} API keys       | Your IBM Cloud API keys                                    | No access required                         | View, create, edit, delete |
| All user {{site.data.keyword.cloud_notm}} API keys | All IBM Cloud API keys created by all users in the account | Administrator role on IAM Identity service | View, edit, and delete     |
{: caption="Table 1. Required access for API key management on the API keys page" caption-side="top"}

## Creating an API key
{: #create_user_key}
{: help}
{: support}
{: ui}

As an {{site.data.keyword.Bluemix_notm}} user you might want to use an API key when you enable a program or script without distributing your password to the script. A benefit of using an API key can be that a user or organization can create several API keys for different programs and the API keys can be deleted independently if compromised without interfering with other API keys or even the user. You can create up to 20 API keys.

To create an API key for your user identity in the UI, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage** &gt; **Access (IAM)** &gt; **API keys**.
2. Click **Create an {{site.data.keyword.Bluemix_notm}} API key**.
3. Enter a name and description for your API key.
4. Click **Create**.
5. Then, click **Show** to display the API key. Or, click **Copy** to copy and save it for later, or click **Download**.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: tip}


## Creating an API key using the CLI
{: #create_user_key-cli}
{: cli}

To create an API key by using the CLI, use the following command:

1. Enter `ibmcloud iam api-key-create NAME [-d DESCRIPTION] [-f, --file FILE]` in your command prompt, and specify a name, description, and file for saving your key. See the following example:

```
ibmcloud iam api-key-create MyKey -d "this is my API key" --file key_file
```

## Creating an API key by using the API
{: #create_user_key-api}
{: api}

To create an API key, call the [IAM Identity Service API](https://cloud.ibm.com/apidocs/iam-identity-token-api?code=go#create-api-key){: external} as shown in the following example: 

```bash
curl -X POST 'https://iam.cloud.ibm.com/v1/apikeys' -H 'Authorization: Bearer TOKEN' -H 'Content-Type: application/json' -d '{
  "name": "My-apikey",
  "description": "my personal key",
  "iam_id": "IBMid-123WEREW",
  "account_id": "ACCOUNT_ID"
  "store_value": false
}'
```
{: codeblock}
{: curl}

```java
CreateApiKeyOptions createApiKeyOptions = new CreateApiKeyOptions.Builder()
    .name(apiKeyName)
    .iamId(iamId)
    .description("Example ApiKey")
    .build();

Response<ApiKey> response = service.createApiKey(createApiKeyOptions).execute();
ApiKey apiKey = response.getResult();
apikeyId = apiKey.getId();
System.out.println(apiKey.toString());
```
{: codeblock}
{: java}

```javascript
const params = {
  name: apikeyName,
  iamId: iamId,
  description: 'Example ApiKey',
};

iamIdentityService.createApiKey(params)
  .then(res => {
    apikeyId = res.result.id
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err);
  });
```
{: codeblock}
{: javascript}

```python
api_key = iam_identity_service.create_api_key(
  name=apikey_name,
  iam_id=iam_id
).get_result()

apikey_id = api_key['id']

print(json.dumps(api_key, indent=2))
```
{: codeblock}
{: python}

```go
createAPIKeyOptions := iamIdentityService.NewCreateAPIKeyOptions(apikeyName, iamID)
createAPIKeyOptions.SetDescription("Example ApiKey")

apiKey, response, err := iamIdentityService.CreateAPIKey(createAPIKeyOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(apiKey, "", "  ")
fmt.Println(string(b))
apikeyID = *apiKey.ID
```
{: codeblock}
{: go}

## Updating an API key
{: #update_user_key}
{: ui}

If you want to change the name or the description of an API key, complete the following steps in the UI or CLI.

To edit an API key, complete the following steps:

1. In the console, go to **Manage** &gt; **Access (IAM)** &gt; **API keys**.
2. Identify the row of the API key that you want to update, and select **Edit** from the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu.
3. Update the information for your API key.
4. Click **Apply**.

To edit an API key that is not your own, but you have access to manage, go to the API keys page. Then, select the **All user {{site.data.keyword.cloud_notm}} API keys** option from the **View** menu to find the API key.
{: tip}


## Updating an API key using the CLI
{: #update_user_key-cli}
{: cli}

To edit an API key by using the CLI, enter the following command:

1. Enter `ibmcloud iam api-key-update NAME [-n NAME] [-d DESCRIPTION]` in your command prompt, specifying the old name, new name, and new description for the key. For example:

```
ibmcloud iam api-key-update MyCurrentName -n MyNewName -d "the new description of my key"
```

## Updating an API key by using the API
{: #update_user_key-api}
{: api}

To edit an API key by using the API, call the [IAM Identity Service API](https://cloud.ibm.com/apidocs/iam-identity-token-api?code=go#update-api-key){: external} as shown in the following example: 

```bash
curl -X PUT 'https://iam.cloud.ibm.com/v1/apikeys/APIKEY_UNIQUE_ID' -H 'Authorization: Bearer TOKEN' -H 'If-Match: <value of etag header from GET request>' -H 'Content-Type: application/json' -d '{
  "name": "My-apikey",
  "description": "my personal key"
}'
```
{: codeblock}
{: curl}

```java
UpdateApiKeyOptions updateApiKeyOptions = new UpdateApiKeyOptions.Builder()
    .id(apikeyId)
    .ifMatch(apikeyEtag)
    .description("This is an updated description")
    .build();

Response<ApiKey> response = service.updateApiKey(updateApiKeyOptions).execute();
ApiKey apiKey = response.getResult();
System.out.println(apiKey.toString());
```
{: codeblock}
{: java}

```javascript
const params = {
  id: apikeyId,
  ifMatch: apikeyEtag,
  description: 'This is an updated description',
};

iamIdentityService.updateApiKey(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err);
  });
```
{: codeblock}
{: javascript}

```python
api_key = iam_identity_service.update_api_key(
  id=apikey_id,
  if_match=apikey_etag,
  description='This is an updated description'
).get_result()

print(json.dumps(api_key, indent=2))
```
{: codeblock}
{: python}

```go
updateAPIKeyOptions := iamIdentityService.NewUpdateAPIKeyOptions(apikeyID, apikeyEtag)
updateAPIKeyOptions.SetDescription("This is an updated description")

apiKey, response, err := iamIdentityService.UpdateAPIKey(updateAPIKeyOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(apiKey, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

## Locking an API key
{: #lock_user_key}

For platform API keys that represent your user identity you can prevent the API key from being deleted by locking it. A locked API key is indicated by the ![Locked icon](images/locked.svg "Locked") icon. 

### Locking and unlocking an API key from the UI
{: #lockui}
{: ui}

1. In the console, go to **Manage** &gt; **Access (IAM)** &gt; **API keys**.
2. Identify the row of the API key that you want to lock, and select **Lock** from the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu.

You can unlock your API key at any time to update or remove the API key from your account. Select the API key from the table that you want to unlock and select **Unlock** from the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu.
{: tip}

### Locking and unlocking an API key by using the CLI
{: #lockcli}
{: cli}

To lock an API key, use the following command:

```
ibmcloud iam api-key-lock (NAME|UUID) [-f, --force]
```

<strong>Prerequisites</strong>:  Endpoint, Login

<strong>Command options</strong>:
<dl>
<dt>NAME (required)</dt>
<dd>Name of the API key to be locked, exclusive with UUID.</dd>
<dt>UUID (required)</dt>
<dd>UUID of the API key to be locked, exclusive with NAME.</dd>
<dt>-f, --force</dt>
<dd>Forces lock without confirmation.</dd>
</dl>

<strong>Example</strong>:

Lock API key `test-api-key`

```
ibmcloud iam api-key-lock test-api-key
```

To unlock an API key, run the following command:

```
ibmcloud iam api-key-unlock (NAME|UUID) [-f, --force]
```

<strong>Prerequisites</strong>:  Endpoint, Login

<strong>Command options</strong>:
<dl>
<dt>NAME (required)</dt>
<dd>Name of the API key to be unlocked, exclusive with UUID.</dd>
<dt>UUID (required)</dt>
<dd>UUID of the API key to be unlocked, exclusive with NAME.</dd>
<dt>-f, --force</dt>
<dd>Forces unlock without confirmation.</dd>
</dl>

<strong>Example</strong>:

Unlock API key `test-api-key`

```
ibmcloud iam api-key-unlock test-api-key
```

## Locking and unlocking an API key by using the API
{: #lock-user-key-api}
{: api}

For platform API keys that represent your user identity you can prevent the API key from being deleted by locking it. 

### Locking an API key
{: lock-keyapi}
{: api}

To lock an API key by using the API, call the [IAM Identity Service API](https://cloud.ibm.com/apidocs/iam-identity-token-api?code=go#lock-api-key){: external} as shown in the following example: 

```bash
curl -X POST 'https://iam.cloud.ibm.com/v1/apikeys/APIKEY_UNIQUE_ID/lock' -H 'Authorization: Bearer TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```java
LockApiKeyOptions lockApiKeyOptions = new LockApiKeyOptions.Builder()
    .id(apikeyId)
    .build();

service.lockApiKey(lockApiKeyOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
  id: apikeyId,
};

iamIdentityService.lockApiKey(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err);
  });
```
{: codeblock}
{: javascript}

```python
lock_api_key(self,
        id: str,
        **kwargs
    ) -> DetailedResponse

response = iam_identity_service.lock_api_key(id=apikey_id)

print(response)
```
{: codeblock}
{: python}

```go
lockAPIKeyOptions := iamIdentityService.NewLockAPIKeyOptions(apikeyID)

response, err := iamIdentityService.LockAPIKey(lockAPIKeyOptions)
if err != nil {
  panic(err)
}
```
{: codeblock}
{: go}

### Unlocking an API key
{: unlock-keyapi}
{: api}

To unlock an API key by using the API, call the [IAM Identity Service API](https://cloud.ibm.com/apidocs/iam-identity-token-api?code=go#unlock-api-key){: external} as shown in the following example: 

```bash
curl -X DELETE 'https://iam.cloud.ibm.com/v1/apikeys/APIKEY_UNIQUE_ID/lock' -H 'Authorization: Bearer TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```java
UnlockApiKeyOptions unlockApiKeyOptions = new UnlockApiKeyOptions.Builder()
    .id(apikeyId)
    .build();

service.unlockApiKey(unlockApiKeyOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
  id: apikeyId,
};

iamIdentityService.unlockApiKey(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err);
  });
```
{: codeblock}
{: javascript}

```python
response = iam_identity_service.unlock_api_key(id=apikey_id)

print(response)
```
{: codeblock}
{: python}

```go
unlockAPIKeyOptions := iamIdentityService.NewUnlockAPIKeyOptions(apikeyID)

response, err := iamIdentityService.UnlockAPIKey(unlockAPIKeyOptions)
if err != nil {
  panic(err)
}
```
{: codeblock}
{: go}

## Deleting an API key
{: #delete_user_key}
{: ui}

If you are using a key rotation strategy, you might want to delete an older key and replace it with a new key.

To delete an API key, complete the following steps:

1. In the console, go to **Manage** &gt; **Access (IAM)** &gt; **API keys**.
2. Identify the row of the API key that you want to delete, and select **Delete** from the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu.
3. Then, confirm the deletion by clicking **Delete**.

To delete an API key that is not your own, but you have access to manage, go to the API keys page. Then, select the **All user {{site.data.keyword.cloud_notm}} API keys** option from the **View** menu to find the API key.
{: tip}

## Deleting an API key using the CLI
{: #delete_user_key-cli}
{: cli}

To delete an API key by using the CLI:

Enter `ibmcloud iam api-key-delete NAME` in your command prompt, specifying the name of the key to delete.

## Deleting an API key using the API
{: #delete_user_key-api}
{: api}

To delete an API key by using the API, call the [IAM Identity Service API](https://cloud.ibm.com/apidocs/iam-identity-token-api#delete-api-key){: external} as shown in the following example: 

```bash
curl -X DELETE 'https://iam.cloud.ibm.com/v1/apikeys/APIKEY_UNIQUE_ID' -H 'Authorization: Bearer TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```java
DeleteApiKeyOptions deleteApiKeyOptions = new DeleteApiKeyOptions.Builder()
    .id(apikeyId)
    .build();

service.deleteApiKey(deleteApiKeyOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
  id: apikeyId,
};

iamIdentityService.deleteApiKey(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err);
  });
```
{: codeblock}
{: javascript}

```python
delete_api_key(self,
        id: str,
        **kwargs
    ) -> DetailedResponse

response = iam_identity_service.delete_api_key(id=apikey_id)

print(response)
```
{: codeblock}
{: python}

```go
deleteAPIKeyOptions := iamIdentityService.NewDeleteAPIKeyOptions(apikeyID)

response, err := iamIdentityService.DeleteAPIKey(deleteAPIKeyOptions)
if err != nil {
  panic(err)
}
```
{: codeblock}
{: go}
