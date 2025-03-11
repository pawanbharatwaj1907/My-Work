# Step 1: Request API Gateway Access Token
    The Supplier Application sends a request to the IDP (Identity Provider) to obtain an API Gateway access token.
    The request includes:
    grant_type: "client_credentials"
    client_assertion_type: JWT bearer token type
    client_assertion: A client-provided assertion
    scope: "apis.default"
    The IDP returns an API Gateway access token with:
    token_type: "Bearer"
    expires_in: 3600 seconds (1 hour)
    access_token: The obtained token
    scope: "apis.default"

# Step 2: Request KMS Access Token
    The Supplier Application sends a request for a KMS access token through the API Gateway.
    The API Gateway forwards the request to KMS.
    The request includes:
    grant_type: "password"
    username: KMS username
    password: KMS password
    The API Gateway includes an authorization header:
    Authorization: Bearer {{IDP_ACCESS_TOKEN}}
    KMS returns a response containing:
    jwt: The KMS access token
    refresh_token: A refresh token
    duration: 300 seconds (5 minutes)

# Step 3: Call KMS Signature API
    The Supplier Application requests the KMS Signature API through the API Gateway.
    The request is sent to:
    https://{{HOST}}/{{URI}}/crypto/sign?hashAlgo=SHA-256&pad=PSS&sign=RSA&keyName={{KEYNAME}}
    The request includes a payload containing the value to be signed.
    The API Gateway adds two authorization headers:
        Authorization: Bearer {{IDP_ACCESS_TOKEN}} (to pass through API Gateway)
        KMS-Authorization: {{KMS_ACCESS_TOKEN}} (to authenticate on KMS)
    The request is forwarded to KMS.
    KMS returns the signed data.

# Final Output
    The Supplier Application receives the signed response containing:
    { "data": "XXXXXXXXXX" }

# Key Points
    API authentication follows a two-step process:
    Obtain API Gateway access token.
    Use it to request a KMS access token.
    Secure communication is maintained using JWT tokens and OAuth authentication.
    Access tokens have expiration times, requiring refresh mechanisms.
    The final goal is to sign data using KMS cryptographic APIs.
