# ACES Marketplace

## Registering ACES Service Nodes

The ACES Marketplace uses OAuth2 authorization for accessing account information.
First create an account on the marketplace:

```
curl -X POST \
  https://marketplace.arkaces.com/api/registrations \
  -H 'Content-Type: application/json' \
  -d '{
	"contactEmailAddress": "contact@example.com",
	"userName": "Test User",
	"userEmailAddress": "user@example.com",
	"userPassword": "password"
}'
```

```
{
    "id": "TL2OGrvJmmoHqdqUFCtF",
    "createdAt": "2018-03-08T01:10:53.984Z",
    "account": {
        "id": "lojmbFIdFks9O737s0tE",
        "contactEmailAddress": "contact@example.com",
        "isContactEmailAddressVerified": false,
        "createdAt": "2018-03-08T01:10:53.932Z",
        "users": [
            {
                "id": "2voDNTLTkfrFgycpUaYd",
                "name": "Test User",
                "emailAddress": "user@example.com",
                "isEmailAddressVerified": false
            }
        ]
    }
}
```

Use your created user id (`2voDNTLTkfrFgycpUaYd` in the example above) for OAuth2 authentication calls.

## Authenticating User

Authenticate user credentials to obtain an API access token using the OAuth2 token endpoint with
Http Basic Authorization header `Authorization: Basic bWFya2V0cGxhY2U6c2VjcmV0` 
(`Base64("marketplace:secret"`) and your user id and password in the post body:

```
curl -X POST \
  https://marketplace.arkaces.com/api/oauth/token \
  -H 'Authorization: Basic bWFya2V0cGxhY2U6c2VjcmV0' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=password&username=2voDNTLTkfrFgycpUaYd&password=test123
```

```
{
    "access_token": "67474c49-1dad-4fef-8f8c-a7caa0032f5f",
    "token_type": "bearer",
    "refresh_token": "2d1eb332-3926-45ee-9fa2-5b7971eac4a1",
    "expires_in": 3599,
    "scope": "read write"
}
```

The `access_token` can then be used in all future API calls using `Authorization: Bearer {access_token}`.


## Registering an ACES Service Node

Service nodes must implement the [ACES Service API Specification](https://ark-aces.github.io/aces-service-docs/).
To register a service, post the service URL to the `/account/services` endpoint:

```
curl -X POST \
  https://marketplace.arkaces.com/api/account/services \
  -H 'Authorization: Bearer 67474c49-1dad-4fef-8f8c-a7caa0032f5f' \
  -H 'Content-Type: application/json' \
  -d '{
	"url": "http://52.27.61.92:9192/"
}'
```

```
{
    "service": {
        "id": "sUbNqYf2K4CvS5vmIMum",
        "url": "http://52.27.61.92:9192/",
        "name": "Aces Testnet Ark Ethereum Contract Service",
        "version": "1.0.0",
        "description": "ACES Testnet Ark Ethereum Contract Service for deploying Rinkeby testnet smart contracts with Ark",
        "websiteUrl": "https://arkaces.com",
        "createdAt": "2018-03-08T01:11:27.428Z"
    },
    "createdAt": "2018-03-08T01:11:27.434Z"
}
```
