# API Access Management Inline Hook

This page provides reference documeentation for the API Access Management hook, covering the JSON objects contained in the outbound request from Okta to your external service, as well as the JSON objects that your external service can return to Okta in the response.

## See Also

For a general introduction Okta inline hooks, see [Inline Hooks](/use_cases/hooks/).

For setup steps for the API Access Management hook, see [API Access Management Setup](/use_cases/hooks/setup/api-am-token-hook-setup.md).

For reference documentation for the API for registering external service endpoints with Okta, see [Callbacks API](/api/resources/callbacks).

## Objects in the Request from Okta

### data.tokens.access_token

Provides information on the properties of the access token that Okta has generated, including the existing claims it contains.

| Property | Description                               | Data Type                    |
|----------|-------------------------------------------|------------------------------|
| claims   | Claims included in the token.             | [claims](#claims) object     |
| lifetime | Lifetime of the token                     | [lifetime](#lifetime) object |
| scopes   | The scopes contained in the access token. | [scopes](#scopes) object     |

#### claims

| Property   | Description | Data Type |
|------------|-------------|-----------|
| ver        |             |           |
| jti        |             |           |
| iss        |             |           |
| aud        |             |           |
| cid        |             |           |
| uid        |             |           |
| sub        |             |           |
| patientId  |             |           |
| appProfile |             |           |
| claim1     |             |           |

#### lifetime

| Property | Description | Data Type |
| ---      | ---         | ---       |
| expiration |           | Number |

#### scopes

| Property | Description | Data Type |
| ---      | ---         | ---       |
|


### data.tokens.id_token

Provides information on the properties of the ID token that Okta has generated, including the existing claims it contains.

| Property | Description                   | Data Type                    |
|----------|-------------------------------|------------------------------|
| claims   | Claims included in the token. | [claims](#claims) object     |
| lifetime | Lifetime of the token         | [lifetime](#lifetime) object |

## Objects in Your Response to Okta

### commands

The `commands` object is where you can provide commands to Okta, to cause Okta to augment the claims in the token.

The `commands` object is an array. In each array element, there needs to be a `type` property and `value` property. The `type` property is where you specify which of the supported commands you wish to execute; `value` is where you supply an operand. In the case of the the API Access Managment hook type, the `value` property is itself a nested object, in which you specify a particular operation, a path to act on, and a value.

| Property | Description                                                              | Data Type              |
|----------|--------------------------------------------------------------------------|------------------------|
| type     | The name of one of the [supported commands](#list-of-supported-commands) | String                 |
| value    | Lifetime of the token                                                    | [value](#value) object |


#### List of Supported Commands

The following commands are supported for the API Access Management inline hook type:

| Command                            | Description             | 
|------------------------------------|-------------------------|
| com.okta.tokens.id_token.patch     | Modify an ID token.     | 
| com.okta.tokens.access_token.patch | Modify an access token. |

#### value

| Property | Description                                                                                                                                                                                                       | Data Type |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| op       | The operation to perform on the claims included in the token. Currently, only `add` is supported.                                                                                                                 | String    |
| path     | Location within the token to apply the operation, specified as a slash-delimited path. When adding a claim, this will always begin with `/claims/`,  and be followed by the name of the new claim you are adding. | String    |
| value    | Value to set the claim to. | Number    |

#### Sample Listing

"commands":
[{
    "type": "com.okta.tokens.id_token.patch",
    "value":
    [
        {
        "op": "add",
        "path": "/claims/extPatientId",
        "value": "1234"
        }
    ]
    },
    {
    "type": "com.okta.tokens.access_token.patch",
    "value":


    [
        {
        "op": "add",
        "path": "/claims/external_guid",
        "value": "F0384685-F87D-474B-848D-2058AC5655A7"
        }
    ]
    }
]





data.context.request
data.context.protocol
data.context.session
data.context.user
data.context.policy





{
	"eventTypeVersion": "1.0",
	"cloudEventVersion": "0.1",
	"eventType": "com.okta.tokens.transform",
	"contentType": "application/json",
	"source": "https://gronberg.oktapreview.com/oauth2/ausar8buzjFYEyAf00h7/v1/authorize",
	"eventId": "ue59hs25STaP4r0EVDK30A",
	"eventTime": "2018-11-07T04:32:50.000Z",
	"data": {
		"context": {
			"request": {
				"id": "W@Jq8nDC85a0QjAcA7zG8gAABU0",
				"method": "GET",
				"url": {
					"value": "https://gronberg.oktapreview.com/oauth2/ausar8buzjFYEyAf00h7/v1/authorize?scope=openid+profile+email+address+phone+providers%3Aread&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%3A8080&state=myState&nonce=fc2422c5-dd72-469d-af25-23ad33052ec6&client_id=ULx7dd5LbNbXSs1HQOFn&response_mode=fragment"
				},
				"ipAddress": "24.4.98.131"
			},
			"protocol": {
				"type": "OAUTH2.0",
				"request": {
					"scope": "openid profile email address phone providers:read",
					"state": "myState",
					"redirect_uri": "http://localhost:8080",
					"response_mode": "fragment",
					"response_type": "id_token token",
					"client_id": "ULx7dd5LbNbXSs1HQOFn"
				},
				"issuer": {
					"uri": "https://gronberg.oktapreview.com/oauth2/ausar8buzjFYEyAf00h7"
				},
				"client": {
					"id": "ULx7dd5LbNbXSs1HQOFn",
					"name": "AcmeHealth",
					"type": "PUBLIC"
				}
			},
			"session": {
				"id": "1027TQm2pqHTyCeVhJmKHZnkw",
				"userId": "00u8740btccGyzDWx0h7",
				"login": "john.gronberg@okta.com",
				"createdAt": "2018-10-26T00:45:18.000Z",
				"expiresAt": "2019-02-05T04:32:50.000Z",
				"status": "ACTIVE",
				"lastPasswordVerification": "2018-10-26T00:45:18.000Z",
				"lastFactorVerification": "2018-11-02T23:27:08.000Z",
				"amr": ["MULTIFACTOR_AUTHENTICATION", "POP_SOFTWARE_KEY", "PASSWORD"],
				"idp": {
					"id": "00o8740bsphMpKDXb0h7",
					"type": "OKTA"
				},
				"mfaActive": false
			},
			"user": {
				"id": "00u8740btccGyzDWx0h7",
				"passwordChanged": "2016-11-04T21:45:01.000Z",
				"profile": {
					"login": "john.gronberg@okta.com",
					"firstName": "John",
					"lastName": "Gronberg",
					"locale": "en",
					"timeZone": "America/Los_Angeles"
				},
				"_links": {
					"groups": {
						"href": "https://gronberg.oktapreview.com/00u8740btccGyzDWx0h7/groups"
					},
					"factors": {
						"href": "https://gronberg.oktapreview.com/api/v1/users/00u8740btccGyzDWx0h7/factors"
					}
				}
			},
			"policy": {
				"id": "00par8cw5hoBA4cbS0h7",
				"rule": {
					"id": "0prar8bv1vRYcx0BM0h7"
				}
			}
		},
		"tokens": {
			"access_token": {
				"claims": {
					"ver": 1,
					"jti": "AT.VqGGGOzfXBO9W8sQX9VVesoEZPNcfaA40O03kL3bmeY",
					"iss": "https://gronberg.oktapreview.com/oauth2/ausar8buzjFYEyAf00h7",
					"aud": "http://api.acmehealth.com",
					"cid": "ULx7dd5LbNbXSs1HQOFn",
					"uid": "00u8740btccGyzDWx0h7",
					"sub": "john.gronberg@okta.com",
					"patientId": "00u8740btccGyzDWx0h7",
					"appProfile": [null],
					"claim1": "123"
				},
				"lifetime": {
					"expiration": 3600
				},
				"scopes": {
					"providers:read": {
						"id": "scpar8df93cuLZCxF0h7",
						"action": "GRANT"
					},
					"address": {
						"id": "scp84nOCzqB0J8XnANCm",
						"action": "GRANT"
					},
					"phone": {
						"id": "scpNswJCADebMsTYA8QK",
						"action": "GRANT"
					},
					"openid": {
						"id": "scpfdR6WGIhVJRdYu3Nn",
						"action": "GRANT"
					},
					"profile": {
						"id": "scpbqkNe8xw5Kn2TggXf",
						"action": "GRANT"
					},
					"email": {
						"id": "scpaCFpTtlAKTMa28vPW",
						"action": "GRANT"
					}
				}
			},
			"id_token": {
				"claims": {
					"sub": "00u8740btccGyzDWx0h7",
					"name": "Gronberg",
					"email": "john.gronberg@okta.com",
					"ver": 1,
					"iss": "https://gronberg.oktapreview.com/oauth2/ausar8buzjFYEyAf00h7",
					"aud": "ULx7dd5LbNbXSs1HQOFn",
					"jti": "ID.ScFFhqwI6YT0pi8ryr00F0HR84Tom284TlismVBpBjM",
					"amr": ["mfa", "swk", "pwd"],
					"idp": "00o8740bsphMpKDXb0h7",
					"nonce": "fc2422c5-dd72-469d-af25-23ad33052ec6",
					"preferred_username": "john.gronberg@okta.com",
					"auth_time": 1540514718
				},
				"lifetime": {
					"expiration": 3600
				}
			}
		}
	}
}





