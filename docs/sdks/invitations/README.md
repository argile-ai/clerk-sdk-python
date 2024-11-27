# Invitations
(*invitations*)

## Overview

Invitations allow you to invite someone to sign up to your application, via email.
<https://clerk.com/docs/authentication/invitations>

### Available Operations

* [create](#create) - Create an invitation
* [list](#list) - List all invitations
* [revoke](#revoke) - Revokes an invitation

## create

Creates a new invitation for the given email address and sends the invitation email.
Keep in mind that you cannot create an invitation if there is already one for the given email address.
Also, trying to create an invitation for an email address that already exists in your application will result to an error.

### Example Usage

```python
from clerk_backend_api import Clerk

with Clerk(
    bearer_auth="<YOUR_BEARER_TOKEN_HERE>",
) as s:
    res = s.invitations.create(email_address="user@example.com", public_metadata={

    }, redirect_url="https://example.com/welcome", notify=True, ignore_existing=True, expires_in_days=486589)

    if res is not None:
        # handle response
        pass

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                              | Type                                                                                                                                                                                                                                                                                   | Required                                                                                                                                                                                                                                                                               | Description                                                                                                                                                                                                                                                                            | Example                                                                                                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `email_address`                                                                                                                                                                                                                                                                        | *str*                                                                                                                                                                                                                                                                                  | :heavy_check_mark:                                                                                                                                                                                                                                                                     | The email address the invitation will be sent to                                                                                                                                                                                                                                       | user@example.com                                                                                                                                                                                                                                                                       |
| `public_metadata`                                                                                                                                                                                                                                                                      | Dict[str, *Any*]                                                                                                                                                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | Metadata that will be attached to the newly created invitation.<br/>The value of this property should be a well-formed JSON object.<br/>Once the user accepts the invitation and signs up, these metadata will end up in the user's public metadata.                                   | {}                                                                                                                                                                                                                                                                                     |
| `redirect_url`                                                                                                                                                                                                                                                                         | *Optional[str]*                                                                                                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | Optional URL which specifies where to redirect the user once they click the invitation link.<br/>This is only required if you have implemented a [custom flow](https://clerk.com/docs/authentication/invitations#custom-flow) and you're not using Clerk Hosted Pages or Clerk Components. | https://example.com/welcome                                                                                                                                                                                                                                                            |
| `notify`                                                                                                                                                                                                                                                                               | *OptionalNullable[bool]*                                                                                                                                                                                                                                                               | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | Optional flag which denotes whether an email invitation should be sent to the given email address.<br/>Defaults to true.                                                                                                                                                               | true                                                                                                                                                                                                                                                                                   |
| `ignore_existing`                                                                                                                                                                                                                                                                      | *OptionalNullable[bool]*                                                                                                                                                                                                                                                               | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | Whether an invitation should be created if there is already an existing invitation for this email address, or it's claimed by another user.                                                                                                                                            | ​false                                                                                                                                                                                                                                                                                 |
| `expires_in_days`                                                                                                                                                                                                                                                                      | *OptionalNullable[int]*                                                                                                                                                                                                                                                                | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | The number of days the invitation will be valid for. By default, the invitation does not expire.                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                        |
| `retries`                                                                                                                                                                                                                                                                              | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                                                                                                     | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                        |

### Response

**[models.Invitation](../../models/invitation.md)**

### Errors

| Error Type         | Status Code        | Content Type       |
| ------------------ | ------------------ | ------------------ |
| models.ClerkErrors | 400, 422           | application/json   |
| models.SDKError    | 4XX, 5XX           | \*/\*              |

## list

Returns all non-revoked invitations for your application, sorted by creation date

### Example Usage

```python
import clerk_backend_api
from clerk_backend_api import Clerk

with Clerk(
    bearer_auth="<YOUR_BEARER_TOKEN_HERE>",
) as s:
    res = s.invitations.list(limit=20, offset=10, status=clerk_backend_api.ListInvitationsQueryParamStatus.PENDING)

    if res is not None:
        # handle response
        pass

```

### Parameters

| Parameter                                                                                                                                 | Type                                                                                                                                      | Required                                                                                                                                  | Description                                                                                                                               | Example                                                                                                                                   |
| ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `limit`                                                                                                                                   | *Optional[int]*                                                                                                                           | :heavy_minus_sign:                                                                                                                        | Applies a limit to the number of results returned.<br/>Can be used for paginating the results together with `offset`.                     | 20                                                                                                                                        |
| `offset`                                                                                                                                  | *Optional[int]*                                                                                                                           | :heavy_minus_sign:                                                                                                                        | Skip the first `offset` results when paginating.<br/>Needs to be an integer greater or equal to zero.<br/>To be used in conjunction with `limit`. | 10                                                                                                                                        |
| `status`                                                                                                                                  | [Optional[models.ListInvitationsQueryParamStatus]](../../models/listinvitationsqueryparamstatus.md)                                       | :heavy_minus_sign:                                                                                                                        | Filter invitations based on their status                                                                                                  | pending                                                                                                                                   |
| `retries`                                                                                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                          | :heavy_minus_sign:                                                                                                                        | Configuration to override the default retry behavior of the client.                                                                       |                                                                                                                                           |

### Response

**[List[models.Invitation]](../../models/.md)**

### Errors

| Error Type      | Status Code     | Content Type    |
| --------------- | --------------- | --------------- |
| models.SDKError | 4XX, 5XX        | \*/\*           |

## revoke

Revokes the given invitation.
Revoking an invitation will prevent the user from using the invitation link that was sent to them.
However, it doesn't prevent the user from signing up if they follow the sign up flow.
Only active (i.e. non-revoked) invitations can be revoked.

### Example Usage

```python
from clerk_backend_api import Clerk

with Clerk(
    bearer_auth="<YOUR_BEARER_TOKEN_HERE>",
) as s:
    res = s.invitations.revoke(invitation_id="inv_123")

    if res is not None:
        # handle response
        pass

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         | Example                                                             |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `invitation_id`                                                     | *str*                                                               | :heavy_check_mark:                                                  | The ID of the invitation to be revoked                              | inv_123                                                             |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |                                                                     |

### Response

**[models.InvitationRevoked](../../models/invitationrevoked.md)**

### Errors

| Error Type         | Status Code        | Content Type       |
| ------------------ | ------------------ | ------------------ |
| models.ClerkErrors | 400, 404           | application/json   |
| models.SDKError    | 4XX, 5XX           | \*/\*              |