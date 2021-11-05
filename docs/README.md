

Digital Commerce API
====================

1 Swiss Post e-commerce API reference
-------------------------------------

The Swiss Post **e-commerce API** is a **REST** API which provides predictable and resource-oriented URLs and uses HTTP response codes to indicate API errors. It uses built-in HTTP features to receive commands and return responses. This makes it easy to communicate with from a wide variety of environments, from command-line utilities, server-side or client-side web applications and mobile devices. It supports the **JSON** format in requests and it returns JSON content in all of its responses, including errors. It supports cross-origin resource sharing to allow you to interact securely with our API from a client-side web application.

The access to the Swiss Post e-commerce API is controlled by the **OAuth** authorization protocol. An **access token** is required to consume the **endpoints** exposed by the API. This is obtained after a sucessful authentication.

2 Overview
----------

![](https://developer.post.ch/-/media/post-maxisites/developer/images/ms-develop-dc-api-general-info-01.jpg?mw=1600&vs=2&sc_lang=en&hash=33149EEA90A1215CC53FAD2FDFC241C4)

![](https://developer.post.ch/-/media/post-maxisites/developer/images/ms-develop-dc-api-general-info-02.jpg?mw=1600&vs=2&sc_lang=en&hash=7BADBF17749FE08E274F94C2BF85A1D2)

### 2.1 API endpoints

Endpoint

URL

Aim

address

[https://wedec.post.ch/api/address/v1](https://wedec.post.ch/api/address/v1)

Provides the access to the personal addresses of Swiss Post registered users, it refers to the list of their delivery addresses.  
Moreover this API provides the access to unpersonal business data and exposes address validation service and an auto-completion service for zip codes, street names and house numbers.

delivery

[https://wedec.post.ch/api/delivery/v1](https://wedec.post.ch/api/delivery/v1)

Provides the access to unpersonal data delivered by the logistic services of Swiss Post, it refers to the list of the deliverabilities, when and how a given valid address can be delivered.

Pickpost

[https://wedec.post.ch/api/pickpost/v1](https://wedec.post.ch/api/pickpost/v1)

Provides access to retrieve the Pickpost user ID. If a user does not exist yet in PickPost a new PickPost ID is automatically generated and returned to the caller.

Barcode

[https://wedec.post.ch/api/barcode/v1](https://wedec.post.ch/api/barcode/v1)

Provides access to generate address labels

pick@home

[https://api.post.ch/pah/v1](https://api.post.ch/pah/v1)

Allows to create return orders for the collection at a customers home address.

userinfo

[https://wedec.post.ch/api/userinfo](http://https://wedec.post.ch/api/userinfo)

Provides the access to personal data of Swiss Post registered users, it refers to the attributes of their Swiss Post profile.  
[http://openid.net/specs/openid-connect-core-1\_0.html#UserInfo](http://openid.net/specs/openid-connect-core-1_0.html#UserInfo)  
[http://openid.net/specs/openid-connect-core-1\_0.html#IDToken](http://openid.net/specs/openid-connect-core-1_0.html#IDToken)

authorization

[https://wedec.post.ch/WEDECOAuth/authorization](https://wedec.post.ch/WEDECOAuth/authorization)

OAuth Endpoint for querying an authorization code, usually obtained with the consent of an authenticated end user, given a set of scopes and credentials.  
[http://openid.net/specs/openid-connect-core-1\_0.html#AuthRequest](http://openid.net/specs/openid-connect-core-1_0.html#AuthRequest)

token

[https://wedec.post.ch/WEDECOAuth/token](https://wedec.post.ch/WEDECOAuth/token)

OAuth Endpoint for exchanging an authorization code against an access token.  
[http://openid.net/specs/openid-connect-core-1\_0.html#TokenRequest](http://openid.net/specs/openid-connect-core-1_0.html#TokenRequest)



3 Authentication
----------------

The access to the Swiss Post connector and the e-commerce API is controlled by both **OpenID-Connect** protocol **version 1.0** and **OAuth** authorization protocol **version 2.0**.

Only Swiss Post registered clients can be authorized to consume the Swiss Post connector and the e-commerce API. The OpenID endpoint and some endpoints of the e-commerce API require the **authorization** of the end users when personal data is queried.

OAuth is an authorization protocol and not an authentication protocol. The client and the end user have to be authenticated; consequently the authentication is an implicit part of the protocol.

For more information see [http://tools.ietf.org/html/rfc6749](http://tools.ietf.org/html/rfc6749)

### 3.1 OAuth

A client of the Swiss Post e-commerce API must first obtain an OAuth **access token** in order to consume the endpoints of the API.

*   The client is authenticated by the OAuth credentials received after a successful registration: **client-id, client-secret**.
*   A set of scopes need to be mentioned when ordering the access token, a scope corresponds to a use case applied on one or more endpoints of the API.
*   These **scopes** must be a subset of the ones configured during the **registration** process.
*   Moreover the end user may have to be **authenticated** and have to **authorize** the client for the set of ordered scopes.
*   Finally an **access token** is returned to the client through a **redirection URL** configured during the registration. The returned access token is a **bearer token**.
*   Depending on the implemented OAuth **flow** for the query of the access token, a **refresh token** is returned. This is used for access token renewal without user consent.

The OAuth authorization protocol defines a set of standard flows for querying an access token. A client of the Swiss Post e-commerce API is responsible for the integration of the desired flows in his use cases.

For more information see [http://tools.ietf.org/html/rfc6749](http://tools.ietf.org/html/rfc6749)

### 3.2 OpenID-Connect

The access to personal data by a registered client through the Swiss Post connector is controlled by the OpenID-Connect protocol. This is based on the OAuth standard authorization protocol. The OpenID-Connect flow is based on the OAuth authorization code grant flow or the OAuth implicit code grant flow. It finally returns an ID-Token and an access token to the client.

*   The returned ID-Token contains the personal data out of the identity of the end user mapped to each ordered OpenID scope.
*   The returned access token can be used for consuming the standard Userinfo endpoint or the endpoints of the Swiss Post e-commerce API when eventually covered by the other ordered non-OpenID scopes.
*   Both the returned ID-Token and access token are bearer tokens.
*   With the ID-Token the client maybe already receives the full desired data at the end of the flow and is not forced to implement a subsequent call to consume the standard Userinfo endpoint.

A client of the Swiss Post Connector is responsible for the integration of the desired flows in his use cases.

For more information see [http://openid.net/specs/openid-connect-core-1\_0.html#Introduction](http://openid.net/specs/openid-connect-core-1_0.html#Introduction)

### 3.3 Authorization code grant flow

See [http://openid.net/specs/openid-connect-core-1\_0.html#CodeFlowAuth](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)

Flow meant for scopes giving access to personal data, requiring consent by the end user.

Some scopes giving access to non-personal data can be ordered besides the other scopes. They do not require any consent by the end user and are not displayed inside the consent screen.

If only non-personal-data scopes are used, it’s usually better to use the Client Credentials flow

#### 3.3.1 Authentication request

#### GET request

    https://wedec.post.ch/WEDECOAuth/authorization?
    client_id=wedec-fake-shop&
    scope=WEDEC_READ_ADDRESS+openid+profile+email+address&
    response_type=code&
    redirect_uri=http://localhost:8080/fake-shop/&
    nonce=f5bb9f70-2&
    state=bbe6606b-b

Copy

For more information see [http://openid.net/specs/openid-connect-core-1\_0.html#AuthRequest](http://openid.net/specs/openid-connect-core-1_0.html#AuthRequest)

#### 3.3.2 Authentication response

#### Response

    HTTP/1.1 302 Found
    Location: http://localhost:8080/fake-shop/?state=056be689-d&code=iUkxvid_e4k813KgLWcNvYj2KAqK60r0ixe8W4kiMPY

Copy

For more information see [http://openid.net/specs/openid-connect-core-1\_0.html#AuthResponse](http://openid.net/specs/openid-connect-core-1_0.html#AuthResponse)

#### 3.3.3 Authentication response validation

We recommend that the client validates the authentication response, see [http://openid.net/specs/openid-connect-core-1\_0.html#AuthResponseValidation](http://openid.net/specs/openid-connect-core-1_0.html#AuthResponseValidation)

#### 3.3.4 Token request

#### POST request

    https://wedec.post.ch/WEDECOAuth/token?
    grant_type=authorization_code&
    client_id=wedec-fake-shop&
    client_secret=wedec-fake-shop-secret&
    redirect_uri=http://localhost:8080/fake-shop/&
    code=iUkxvid_e4k813KgLWcNvYj2KAqK60r0ixe8W4kiMPY

Copy

For more information see [http://openid.net/specs/openid-connect-core-1\_0.html#TokenRequest](http://openid.net/specs/openid-connect-core-1_0.html#TokenRequest)

#### 3.3.5 Token response

#### Response

    HTTP/1.1 200 OK
    Content-Type: application/json;charset=UTF-8
    {
    “expires_in”:300,
    “token_type”:“Bearer”,
    “id_token”:
    “eyJhbGciOiJSUzI1NiJ9.eyJleHAiOjE0MTEwNTE5MTQsInN1YiI6IjEwMDQ5MjIiLCJub25jZSI6IjczZDc3N2U0LTciLCJhdWQiOlsid2Vk
    ZWMtZmFrZS1zaG9wIl0sImlzcyI6Imh0dHBzOlwvXC9hcGlkZXYucG5ldC5jaCIsImlhdCI6MTQxMTA1MTMxNH0.
    SBYodQCRmsbZvSHnBenLqPGS-U9Q_Z
    S8wTM7TvvyiDxiF27pKvjwsF6vYzJsucpluz750bH-OVtL-Esh72M-Ki8L_3hGImgpZ-K7KaRRM9BG3UA-
    5M8ZZloVTpz6W47H_xi-Q_NwCqApgawdEP8rI
    ECKtSdk3En8A3rDSrCLNhF2LO-56rsC2rwcdBqrpth_89Iq00O1kMPNZ2H_HJpQzBIku04WGOwbx-
    2K3f5b_BV-VKVjqkqEMoacJcP_c9pQY2YxpIGAfOnS
    ROMYCJfM5M-QmsgDCn9B9Z0yicbPwDexS4y1FqOMFXVIJYt94qek8n8CVqp9KQBl2ptEuAbD6HIA”,
    “access_token”:“eyJhbGciOiJSU0ExXzUiLCJlbmMiOiJBMjU2R0NNIn0.XpENWZLOwkvlwiY0TWCUZ2LTAbd-
    P895E1BlAClqmB3oHTehtMGkXCNudURQtwc
    CVR23dYalRYLDP_huIHqwNjHoAYip2CsyiTrYQySp4wZzRb5Cm1QKvTq2yGfFUcK2qIqr7nFD_CEei8_EpVHanYSE3srnWZkVEI8btQOg32bKOaidwLBaP
    LgFHfgom6l7n6ewh7UFv1WcRjk9Ug48oHUrpVQEBnu1pXr8gFEtEpmTZhT1TUCwUsyxzH36dkgzSZ4DHCvuIPDOtWpurQZQiu0OQVWd3ih2z7jSECXzUD7
    9oIxqzN_pWNVeb4jGvVhTYktBmsCYxJZCslm_NtuMJg.V4qpGmkldszeRMk.KI_swCCP6rvvkiGOkNWLghqK-
    0BXrsz5qdimYNxkGNkbE2NQeJKfaGPtLKb
    1d59YIwe_Ng_kqpBg05_39OOLgC4tA5JsEipOPm951fDlclmx0ybKwPqtNCjigGP_yxrwcbHwPSEofnNMakt-
    GnVpCadUMjeNUb6QRzybylS3CxeWsv19md
    U48xTiHGNqzgB2iWT9DkzfCcP0o3bTqTNcdD4uLdE1oA-hev5bT3XDV56XlH1SprSva2sQ86k6mFgHTcqTp8MnFzEXSjBe_
    QkiJJgfx53c8pzVruby2kiW
    xdYm91ILStVUCQZ26EPGFhTN03TZTZ0BeM6cHvapvOO_JWwWdbsc_dx4YLpCIdF9FjlJruLHFx-
    29ZAweSuXdSmM4v7X9jCh3_5iNWeniYjwmzYb9vMbOd2
    UrlGoGxnUdJ_BTOjB8LXHQ7mO35ycl4L0o7j8pF6qDxJiQapPFl4jI77WksrvkVFNvyTUIL_JggQrPjO0cCuHlevIi-
    W0I50aS3Vj8oGLFcOM9QnBuL3tyk
    1BvQdwtfXX6mmqvp1OfFWkfClytBcBcBWdLgzFKKpkXQ0Unaa_uQxtVLBfAuisF1w8idPia9hcP2jv_Eeyy0iqjnDtO.
    CKPdNsuq22qlhNRRQODqnw”
    }

Copy

For more information see [http://openid.net/specs/openid-connect-core-1\_0.html#TokenResponse](http://openid.net/specs/openid-connect-core-1_0.html#TokenResponse)

#### 3.3.6 Token response validation

We recommend that the client validates the token response, see [http://openid.net/specs/openid-connectcore-1\_0.html#TokenResponseValidation](http://openid.net/specs/openid-connectcore-1_0.html#TokenResponseValidation)

### 3.4 Client credential flow

This flow is suitable for the server-side access by a registered client to non-personal data. No consent by any end user is necessary for the ordered scopes, the client exchanges its OAuth credentials for an access token.

This flow is implemented on the server side.

#### 3.4.1 Token request

    $ curl -X POST https://wedec.post.ch/WEDECOAuth/token?grant_type=client_credentials&client_id=wedec-fake-shop&client_secret=wedec-fake-shop-secret&scope=WEDEC_DELIVERY

Copy

### 3.5 Use access token

#### 3.5.1 Resource request

    $ curl -H Authorization: Bearer
    eyJhbGciOiJSU0ExXzUiLCJlbmMiOiJBMjU2R0NNIn0.XpENWZLOwkvlwiY0TWCUZ2LTAbdP895E1BlAClqm-
    B3oHTehtMGkXCNudURQtwc
    CVR23dYalRYLDP_huIHqwNjHoAYip2CsyiTrYQySp4wZzRb5Cm1QKvTq2yGfFUcK2qIqr7nFD_CEei8_
    EpVHanYSE3srnWZkVEI8btQOg32bKOaidwLBaP
    LgFHfgom6l7n6ewh7UFv1WcRjk9Ug48oHUrpVQEBnu1pXr8gFEtEpmTZhT1TUCwUsyxzH36dkgzSZ4DHCvuIPDOtWpurQZQiu0OQVWd3ih2z7jSECXzUD7
    9oIxqzN_pWNVeb4jGvVhTYktBmsCYxJZCslm_NtuMJg.V4qpGmkldszeRMk.KI_swCCP6rvvkiGOkNWLghqK0BXrsz5qdimYNxkGNkbE2NQeJKfaGPtLKb
    1d59YIwe_Ng_kqpBg05_39OOLgC4tA5JsEipOPm951fDlclmx0ybKwPqtNCjigGP_yxrwcbHwPSEofnNMaktGnVpCadUMjeNUb6QRzybylS3CxeWsv19md
    U48xTiHGNqzgB2iWT9DkzfCcP0o3bTqTNcdD4uLdE1oAhev5bT3XDV56XlH1SprSva2sQ86k6mFgHTcqTp8MnFzEXSjBe_
    QkiJJgfx53c8pzVruby2kiW
    xdYm91ILStVUCQZ26EPGFhTN03TZTZ0BeM6cHvapvOO_JWwWdbsc_dx4YLpCIdF9FjlJruLHFx-
    29ZAweSuXdSmM4v7X9jCh3_5iNWeniYjwmzYb9vMbOd2
    UrlGoGxnUdJ_BTOjB8LXHQ7mO35ycl4L0o7j8pF6qDxJiQapPFl4jI77WksrvkVFNvyTUIL_JggQrPjO0cCu-
    HlevIiW0I50aS3Vj8oGLFcOM9QnBuL3tyk
    1BvQdwtfXX6mmqvp1OfFWkfClytBcBcBWdLgzFKKpkXQ0Unaa_uQxtVLBfAuisF1w8idPia9hcP2jv_Eeyy-
    0iqjnDtO.CKPdNsuq22qlhNRRQODqnw
    https://wedec.post.ch/api/userinfo
    HTTP/1.1 200 OK Content-Type: application/json
    {
    “sub”: “248289761001”,
    “name”: “Lang Michael”,
    “given_name”: “Michael”,
    “family_name”: “Lang”,
    “preferred_username”: “michael.lang”,
    “email”: “michael.lang@gmail.com”,
    }

Copy

### 3.6 Use refresh token

Refresh tokens are issued when access tokens are issued during the authorization code grant flow and client credential grant flow. It can also depend on the configuration of the authroization server.

A refresh token has a higher TTL than the associated access token. This refresh token is used for access token renewal without user consent.

#### 3.6.1 Access token renewal

    $ curl -H Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    https://wedec.post.ch/WEDECOAuth/token?
    grant_type=refresh_token&
    refresh_token=tGzv3JOkF0XG5Qx2TlKWIA

Copy

### 3.7 End user’s personal information query (Login Post Connector)

#### 3.7.1 Prerequisites

*   OAuth access token for a set of OpenID scopes: openid, profile, email, address, phone. These scopes require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow.
*   Each OpenID scope is associated with a set of OpenID claims, see [https://openid.net/specs/openid-connect-core-1\_0.html#ClaimsNot AccessibleTarget not accessible](https://openid.net/specs/openid-connect-core-1_0.html#Claims)
*   Bearer access token stored in the header of the request:  
    Authorization: Bearer <OAuth access token>

#### 3.7.2 KLP mapping

The standard OpenID claims (see [http://openid.net/specs/openid-connect-core-1\_0.html#StandardClaims](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)) are mapped this way:

OpenID claims

Aggregation

Swiss Post profile attribute

sub

\-

user profile id

name

\-

firstName name

given\_name

\-

firstName

family\_name

\-

name

email

\-

email

email\_verified

\-

TRUE or FALSE

gender

\-

female or male

locale

\-

language (rfc5646, without country code)

phone\_number

\-

mobilePhone or phone

phone\_number\_verified

\-

TRUE or FALSE

address

formatted

name firstName  
addressLine1  
addressLine2  
addressLine3  
addressLine4  
zip city  
country

address

street\_address

addressLine2

address

locality

city

address

postal\_code

zip

address

country

country

updated\_at

\-

last user account change

#### 3.7.3 The information out of the profile of the end user

#### GET request

    https://wedec.post.ch/api/userinfo

Copy

#### Response

    {
    “sub”: “248289761001”,
    “name”: “Lang Michael”,
    “given_name”: “Michael”,
    “family_name”: “Lang”,
    “preferred_username”: “michael.lang”,
    “email”: “michael.lang@gmail.com”,
    }

Copy

#### 3.7.4 Error codes

Code

Reason

200

OK

403

Not authorized

404

No results found

500

Internal server error

4 Alternative Delivery Addresses
--------------------------------

### 4.1 End user’s addresses query

#### 4.1.1 Prerequisites

*   OAuth access token for a set of scopes: WEDEC\_READ\_ADDRESS, WEDEC\_READ\_MAIN\_ADDRESS. Both scopes require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow.
*   Bearer access token stored in the header of the request:  
    Authorization: Bearer <OAuth access token>
*   Address API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml)
*   Address API | PROD | [https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/address/v1/swagger.yaml](https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/address/v1/swagger.yaml)

#### 4.1.2 The personal addresses of the end user

#### GET request

    https://wedec.post.ch/api/address/v1/users/current/addresses

Copy

#### Response

    [
    {
    “type”:“MAIN”,
    “nickname”:“KLP”,
    “addressee”:{“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Via
    Golena”,“houseNumber”:“31C”},“zip”:{“zip”:“6512”,“city”:“Giubiasco”}},
    “logisticLocation”:{“house”:{“street”:“Via
    Golena”,“houseNumber”:“31C”},“zip”:{“zip”:“6512”,“city”:“Giubiasco”}}
    },
    {
    “id”:“e3089ed6-35cd-47ed-b477-143396f96ef4”,
    “type”:“DOMICILE”,
    “nickname”:“test-zytglogge”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Zytgloggelaube”,“houseNumber”:“2”,“houseKey”:“701091
    5”},“zip”:{“zip”:“3011”,“city”:“Bern”}},
    “logisticLocation”:{“house”:{“street”:“Zytgloggelaube”,“houseNumber”:“2”,“houseKey”:“7010915”},
    “zip”:{“zip”:“3011”,“city”:“Bern”}}
    },
    {
    “id”:“573b3a3c-43df-4c3b-aaf2-4c3c0ca8f315”,
    “type”:“POST_OFFICE”,
    “nickname”:“432”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Bahnhofstrasse”,“houseNumber”:“18”,“houseKey”:“37651
    ”},“zip”:{“zip”:“611000”,“city”:“Wolhusen”}},
    “logisticLocation”:{“postBoxNumber”:“999”,“house”:{“street”:“Postfach”,“houseKey”:“37651”},“-
    zip”:{“zip”:“6110”,“city”:“Wolhusen”}}
    },
    {
    “id”:“1abf6700-2a3c-49fb-bf2c-c1790d85352f”,
    “type”:“PICK_POST”,
    “nickname”:“Test Modifica”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Brünigstrasse”,“houseNumber”:“101”,“house-
    Key”:“174827”},
    “zip”:{“zip”:“607200”,“city”:“Sachseln”}},
    “logisticLocation”:{“postBoxNumber”:“5”,“house”:{“street”:“Postfach”,“houseKey”:“174827”},“zip”:
    {“zip”:“6072”,“city”:“Sachseln”}}
    },
    {
    “id”:“b8dbdf51-6a6a-4ba4-aa55-45377277651f”,
    “type”:“MY_POST_24”,
    “nickname”:“mypost24”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Avenue
    A.Piccard”,“houseKey”:“76419394”},“zip”:{“zip”:“101573”,“city”:“Lausanne”}},
    “logisticLocation”:{“house”:{“street”:“Avenue
    A.Piccard”,“houseKey”:“76419394”},“zip”:{“zip”:“1015”,“city”:“Lausanne”}}
    },
    {
    “id”:“36e720e8-a908-469e-9daf-6b376071a557”,
    “type”:“POSTBOX”,
    “nickname”:“testPF”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Via
    Galbisio”,“houseNumber”:“2”,“houseKey”:“72014233”,“zip”:{“zip”:“6503”,“city”:“Bellinzona”}},
    “logisticLocation”:{“postBoxNumber”:“123”,“house”:{“street”:“Casella
    postale”,“houseKey”:“72014233”},“zip”:{“zip”:“6503”,“city”:“Bellinzona”}}
    }
    ]

Copy

#### 4.1.3 The main address of the end user

#### GET request

    https://wedec.post.ch/api/address/v1/users/current/addresses/main

Copy

#### Response

    {
    “type”:“MAIN”,
    “nickname”:“KLP”,
    “addressee”:{“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Via Golena”,“houseNumber”:“31C”},“zip”:{“zip”:“6512”,
    “city”:“Giubiasco”}},
    “logisticLocation”:{“house”:{“street”:“Via Golena”,“houseNumber”:“31C”},“zip”:{“zip”:“6512”,“city”:
    “Giubiasco”}}
    }

Copy

#### 4.1.4 A single personal address of the end user

#### GET request

    https://wedec.post.ch/api/address/v1/users/current/addresses/e3089ed6-35cd-47ed-b477-143396f96ef4

Copy

#### Response

    {
    “id”:“e3089ed6-35cd-47ed-b477-143396f96ef4”,
    “type”:“DOMICILE”,
    “nickname”:“test-zytglogge”,
    “addressee”:{“title”:“MISTER”,“firstName”:“Massimo”,“lastName”:“Cotelli”},
    “geographicLocation”:{“house”:{“street”:“Zytgloggelaube”,“houseNumber”:“2”,“house-
    Key”:“7010915”
    ,“zip”:{“zip”:“3011”,“city”:“Bern”}},
    “logisticLocation”:{“house”:{“street”:“Zytgloggelaube”,“houseNumber”:“2”,“house-
    Key”:“7010915”},“zip
    :{“zip”:“3011”,“city”:“Bern”}}
    }

Copy

#### 4.1.5 Error codes

Code

Reason

200

OK

400

Invalis id parameter

403

Not authorized

404

No result found or address does not exist or is not owned by the currently authenticated Swiss Post user

500

Internal server error



5 Address Checker
-----------------

### 5.1 Address validation

#### 5.1.1 Prerequisites

*   OAuth access token for scope WEDEC\_VALIDATE\_ADDRESS. This scope does not require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow or a client credential grant flow.
*   Bearer access token stored in the header of the request:  
    Authorization: Bearer <OAuth access token>
*   Address API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml)
*   Address API | PROD | [https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/address/v1/swagger.yaml](https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/address/v1/swagger.yaml)

#### 5.1.2 Validate a given address

#### POST request

    https://wedec.post.ch/api/address/v1/addresses/validation

Copy

#### POST payload

    {
       "addressee":{
          "firstName":"Hans",
          "lastName":"Muster",
          "title":"MISTER"
       },
       "geographicLocation":{
          "house":{
             "street":"viale Stazione",
             "houseNumber":"15",
             "additionalAddress":""
          },
          "zip":{
             "zip":"6500",
             "city":"Bellinzona"
          }
       },
       "logisticLocation":{
          "postBoxNumber":""
       },
       "fullValidation":true
    }

Copy

#### Response

    {
       "quality":"CERTIFIED",
       "expires":"20190822T181333+0200",
       "address":{
          "type":"DOMICILE",
          "addressee":{
             "firstName":"Hans",
             "lastName":"Muster"
          },
          "logisticLocation":{
             "postBoxNumber":"",
             "house":{
    
             },
             "zip":{
    
             }
          },
          "geographicLocation":{
             "house":{
                "street":"viale Stazione",
                "houseNumber":"15",
                "additionalAddress":"",
                "houseKey":"76439798"
             },
             "zip":{
                "zip":"6500",
                "city":"Bellinzona"
             }
          },
          "id":"034ce086-c9bb-49df-9ed1-1b674e6d5717"
       }
    }

Copy

#### 5.1.3 Error codes

Code

Reason

200

OK

400

Invalid address parameter

403

Not authorized

500

Internal server error

### 5.2 Address auto-completion

#### 5.2.1 Prerequisites

*   OAuth access token for scope WEDEC\_AUTOCOMPLETE\_ADDRESS. This scope does not require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow or a client credential grant flow.
*   Bearer access token stored in the header of the request:  
    Authorization: Bearer <OAuth access token>
*   Address API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/address/v1/swagger.yaml)

#### 5.2.2 Zip auto-completion

#### GET request

    https://wedec.post.ch/api/address/v1/zips?zipCity=66&type=DOMICILE
    https://wedec.post.ch/api/address/v1/zips?zipCity=66&type=POSTBOX

Copy

#### Response

    {
    “zips”:[
    {“zip”:“6600”,“city18”:“Locarno”,“city27”:“Locarno”},
    {“zip”:“6600”,“city18”:“Muralto”,“city27”:“Muralto”},
    {“zip”:“6600”,“city18”:“Solduno”,“city27”:“Solduno”},
    {“zip”:“6601”,“city18”:“Locarno”,“city27”:“Locarno”},
    {“zip”:“6602”,“city18”:“Muralto”,“city27”:“Muralto”},
    {“zip”:“6604”,“city18”:“Locarno”,“city27”:“Locarno”},
    {“zip”:“6605”,“city18”:“Locarno”,“city27”:“Locarno”},
    {“zip”:“6605”,“city18”:“Monte Brè Locarno”,“city27”:“Monte Brè sopra Locarno”},
    {“zip”:“6611”,“city18”:“Crana”,“city27”:“Crana”},
    {“zip”:“6611”,“city18”:“Gresso”,“city27”:“Gresso”},
    {“zip”:“6611”,“city18”:“Mosogno”,“city27”:“Mosogno”},
    {“zip”:“6612”,“city18”:“Ascona”,“city27”:“Ascona”},
    {“zip”:“6613”,“city18”:“Porto Ronco”,“city27”:“Porto Ronco”},
    {“zip”:“6614”,“city18”:“Brissago”,“city27”:“Brissago”},
    {“zip”:“6614”,“city18”:“Isole di Brissago”,“city27”:“Isole di Brissago”},
    {“zip”:“6616”,“city18”:“Losone”,“city27”:“Losone”},
    {“zip”:“6618”,“city18”:“Arcegno”,“city27”:“Arcegno”},
    {“zip”:“6622”,“city18”:“Ronco sopra Ascona”,“city27”:“Ronco sopra Ascona”},
    {“zip”:“6631”,“city18”:“Corippo”,“city27”:“Corippo”},
    {“zip”:“6632”,“city18”:“Vogorno”,“city27”:“Vogorno”}
    ]
    }

Copy

#### 5.2.3 Street name auto-completion

#### GET request

    https://wedec.post.ch/api/address/v1/streets?name=Via+Se
    https://wedec.post.ch/api/address/v1/streets?name=Via+
    Se&zip=6600

Copy

#### Response

    {
    “streets”:[
    “Via Sempione”,
    “Via Serafino Balestra”
    ]
    }

Copy

#### 5.2.4 House number auto-completion

#### GET request

    https://wedec.post.ch/api/address/v1/houses?zip=6600&streetname=Via+Serafino+Balestra&number=2

Copy

#### Response

    {
    “houses”:[“1”,“1 A”,“2”,“3”,“4”,“5”,“6”,“7”,“8”,“9 A”]
    }

Copy

#### 5.2.5 Error codes

Code

Reason

200

OK

400

Incomplete/invalid search parameters

403

Not authorized

404

No results found

500

Internal server error

6 Delivery
----------

### 6.1 Prerequisites

*   OAuth access token for scope WEDEC\_DELIVERY. This scope does not require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow or a client credential grant flow.
*   Bearer access token stored in the header of the request: Authorization: Bearer <OAuth access token>
*   Delivery API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/delivery/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/delivery/v1/swagger.yaml)
*   Delivery API | PROD | [https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/delivery/v1/swagger.yaml](https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/delivery/v1/swagger.yaml)

### 6.2 Delivery days query

#### 6.2.1 The deliverabilities at Serafino Balestra 20, 6600 Locarno for the next day

#### GET request

    https://wedec.post.ch/api/delivery/v1/deliverabilities/when?
    address.geographicLocation.house.street=Serafino+Balestra&
    address.geographicLocation.house.houseNumber=20&
    address.geographicLocation.zip.zip=6600&
    address.geographicLocation.zip.city=Locarno&
    dayCount=3

Copy

Please note: Using the product filter is optional.

#### Response

    {
       "days":[
          {
             "deliveryDate":"2019-09-03",
             "deliverabilities":[
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"PRI",
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"SEM",
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZFZ0912,PRI",
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZFZ1114,PRI",
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZFZ0912,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"04:00"
                   },
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZFZ1114,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"04:00"
                   },
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZF16302100,PRI"
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZF16301800,PRI"
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZF17301900,PRI"
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZF18302000,PRI"
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZF19302100,PRI"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF16302100,SKB"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF16301800,SKB"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF17301900,SKB"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF18302000,SKB"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF19302100,SKB"
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF16302100,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"14:00"
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF16301800,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"14:00"
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF17301900,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"14:00"
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF18302000,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"14:00"
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZF19302100,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"14:00"
                   }
                },
                {
                   "incomingDate":"2019-09-02",
                   "productCode":"ZFZ1217,PRI",
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                },
                {
                   "incomingDate":"2019-09-03",
                   "productCode":"ZFZ1217,DIR",
                   "dropOffInfo":{
                      "dropOffPoint":"659370",
                      "lastDropOffTime":"04:00"
                   },
                   "deliveryDetails":{
                      "route":"121",
                      "branch":{
                         "id":"659370",
                         "description":"Cadenazzo Base di distribuzione"
                      }
                   }
                }
             ],
             "activity":"WORK"
          }
       ],
       "quality":"RELIABLE",
       "deliveryToTheDoor":"AVAILABLE"
    }

Copy

#### 6.2.2 The deliverabilities at 6600 Locarno (default date of first day = today, default day count = 14)

#### GET request

    https://wedec.post.ch/api/delivery/v1/deliverabilities/when?address.geographicLocation.zip.zip=6600&address.
    geographicLocation.zip.city=Locarno
    https://wedec.post.ch/api/delivery/v1/deliverabilities/when?address.geographicLocation.zip.zip=6600&address.
    geographicLocation.zip.city=Locarno&date=2014-11-17&dayCount=2
    https://wedec.post.ch/api/delivery/v1/deliverabilities/when?address.geographicLocation.zip.zip=6600&address.
    geographicLocation.zip.city=Locarno&dayCount=2

Copy

#### 6.2.3 The deliverabilities at 6959 Piandera Paese for the given delivery dates

#### GET request

    https://wedec.post.ch/api/delivery/v1/deliverabilities/when?
    address.geographicLocation.zip.zip=6959&
    address.geographicLocation.zip.city=Piandera+Paese&
    deliveryDates=2014-12-24&
    deliveryDates=2014-12-25

Copy

#### POST request

    https://wedec.post.ch/api/delivery/v1/deliverabilities/when

Copy

#### POST payload

    {
    “address”: {
    “geographicLocation”: {
    “zip”: {
    “zip”: 6959,
    “city”: “Piandera Paese”
    }
    }
    },
    “deliveryDates”: [“2014-12-24”,“2014-12-25“]
    }

Copy

#### 6.2.4 Error codes

Code

Reason

200

OK

403

Not authorized

404

No results found

500

Internal server error

### 6.3 Logistic product information query

#### 6.3.1 The information for the logistic product: example “PRI”

#### GET request

    https://wedec.post.ch/api/delivery/v1/deliverabilities/how/PRI

Copy

#### Response

    {
    “productCode”:“PRI”,
    “barcode”:“0509”,
    “deliveryInstructions”:[
    {“code”:“3211”},{“code”:“3212”},{“code”:“3213”},{“code”:“3214”},{“code”:“3215”},{“code”:
    “3216”},
    {“code”:“3217”},{“code”:“3218”},{“code”:“3219”},{“code”:“3220”},{“code”:“3222”},{“code”:
    “3232”},
    {“code”:“3233”},{“code”:“3234”}],
    “additionalServices”:[
    {“code”:“BLN”,“mandatory”:false},
    {“code”:“COLD”,“mandatory”:false},
    {“code”:“RMP”,“mandatory”:false},
    {“code”:“SP”,“mandatory”:false},
    {“code”:“MAN”,“mandatory”:false},
    {“code”:“FRA”,“mandatory”:false},
    {“code”:“AS”,“mandatory”:false},
    {“code”:“SI”,“mandatory”:false},
    {“code”:“LQ”,“mandatory”:false}]
    }

Copy

#### 6.3.2 Error codes

Code

Reasom

200

OK

403

Not authorized

404

No results found

500

Internal server error

### 6.4 Additional services information query 

#### 6.4.1 Information for all additional services

#### GET request

    https://wedec.post.ch/api/delivery/v1/options/additionalservices

Copy

#### Response

    [
    {
    “code”:“BLN”,
    “name”:{
    “de”:“BLN”,
    “fr”:“BLN”,
    “it”:“BLN”,
    “en”:“BLN”},
    “barcode”:“0341”
    },
    {“code”:“COLD”,“name”:{“de”:“COLD”,“fr”:“COLD”,“it”:“COLD”,“en”:“COLD”},“barcode”:“
    3781”},
    {“code”:“RMP”,“name”:{“de”:“RMP”,“fr”:“RMP”,“it”:“RMP”,“en”:“RMP”},“barcode”:“0322”},
    {“code”:“SP”,“name”:{“de”:“SP”,“fr”:“SP”,“it”:“SP”,“en”:“SP”},“barcode”:“0309”},
    {“code”:“MAN”,“name”:{“de”:“MAN”,“fr”:“MAN”,“it”:“MAN”,“en”:“MAN”},“barcode”:“0421”},
    {“code”:“FRA”,“name”:{“de”:“FRA”,“fr”:“FRA”,“it”:“FRA”,“en”:“FRA”},“barcode”:“0310”},
    {“code”:“AS”,“name”:{“de”:“AS”,“fr”:“AS”,“it”:“AS”,“en”:“AS”},“barcode”:“0308”},
    {“code”:“SI”,“name”:{“de”:“SI”,“fr”:“SI”,“it”:“SI”,“en”:“SI”},“barcode”:“0307”},
    {“code”:“LQ”,“name”:{“de”:“LQ”,“fr”:“LQ”,“it”:“LQ”,“en”:“LQ”},“barcode”:“0549”},
    {“code”:“SA”,“name”:{“de”:“SA”,“fr”:“SA”,“it”:“SA”,“en”:“SA”},“barcode”:“0543”},
    {“code”:“AZS”,“name”:{“de”:“AZS”,“fr”:“AZS”,“it”:“AZS”,“en”:“AZS”},“barcode”:“0581”}
    ]

Copy

### 6.5 Delivery instructions information query 

#### 6.5.1 Information for all delivery instructions

#### GET request

    https://wedec.post.ch/api/delivery/v1/options/deliveryinstructions

Copy

#### Response

    [
    {
    “code”:“3211”,
    “barcode”:“3211”,
    “name”:{
    “de”:“Sendung dem Empfänger direkt auf der Etage zustellen”,
    “fr”:“Sendung dem Empfänger direkt auf der Etage zustellen”,
    “it”:“Sendung dem Empfänger direkt auf der Etage zustellen”,
    “en”:“Sendung dem Empfänger direkt auf der Etage zustellen”},
    “editable”:false,
    “placeHolderNames”:[],
    “optionNames”:[]
    },
    ...
    ]

Copy

#### 6.5.2 Error codes

Code

Reasom

200

OK

403

Not authorized

404

No results found

500

Internal server error

### 6.6 Holidays query 

#### 6.6.1 The list of holidays

#### GET request

    https://wedec.post.ch/api/delivery/v1/options/holidays

Copy

#### Response

    [
    {
    “date”:“2014-12-24”,
    “name”:{
    “de”:“Weihnachten”,
    “fr”:“Weihnachten”,
    “it”:“Weihnachten”,
    “en”:“Weihnachten”}
    },
    ...
    ]

Copy

#### 6.6.2 Error codes

Code

Reasom

200

OK

403

Not authorized

404

No results found

500

Internal server error

7 PickPost / My Post 24
-----------------------

### 7.1 Prerequisites

*   OAuth access token for scope WEDEC\_PICKPOST. This scope does not require the consent of the end user and can be obtained from:  
    \- an authorization code grant flow  
    \- an implicit grant flow  
    \- a client credential grant flow
*   Bearer access token stored in the header of the request:Authorization: Bearer <OAuth access token>
*   Integration of the “Location search” map application:  
    \- You need to obtain a Google Maps API key. Registration with Google is a necessary prerequisite for this.  
    \- Make the necessary changes in the HTML code in order to integrate the Swiss Post location search into your website.  
    \- You can find a [detailed manual for the integration of the “Location search” map application here](https://www.post.ch/en/business-solutions/digital-commerce/integration-of-the-location-search-map-application).
*   PickPost API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/pickpost/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/pickpost/v1/swagger.yaml)
*   PickPost API | PROD | [https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/pickpost/v1/swagger.yaml](https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/pickpost/v1/swagger.yaml)

### 7.2 Get PickPost ID or My Post 24 ID for a specific user

#### POST request

    https://wedecint.post.ch/api/pickpost/v1/users

Copy

#### POST payload

    {
    “userDetails”: {
    “gender”: “MALE”,
    “firstName”: “Test”,
    “lastName”: “User”,
    “language”: “DE”
    },
    “address”: {
    “house”: {
    “street”: “Bellerivestrasse”,
    “houseNumber”: “5”
    },
    “zip”: {
    “zip”: “8008”,
    “city”: “Zürich”,
    “countryCode”: “CH”
    }
    },
    “notification”: {
    “notifyMode”: “EMAIL”,
    “email”: “testuser1@post.ch”
    },
    “service”: “PICKPOST”
    }

Copy

#### Response

    {
    “id”: “PT011227”,
    “verified”: “NONE”
    }

Copy

#### 7.2.1 Error codes

Code

Reason

200

OK

403

Not authorized

500

Internal server error

8 Barcode
---------

### 8.1 Prerequisites

*   OAuth access token for scope WEDEC\_BARCODE\_READ. This scope does not require the consent of the end user and can be obtained from a client credential grant flow.
*   Bearer access token stored in the header of the request: Authorization: Bearer <OAuth access token>
*   Barcode API | INT | [https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/barcode/v1/swagger.yaml](https://wedecint.post.ch/doc/swagger/index.html?url=https://wedecint.post.ch/doc/api/barcode/v1/swagger.yaml)
*   • Barcode API | PROD | [https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/barcode/v1/swagger.yaml](https://wedec.post.ch/doc/swagger/index.html?url=https://wedec.post.ch/doc/api/barcode/v1/swagger.yaml)

### 8.2 Label formats

*   A7 format (74 ×105 mm)
*   A6 format (105 ×148 mm)
*   A5 format (148 × 210 mm): only available for Domestic Parcels, Express and Solutions
*   Format FE (window envelope): only available for Letters with barcode (BMB) domestic and international

#### 8.2.1 Recipient’s address – maximum number of address lines (concerns the “GenerateLabel” request)

The number of address lines that can be printed on an address label is limited because there is a limited amount of space on the labels. Depending on the selected format, the selected basic service, the number of address fields and, if applicable, the delivery instructions (ZAW) or free text, not all address lines can be printed.

##### Rules when exceeding the maximum amount of address lines

When the maximum permitted amount of address lines is exceeded, address lines are omitted from the address label in the order below. This only applies to address lines from the “Recipient” address block and – if applicable and permissible – for free text:

1.  Title (Title) is omitted
2.  Address suffix (AddressSuffix) is omitted
3.  Name 3 (Name3) is omitted
4.  Free text (FreeText) is omitted

Please find some examples further down.

##### Data transmission

The information from the “AddressSuffix” address field element is not transmitted to DataTransfer, regardless of the number of address lines used.

##### “LabelAddress“ address block

When using the “LabelAddress” address block, you can define yourself which recipient’s address lines are to be printed on the address label and in what order for a minimum of 2 and a maximum of 5 address lines (LabelLine1 to LabelLine5). An exception applies to the fields “ZIP” and “City” (and, for international mailings, also to “Country”), which are taken across from the “Recipient” address block. This means that you must define the procedure to be used yourself if the maximum amount of address lines is exceeded.

1 [↑](#bffn1) Delivery instructions are not possible for A7 format.

##### Examples for addressing rules for the “Recipient” address block

The examples below apply only if the “LabelAddress” address block is not used. Missing information in the recipient’s address is completed using the contents of the “Recipient” address block and – if available – the free text.

Maximum number of address lines per DLG and format

Format ”Fenster“ (FE)

Format A7[\[1\]](#ffn1)

Format A6[\[1\]](#ffn1)

Format A5[\[1\]](#ffn1)

DLG parcel incl. any free text (up to 1 delivery instruction)

\-

6[\[1\]](#ffn1)

8

8

DLG parcel incl. any free text (with 2 delivery instructions)

\-

\-

6

8

DLG Express incl. any free text (up to 1 delivery instruction)

\-

6[\[1\]](#ffn1)

8

8

DLG Express incl. any free text (with 2 delivery instructions)

\-

\-

6

8

DLG solutions, VinoLog **only** incl. any free text (up to 1 delivery instruction)

\-

\-

7

7

DLG solutions, VinoLog **only** incl. any free text (with 2 delivery instructions)

\-

\-

5

7

DLG solutions, **without** VinoLog incl. any free text (up to 1 delivery instruction)

\-

5[\[1\]](#ffn1)

7

7

DLG solutions, **without** VinoLog incl. any free text (with 2 delivery instructions)

\-

\-

5

7

Notime

\-

\-

3

-

DLG BMB domestic

6

6[\[1\]](#ffn1)

8

-

DLG BMB international

6

6[\[1\]](#ffn1)

8

-

Example 1: Format A6, max. 1 ZAW, DLG parcels Details in the “Recipient” block + free text: 8 address lines

Details on the address label: max. no. of address lines allowed: 8 no adjustment by WSBC required

FreeText (1st address line)

FreeText

Title (2nd address line)

Title

Firstname (3rd address line)

Firstname Name1

Name1 (3rd address line)

Name2

Name2 (4th address line)

Name3

Name3 (5th address line)

AddressSuffix

AddressSuffix (6th address line)

Street HouseNo

Street (7th address line)

ZIP City

HouseNo (7th address line)

\-

ZIP (8th address line)

\-

City (8th address line)

\-

Example 2: Format A6, 1 ZAW, DLG parcels Details in the “Recipient” block + free text: 8 address lines

Details on the address label: max. no. of address lines allowed: 6 automatic adjustment by WSBC required

FreeText (1st address line)

FreeText

Title (2nd address line)

Title

Firstname (3rd address line)

Firstname Name1

Name1 (3rd address line)

Name2

Name2 (4th address line)

Name3

Name3 (5th address line)

AddressSuffix

AddressSuffix (6th address line)

Street HouseNo

Street (7th address line)

ZIP City

HouseNo (7th address line)

\-

ZIP (8th address line)

\-

City (8th address line)

\-

Example 3: Format A7, DLG BMB domestic; “Registered (R) domestic” Details in the “Recipient” block: 7 address lines

Details on the address label: max. no. of address lines allowed: 6 automatic adjustment by WSBC

Title (1st address line)

Title

Firstname (2nd address line)

Firstname

Name1 2nd address line)

Name1

Name2 (3rd address line)

Name2

Name3 (4th address line)

Name3

AddressSuffix (5th address line)

AddressSuffix

Street (6th address line)

Street HouseNo

HouseNo (6th address line)

ZIP City

ZIP (7th address line)

\-

City (7th address line)

\-

### 8.3 Printer resolution (dpi)

*   200 dpi (equivalent to 203 dpi on Zebra label printers)
*   300 dpi (equivalent to 305 dpi on Zebra label printers)
*   600 dpi (equivalent to 610 dpi on Zebra label printers)

### 8.4 Image formats / printer languages

*   EPS
*   GIF
*   JPG (not recommended as barcode may not have high enough quality)
*   PNG
*   PDF[\[2\]](#ffn2)
*   sPDF[\[1\]](#ffn2)
*   ZPL2

1 [↑](#bffn2)  Format sPDF is a PDF file without embedded fonts. In order to display this format correctly, the Arial font must be installed on your computer. The generation and transmission times are faster with sPDF than with PDF.

2 [↑](#bffn2)  notime labels can only be created as PDF files.

### 8.5 Layout options for express items

The basic service barcodes for SameDay and Swiss-Express “Moon” services are printed in colour.

![](https://developer.post.ch/-/media/post-maxisites/developer/images/layoutexpressitem.jpg?mw=1600&vs=2&hash=8A903536E950E604C73E2708DC3A1C7D)

If there is no possibility to print the corresponding basic service barcode in colour on the address label, it can be printed in black and white. However, an additional, coloured basic service barcode must then be affixed to the item.

The coloured stickers can be ordered via [www.swisspost.ch/online-services](http://www.swisspost.ch/online-services) > Order forms & brochures.

### 8.6 Label generation time and file sizes

The time it takes to generate a label and the corresponding file size depend on the format selected, the printer resolution, the sender’s logo and the image format / printer language used. The speed of the Internet connection is also a key factor. It is therefore very important to have a fast connection. The table below gives some guideline values (measured with transfer rate of 45,000 kbps, without a sender’s logo). However, these do not take the data rate of your Internet connection into account, which could have a major impact on performance. These are average figures for formats A5, A6, A7 and FE.

Image formats / printer languages

Average figure, only generation time in milliseconds

Average including data transmission in milliseconds

EPS

~ 50

500–1000

GIF

~ 100

500–1000

JPG

~ 300

750–1500

PNG

~ 400

750–1500

PDF

~ 50

500–1000

sPDF

~ 15

300–750

ZPL2

~ 5

300–750

### 8.7 Sender’s logo

The sender’s address must always be entered in the “Barcode” web service. You can hide the display of the sender details on the address label or display them as a text or image (e.g. company logo).

If using an image/logo, please note the following:

File size: max. 50 KB

File format: GIF, PNG or JPG

You can control how your image/logo is printed on address labels with the following four optional fields:

*   Aspect ratio: Using this field, you can decide whether the original ratio of width to height should be maintained or scaled to 47 mm × 25 mm.
*   Vertical align: Using this field, you can decide whether the logo should be aligned vertically at the top or in the middle.
*   Horizontal align: Using this field, you can decide whether the logo should be aligned horizontally at the left margin or flush with the barcode.
*   Rotation: Using this field, you can decide whether the logo should be printed in portrait or landscape orientation on the address label (clockwise rotation options: 0°/90°/180°/270°).

If no settings are changed in these fields, your image/logo will be automatically printed with the following settings:

*   Scaling to the aspect ratio of 1.88 (image width: 47 mm / image height: 25 mm)
*   The logo will be printed rotated anti-clockwise by 90°.

We recommend using a black and white logo for printing in the ZLP2 format.

### 8.8 Notification Services

#### 8.8.1 Notification code

In the "Notification" element, the "Service" field has the following valid values:

Notification

Code

Proof of posting

1

Delivery information

2

Collection information

4

Reminder of recipient

32

Handover status to sender

64

"Exchange/return"

128\*

Saturday delivery

256 \*\*

\* This notification service can only be used with delivery instruction ZAW3233.  
\*\* This notification service can only be used with the additional service “SA”.

Note: this kind of notification services cannot be used for notime. For notime notification please contact your notime agent.

#### 8.8.2 Overview of notification services

Notification services are currently available for the following delivery options:

Basic services1)

Proof of posting (Service code 1)

Delivery information (Service code 2)

Collection information (Service code 4)

Reminder to recipient (Service code 32)

Handover status to sender (Service code 64)

Exchange/return (Service code 128)

Saturday delivery (Service code 256)

PostPac Economy

✓

✓

✓

✓

✓

\-

\-

Bulky goods Economy

✓

✓

✓

✓

✓

\-

\-

PostPac Priority

✓

✓

✓

✓

✓

✓

✓

Bulky good Priority

✓

✓

✓

✓

✓

✓

✓

Swiss-Express "Moon"

✓

✓

✓

✓

✓

✓

 

Bulky goods "Moon"

✓

✓

✓

✓

✓

✓

 

SameDay afternoon/evening

✓

\-

\-

✓

✓

✓

\-

SameDay afternoon/evening bulky goods

✓

\-

\-

✓

✓

✓

\-

VinoLog

✓

✓

✓

✓

✓

✓

✓

Direct parcel posting

✓

\-

\-

✓

✓

✓

\-

Free text supported

✓

✓

\-

✓

\-

\-

\-

1) Notification services cannot be used for basic services with business reply labels.

#### 8.8.3 Notification text messages

The description of the content of SMS and e-mail messages are available at [https://www.post.ch/en/business-solutions/digital-commerce/notification-services](https://www.post.ch/en/business-solutions/digital-commerce/notification-services)

#### 8.8.4 Notification with delivery management option

Offer your customers the option of individually controlling the receipt of their consignments with the status message "Proof of posting"or “delivery information”.

#### 8.8.4.1 Necessary fields

Input field

Explanation

Type

Notification channel (EMAIL)

Communication Email

E-mail address of the consignment recipient

Service

Time of notification (1 “Proof of mailing” or 2 “Delivery information”)

Language

Language of notification

DispatchDate

Handover date of the consignment to Swiss Post

PrzL

Product description (e.g. PRI, SEM)

DeliveryInstructions

Description of the permitted service  
Available options:   
10 = Deposit consignment  
14 = Desired neighbour  
18 = Weekday  
20 = Forwarding

#### 8.8.4.2 Optimal data delivery

In order for the recipient to have as much time as possible to manage the consignment, we recommend ordering service 1 “Notification upon proof of posting”. By specifying the handover date of the delivery (“DispatchDate”) and the product description (“Przl”), the recipient can also be informed of the expected delivery date beforehand.

The “Notification with delivery management option” can only be sent by E-Mail.

If Swiss Post identifies the recipient as a registered user of the "My consignments" online service, he will not receive the notification with delivery management option. Instead, he will be forwarded to his login to access all services of "My consignments".

With the field "DynPic" your logo can be transmitted. This will then be displayed in the notification. The image can have the following formats: jpg, jpeg, gif, png. The ideal resolution is 640x160 pixels.

### 8.9 Printer models approved for “Barcode” web service

When your system receives them, you can forward the labels generated by the “Barcode” web service directly to a continuous label printer. This is possible with printer language ZPL2. In order for this to work, the printer models used must support ZPL2 as a printer language, otherwise the quality requirements of the labels will not be met.

Ideally you should use one of the printer models we have already homologated. To ensure adequate barcode print quality, you should always use high-quality shipping label materials. An overview of our homologated printer models is available at www.swisspost.ch/post-mypostbusiness-auftrag-druckermodelle.

Note also that shipping labels are printed in either landscape or portrait format, depending on the specifications of the homologated printer models.

### 8.10 Overview for service codes (DLC) – Domestic Parcels, Express and Solutions

The service descriptions for the following basic and additional services plus delivery instructions can be found at www.swisspost.ch/post-distribution-national.

Combinations of multiple service codes, e.g. “PRI, SP”, are split into their individual elements. The following is given as an example (sequencing of individual content does not matter):

*   <PRZL>PRI</PRZL>
*   <PRZL>SP</PRZL>

DLC

Basic services

ECO

PostPac Economy

PRI

PostPac Priority

SP, ECO

Bulky goods Economy

SP, PRI

Bulky goods Priority

PPR

PostPac Promo

SEM

Swiss-Express “Moon”

SEM, SP

Bulky goods “Moon”

SKB

SameDay afternoon/evening

SKB, SP

SameDay afternoon/evening bulky goods

VL

VinoLog

DIR

Direct parcel posting

GAS, ECO

PostPac Economy GAS

GAS, PRI

PostPac Priority GAS

GAS, SP, ECO

Bulky goods Economy GAS

GAS, SP, PRI

Bulky goods Priority GAS

GAS, SEM

Swiss-Express “Moon” GAS

GAS, SKB

SameDay afternoon/evening GAS

NOTIME\_PickupPoint

notime eCom with pickup

NOTIME\_HomeDelivery

notime eCom with home delivery

DLC

Delivery instructions

ZAW3211

Direct delivery to an upper floor (A)

ZAW3212

Do not place in letterbox; deliver manually or notify (B)

ZAW3213

Notify delivery by telephone (C)

ZAW3214

Place in letterbox or at front door (D)

ZAW3215

Deliver contents; take back box (K)

ZAW3216

Failed delivery; return item as priority on the same day (E)

ZAW3217

Specific delivery date, deliver on ... (F)

ZAW3218

Deliver when all items have arrived (G)

ZAW3219

Deposit item (H)

ZAW3220

Follow delivery information in document pouch (I)

ZAW3222

Present item; leave in cellar (L)

ZAW3232

You require a contract with Post CH Ltd[\[2\]](#ffn4)

ZAW3233

Exchange/Return[\[3\]](#ffn5)

ZAW3234

Do not deliver to mailbox or neighbour: do not leave anywhere

2 [↑](#bffn4)  For the collection of empty containers or materials for recycling – please contact your customer advisor for further information.

3 [↑](#bffn5)  Only available in conjunction with notification service code 128 (“Exchange/return”).

DLC

Additional services

FRA

Fragile

MAN

Manual processing

RMP

Personal delivery

SI

Signature

AS

Signature (insurance)

COLD

ThermoCare Cold

BLN

Electronic COD

LQ

Limited Quantities (hazarouds goods)

AMB

ThermoCare Ambient

DLOG

Data logger

SA

Saturday delivery

ZFZ0912

Time slot delivery 9 –12[\[1\]](#ffn3)

ZFZ1114

Time slot delivery 11–14[\[1\]](#ffn3)

ZFZ1217

Time slot delivery 12–17[\[1\]](#ffn3)

ZF16302100

Time slot delivery 1630–21[\[1\]](#ffn3)

ZF16301800

Time slot delivery 1630–18[\[1\]](#ffn3)

ZF17301900

Time slot delivery 1730–19[\[1\]](#ffn3)

ZF18302000

Time slot delivery 1830–20[\[1\]](#ffn3)

ZF19302100

Time slot delivery 1930–21[\[1\]](#ffn3)


1 [↑](#bffn3)  When using the ZFZ (Time slot delivery) value-added service, we recommend you first perform an area check for each recipient address via your connection to the Digital Commerce API – Swiss Post shipping options.

DLC

Combination code (comprising multiple DLC)

PRISI

PRI + SI

PRI0912

PRI + ZFZ0912

PRI1114

PRI + ZFZ1114

PRISI09

PRI + SI + ZFZ0912

PRISI11

PRI + SI + ZFZ1114

DIRSUN9

DIR + Sun + 09-12

VLSI

VinoLog + SI

PRI1217

PRI + ZFZ1217

PRISI12

PRI + SI + ZFZ1217

PRI163021

PRI + ZF16302100

PRIS163021

PRI + SI + ZF16302100

PRI163018

PRI + ZF16301800

PRIS163018

PRI + SI + ZF16301800

PRI173019

PRI + ZF17301900

PRIS173019

PRI + SI + ZF17301900

PRI183020

PRI + ZF18302000

PRIS183020

PRI + SI + ZF18302000

PRI193021

PRI + ZF19302100

PRIS193021

PRI + SI + ZF19302100

DIR0912

DIR + ZFZ0912

DIR1114

DIR + ZFZ1114

DIR1217

DIR + ZFZ1217

DIR163021

DIR + ZF163021

DIR163018

DIR + ZF163018

DIR173019

DIR + ZF173019

DIR183020

DIR + ZF183020

DIR193021

DIR + ZF193021

DIRSI

DIR + SI

DIRSI09

DIR + SI + ZFZ0912

DIRSI11

DIR + SI + ZFZ1114

DIRSI12

DIR + SI + ZFZ1217

DIRS163021

DIR + SI + ZF163021

DIRS163018

DIR + SI + ZF163018

DIRS173019

DIR + SI + ZF173019

DIRS183020

DIR + SI + ZF183020

DIRS193021

DIR + SI + ZF193021

### 8.11 Overview of service codes (DLC) – Letters with barcode (BMB) domestic

DLC

Basic services

RINL

Registered (R) domestic

APLUS

A Mail Plus

DISP

Dispomail

APOST

A Mail

BPOST

B Mail individual items

DLC

Additional services

AR

Acknowledgement of receipt (AR)

BLN

Electronic cash on delivery (BLN)

CEC

Item for the blind (CEC)

RMP

Personal delivery (RMP)

eAR

Electronic return receipt

### 8.12 Overview of service codes (DLC) – Letters with barcode (BMB) international

DLC

Basic services

RETR, PRI

Registered (R) international PRIORITY

INTL

PRIORITY Plus

DLC

Additional services

AR

Acknowledgement of receipt (AR)

CEC

Item for the blind (CEC)

RMP

Personal delivery (RMP)

### 8.13 Generate Label request (Generate Label)

Element

Cardinality

Type

Description

Example (if appropriate)

**GenerateLabel**

1..1

GenerateLabel

Root element of Generate address label operation

\-

Language

1..1

Enumeration (de, fr, it, en)

Language in which the service is activated

de

**Envelope**

1..1

GenerateLabelEnvelope

Content holder

\-

**LabelDefinition**

1..1

GenerateLabelDefinition

Content holder with address label details

\-

LabelLayout

1..1

String (2, \[a-zA-Z,0-9\]{2})

Address label layout

A5

PrintAddresses

1..1

Enumeration (None, OnlyRecipient, OnlyCustomer, RecipientAndCustomer)

Details on the printing of sender’s and recipient’s address (delivery note)  
None – no addresses are printed   
OnlyRecipient – only the recipient’s address is printed  
OnlyCustomer – only the customer’s address is printed  
RecipientAndCustomer – Both the sender’s and the recipient’s addresses are printed

OnlyRecipient

ImageFileType

1..1

String (1..5,\[a-zA-Z,0-9\]{1,5})

Address label file format

PDF

ImageResolution

1..1

Integer

Address label resolution in DPI (dots per inch)

300

PrintPreview

1..1

Boolean

PrintPreview enabled/disabled (SPECIMEN lettering from the label generated)

true

**FileInfos**

1..1

GenerateFileInfos

Content holder

\-

FrankingLicense[\[2\]](#ffn8)

1..1

String (4..8,\[a-zA-Z,0-9\]{4} or \[0-9\]{6} or \[0-9\]{8})

Customer franking licence number or postcode

\-

PpFranking[\[4\]](#ffn10)

1..1

Boolean

Indicates whether the PP flag has been set or not

true

CustomerSystem

0..1

String(0..255), \[a-zA-Z,0-9,\\s\]{1,255}

Indicates optional parameters for customer system names

AVG Client

**Customer**

1..1

GenerateCustomer

Content holder with customer details. Refers to the sender’s customer

\-

Name1

1..1

String (0..25)

First name and surname, or company name

Meier AG

Name2

0..1

String (0..25)

Additional name 1 (company suffix or department)

General  
Agency

Street

1..1

String (0..25)

Address (house number and street)

Viktoriaplatz 10

POBox

0..1

String (0..25)

P.O. Box

P.O. Box 4021

ZIP

1..1

Integer (0..6)

Postcode

8048

City

1..1

String (0..25)

Place

Zurich

Country

0..1

(2,\[a-zA-Z\]{2})

Country as two-digit ISO-3166-1-alpha-2 code

CH

Logo

0..1

Binary (Base64)

Binary customer logo

\-

LogoFormat

0..1

String (3)

Logo format

GIF

LogoRotation

0..1

Enumeration (0, 90, 180, 270)

Clockwise rotation

270

LogoAspectRatio

0..1

Enumeration (EXPAND, KEEP)

Aspect ratio (width to height)

EXPAND

LogoHorizontalAlign

0..1

Enumeration (WITH\_CONTENT, LEFT)

Horizontal alignment

WITH\_CONTENT

LogoVerticalAlign

0..1

Enumeration (TOP, MIDDLE)

Vertical alignment

TOP

DomicilePostOffice

0..1

String (0..35)

Domicile Post Office

3097 Liebefeld

**Data**

1..1

GenerateData

Content holder

\-

**Provider**

1..1

GenerateProvider

Content holder

\-

**Sending**

1..1

GenerateSending

Content holder

\-

**SendingID**

0..1

String (0..50)

ID assigned by customer at request level is returned unchanged in the response.  
If no SendingID is supplied, WSBC generates a random number.

\-

**Item**

1..n

GenerateItem

Content holder per address label

\-

ItemID[\[2\]](#ffn8)

0..1

String (0..200)

ID assigned by customers at address label level will be returned unchanged in the response

\-

ItemNumber[\[2\]](#ffn8)

0..1

String (0..8,\[0-9\]{1,8})

Mailing number

12345678

IdentCode[\[2\]](#ffn8)

0..1

String (13..23, \[0-9\]{18} or \[0-9\]{23} or \[a-zA-Z,0-9\]{13})

Mailing code. For use by Swiss Post internal systems only. In systems external to Swiss Post this field is ignored and a warning returned.

9934123456  
12345678

Recipient[\[3\]](#ffn9)

1..1

Generate  
Recipient

Content holder with recipient details

\-

PostIdent

0..1

String (0..15)

Postal identification

\-

Title

0..1

String (0..35)

Salutation

Ms

PersonallyAddressed

0..1

Boolean

When set to FALSE, indicates the company first, then the recipient, on the address label Toggles to TRUE Default True.

True

Firstname

0..1

String (0..35)

First name of recipient

Melanie

Name1

1..1

String (0..35)

Last name and first name (if not in Firstname)

Steiner

Name2

0..1

String (0..35)

Additional name 1 (company suffix, department or keyword & PickPost / My Post 24 UserID)

Marketing Dept., PickPost 12345678 or MyPost24 12345678

Name3

0..1

String (0..35)

Additional name 2 (Attn/FAO; c/o or department \[if not in Name2\])

Attn/FAO  
Hans Meier

AddressSuffix

0..1

String (0..35)

Additional name address

East building

Street[\[1\]](#ffn7)

0..1

String (0..35)

Street

Viktoriastrasse

HouseNo

0..1

String (0..10)

House number

21

PoBox[\[1\]](#ffn7)

0..1

String (0..35)

Name “P.O. box” and – if available – P.O. box number

P.O. Box 4021

FloorNo

0..1

String (0..5)

Floor number (data transfer only, not printed on address labels)

3a

MailboxNo

0..1

Integer (0..10)

Letter box number (data transfer only, not printed on address labels)

10

ZIP

0..1

String (0..10)

Postcode

3030

City

1..1

String (0..35)

Place

Berne 1

Country

0..1

String (2,\[a-zA-Z\]{2})

Country – two-digit ISO 3166-1-alpha-2 code

CH

Hauskey

0..1

SInteger (0.. 13)

Hause key: authorized for internal Swiss Post Systems only

58096554

Phone

0..1

String (0..20)

Telephone number (for delivery instruction 3213)

031 338 11 11

Mobile

0..1

String (0..20)

Mobile number (for delivery instruction 3213)

031 338 11 11

Email

0..1

String (0..160)

E-mail address

h.muster@post.ch

**LabelAddress**

0..1

LabelAddress

Used in order to display the address lines in a customized order, or to specifically abbreviate long addresses. The postcode and location are taken across from the “Recipient” address block.

\-

LabelLine

2..5

String (0..35)

Contents of the recipient address lines, min. 2 and max. 5 address lines (the postcode and location fields are automatically taken across from the “Recipient” address block.

\-

**AdditionalINFOS**

0..1

GenerateAdditionalINFOS

Content holder

\-

**AdditionalData**

0..20

GenerateAdditionalData

Content holder

\-

Type

1..1  
1..1

String (0..35)

**General keys for electronic cash on delivery (BLN)** COD amount in CHF  
**Additional keys for BLN with ISR** ISR reference number

NN\_BETRAG  
NN\_ESR\_REFNR

Value

1..1

String (0..50)

Value for additional information, must be separated by decimal point (comma not allowed)

150.50

**Attributes**

0..1

GenerateAttributes

Content holder

\-

PRZL

1..n

String (1..7,\[a-zA-Z,0-9\]{1,7})

Service code (DLC)

ECO, PRI, SP

FreeText

0..1

String (0..34)

Free text for recipient address

Thank you for your order

DeliveryDate

0..1

Date

Delivery date (for delivery instruction 3217)

2009-08-20

ParcelNo

0..1

Integer (0..99)

Parcel number of total (for delivery instruction 3218)

2

ParcelTotal

0..1

Integer (0..99)

Total number of parcels (for delivery instruction 3218)

5

DeliveryPlace

0..1

String (0..35)

Drop point (for delivery instruction 3219)

At front door

ProClima

0..1

Boolean

Printing of ProClima logo

\-

**ReturnInfo**

0..1

ReturnInfoType

Content holder for returns information

\-

ReturnNote

0..1

Boolean

Return note, printed as text on the address label

\-

InstructionForReturns

0..1

Boolean

Instructions for returns, DmC is printed on the address label

\-

ReturnService

0..1

Integer (1)

Return service

5

CustomerIDReturnAddress

0..1

Integer (8)

Address ID return address, corresponds to the AMP key

16078484

**Dimensions**

0..1

Dimensions

Content holder for dimensions

\-

Weight

0..1

Integer (0..99’999)

Weight in grams (limited to 5 digits) for Parcels, Express and Solutions service groups

12500

**UNNumbers**

0..1

\-

Content holder for UN number for the “LQ” (hazardous goods) additional service

\-

UNNumber

0..n

Integer (0..9’999)

List of UN numbers (limited to 4 digits) for “LQ” additional service (hazardous goods)

1234, 1235, 1236

**Notification**

0..15

Generate Notification

List of notification services

\-

Type

1..1

String (Mail, SMS)

Means of communication

SMS or EMAIL

Service

1..1

Integer (0..20)

Service code

1, 2, 128

FreeText1

0..1

String (0..160)

Free text 1

Test 1

FreeText2

0..1

String (0..512)

Free text 2

Test 2

Language

1..1

Language

Language

DE, FR, IT or EN

**Communication**

1..1

Generate-Communication

Content holder for communication medium

Email or Mobile

Email

0..1

String (0..160)

E-mail address

a@b.ch

Mobile

0..1

String (9..20)

Mobile number

+41791234567


1 [↑](#bffn7)  Domestic Parcels, Express and Solutions: either address **or** P.O. box permitted. BMB domestic: state address **and** P.O. box with number (if applicable). P.O. Box details are compulsory fpr Dispomail and Dispomail Easy. BMB international: no rules. All address components must be split between Address 1 and Address 2.


2 [↑](#bffn8)  Validation logic for FrankingLicence, ItemID, ItemNumber IdentCode and Hauskey fields:

*   FrankingLicence: Mandatory (left-pad with zeros up to 8 digits)
*   ItemID: Optional, any value
*   ItemNumber: Optional, any value. If filled in, validation for uniqueness. If ItemNumber is empty, the item number is generated and the identcode is generated from this item number and the franking licence.
*   IdentCode and Hauskey: Not permitted. If this field is filled, it will be ignored and a warning will also be returned. IdentCode is provided solely for internal calls at Swiss Post.


3 [↑](#bffn9)  With the basic services with GAS, the recipient is the return address in accordance with the contractual terms for business reply items.


4 [↑](#bffn10)  The postage paid impression for the Letters with barcode (BMB) domestic and international service groups does not appear automatically in the address and applies to each request.

#### 8.11.1 Posting BLN (electronic COD) via “Barcode” web service (for “Parcels” and “Swiss-Express”)

##### BLN-defined transaction types

If you already use the “Barcode” web service actively and would later like to programme BLN, we can provide a test environment for you. Please contact Web Service Support for further information.

The credit note for COD amounts can be applied by means of two different account types:

1.  Yellow Account with inpayment slip (IS) from PostFinance, with upper limit on domestic transactions.
2.  By Swiss Post ISR.

With transaction type 1 (yellow Account IS) only the COD amount is required. With transaction type 2 (Swiss Post ISR) both the COD amount and the ISR reference number is required.

##### ISR reference number

For the ISR reference number, the following data format is valid (excerpt from the PostFinance manual on “Record Structures – electronic Services”) [www.postfinance.ch/content/dam/pf/de/doc/consult/manual/dldata/efin\_recdescr\_man\_en.pdf](www.postfinance.ch/content/dam/pf/de/doc/consult/manual/dldata/efin_recdescr_man_en.pdf)

*   Reference number
*   84
*   9(27)
*   For 5-digit ISR customer numbers 000000000000999999999999999
*   For 9-digit ISR customer numbers 99999999999999999999999999P
*   Mandatory: The reference number is printed on the processing document in blocks of 5, whereby leading zeros can be suppresses. The details must be entered in the field with right alignment, empty positions must be extended with leading zeros. Reference numbers with the value “0” (zero) will be rejected. We recommaned that you recalculate and compare the check digit (modulo 10, recursive).

##### Yellow Account with inpayment slip (TransactionType 1)

Element

Cardinality

Type

Description

Example (if appropriate)

**AdditionalINFOS**

0..1

\-

Content holder

\-

**AdditionalData**

0..20

\-

Content holder

\-

Type

1..1

String (0..35)

Field for COD amount

NN\_BETRAG

Value

1..1

String (0..50)

COD amount, must be separated by decimal point (comma not allowed)

150.50

Type

1..1

String (0..35)

Field for ISR reference number

NN\_ESR\_REFNR

Value

1..1

String (0..50)

Reference number

Reference number

#### 8.11.2 Notification services

##### Notification code

In the “Notification” element, the “Service” field has the following valid values:

Notification

Code

Proof of posting

1

Delivery information

2

Collection information

4

Reminder to recipient

32

Handover status to sender

64

“Exchange/return”

128[\[1\]](#ffn11)

Saturday delivery

256[\[2\]](#ffn12)


1 [↑](#bffn11)  This notification service can only be used with delivery instruction ZAW3233.


2 [↑](#bffn12)  This notification service can only be used with the additional service “SA”.


3 [↑](#bffn13)  This notification service can only be used with the additional service “AZS” and the basic service PostPac Priority, bulky goods Priority, PostPac Economy or bulky goods Economy.


4 [↑](#bffn14)  This notification service can only be used with the additional service “AZS” and the basic services SameDay afternoon/evening, SameDay afternoon/evening bulky goods or “Direct”.

##### Notification text messages

The description of the content of SMS and e-mail messages as well as technical specifications regarding free text are available at www.swisspost.ch/post-e-log-avisierungsservices-details.

### 8.14 Retrieve the Barcode Label

#### POST request

    https://wedecint.post.ch/api/barcode/v1/generateAddressLabel

Copy

#### POST payload

    {
    “language”:”DE”,
    “frankingLicense”: “35900034”,
    “customer”: {
    “name1”: “Test Kunde”,
    “street”: “Wankdorfallee 4”,
    “zip”: “3030”,
    “city”: “Bern”,
    “domicilePostOffice”:”3011 Bern”,
    “country”: “CH”
    },
    “labelDefinition”: {
    “labelLayout”: “A6”,
    “printAddresses”: “RECIPIENT_AND_CUSTOMER”,
    “imageFileType”: “JPG”,
    “imageResolution”: 300
    },
    “item”:
    {
    “itemID”: “1234567”,
    “recipient”: {
    “name1”: “Hans”,
    “name2”: “Muster”,
    “street”: “Wankdorfallee”,
    “houseNo”: “4”,
    “zip”:”3030”,
    “city”: “Bern”,
    “country”: “CH”
    },
    “attributes”: {
    “przl”: [“PRI”,”SA”],
    “weight”:12345
    }
    }
    }

Copy

#### Response

    {
    “labelDefinition” : {
    “labelLayout” : “A6”,
    “imageFileType” : “jpg”,
    “imageResolution” : 300,
    “printPreview” : false,
    “colorPrintRequired” : true
    },
    “item” : {
    “itemID” : “1234567”,
    “identCode” : “993590003400000002”,
    “label” : [ “<Base64Image>” ]
    }
    }

Copy

#### 8.14.1 Error messages

Every error message consists of a four-digit error code prefixed by “E” (E1234), beginning at E1000, plus an associated error text. The web service returns the error texts in the specified language (German, French, Italian or English).

If an error message is returned, the requested service is not executed and is rejected. The error must be corrected and the call repeated.

The curly brackets are placeholders and are replaced by the relevant values in the actual error message.

Error code

Error message (English)

E1000

The desired combination of the basic service codes ({0}) is invalid.

E1001

The desired additional services ({0}) cannot be combined with the requested basic service ({1}).

E1002

The requested additional services ({0}) cannot be combined with each other.

E1003

The desired delivery instructions ({0}) cannot be combined with the requested basic service ({1}).

E1004

The desired delivery instructions ({0}) cannot be combined with the requested additional services ({1}).

E1005

The requested delivery instructions ({0}) cannot be combined with each other.

E1006

The stated number of basic and additional service codes ({0}) exceeds the maximum number of presentable service codes for the desired presentation type. Please reduce the number of service codes or choose a larger presentation format.

E1007

The stated number of delivery instructions ({0}) exceeds the maximum number of presentable delivery instructions for the desired presentation type. Please reduce the number of delivery instructions or choose a larger presentation format.

E1008

The desired service group is invalid.

E1009

The desired basic service is invalid.

E1011

The stated presentation type ({0}) is invalid.

E1012

The stated service code ({0}) is invalid for the desired service group ({1}).

E1013

The stated presentation type ({0}) is invalid for the desired service group ({1}).

E1014

An additional service with COD amount must be selected (BLN).

E1015

No valid basic service codes could be found in the list of service codes ({0}).

E1016

The presentation time ({0}) is invalid for the basic service selected ({1}).

E2001

A valid recipient address must be stated.

E2002

A domicile post office must be provided.

E2003

The COD amount must be provided for COD items (BLN).

E2004

The ISR reference number for the electronic COD (BLN) is invalid. Please check the format and check digit.

E2005

A telephone number must be provided for the delivery instruction “Notify delivery by telephone” (ZAW3213).

E2006

A valid delivery date must be stated for the delivery instruction “Specific delivery date: deliver on ...” (ZAW3217).

E2007

A “parcel number” and a “parcel total” must be provided for the delivery instruction “Deliver when all items have arrived” (ZAW3218).

E2008

A “deposit point” must be provided for the delivery instruction “Deposit consignments” (ZAW3219).

E2009

No franking licence was indicated.

E2010

This account is not authorized to purchase addresses for the franking licence ({0}).

E2011

The consignment number provided is outside the area of validity (1–{0}).

E2012

The desired picture format ({1}) is not offered. Please select a valid picture format ({1}).

E2013

The desired resolution ({0} dpi) is not offered. Please select a valid picture format ({1}).

E2014

VinoLog deliveries are not possible for the desired recipient postcode ({0}).

E2015

VinoLog deliveries in combination wtih evening delivery (ZAW3229) are not possible for the desired recipient postcode ({0}).

E2016

The consignment number provided is not unique.

E2017

A valid sender address must be stated.

E2018

The indicated sender logo format ({0}) is not permitted.

E2019

The sender logo exceeds the maximum size of {0} KB.

E2020

The COD amount is outside the valid range.

E2021

Two COD amounts were indicated. Please indicate the amount for COD items (N) in the “ATT\_Amount” field and in the “REC\_DATA” field for electronic COD items (BLN).

E2024

The sender logo could not be scanned. Please check that it conforms to a valid picture format ({0}).

E2025

A P.O. box address must be specified.

E2026

The franking licence used ({0}) is not authorized for the basic service ({1}) of the service group ({2}).

E2027

The franking licence used ({0}) is not the correct length.

E2028

{0} is not a valid ISO country code.

E2029

Additional service {0} is not permitted for mailings to {1} or only in combination with another additional service.

E2030

Basic service {0} does not belong to an international service group.

E2031

Basic service {0} does not belong to a domestic service group.

E2032

For domestic mailings the addressee’s postcode must be specified.

E2033

For domestic mailings the addressee’s postcode must consist of digits only.

E2034

For domestic mailings the addressee’s postcode must not exceed the maximum length of {0} characters.

E2035

This franking licence ({0}) is not a customer franking licence and a valid consignment barcode must therefore be specified.

E2036

The weight should be express as a maximum of 5 digits and should not amount to more than {0} grams (e.g.29500).

E2037

The weight must be a value great than 0.

E2038

The UN number must be exactly 4 digits (e.g. 1234).

E2039

The LQ additional service is only available from Version 2.1 onwards.

E2040

The notification service {0} cannot be combined with the basic service {1}.

E2041

The e-mail address ({0}) must correspond to the following pattern {1}.

E2042

The telephone number ({0}) must be between 10 and 20 digits long and must begin with {1}, {2} or {3}. Numbers and spaces are permitted; hyphens (-) or forward slashes (/) or other special characters (|, \\, ^, etc.) are not permitted.

E2043

For the {0} notification the delivery instruction {1} is required.

E2044

For the {0} delivery instruction the notification {1} is required.

E2045

The following characters only are permitted in the Item ID: A to Z (and a to z), numbers 0 to 9, underscore “\_”, hyphen “-”, plus sign “+”.

E2047

The communication type (e-mail or SMS) does not match the telephone number or e-mail address given.

E2049

For COD items (BLN), a valid ISR customer number (NN\_ESR\_KNDNR) must be set.

E2050

For COD items (BLN), a valid IBAN number (NN\_IBAN) must be set.

E2051

For COD items (BLN) with IBAN, a valid name for the end beneficiary (NN\_END\_NAME\_VORNAME) must be set.

E2052

For COD items (BLN) with IBAN, a valid additional description for the end beneficiary (NN\_END\_ZUSATZ\_NAME) must be set.

E2053

For COD items (BLN) with IBAN, a valid street (NN\_END\_STRASSE) must be set.

E2054

For COD items (BLN) with IBAN, a valid postcode (NN\_END\_PLZ) must be set.

E2055

For COD items (BLN) with IBAN, a valid city (NN\_END\_ORT) must be set.

E2056

For COD items (BLN) with IBAN or ISR account number (NN\_ESR\_KNDNR), a valid sender contact e-mail address (NN\_CUS\_EMAIL) must be specified.

E2057

For COD items (BLN) with IBAN or ISR account number (NN\_ESR\_KNDNR), a valid sender contact phone number (NN\_CUS\_PHONE or NN\_CUS\_MOBILE) must be specified.

E2058

For COD items (BLN), a combination of ISR and IBAN fields is not permitted.

E2059

The basic service {0} can only be used in conjunction with the value-added service {1}.

E2060

The notification service {0} can only be used together with the value-added service {1}.

E2061

The recipient address could not be determined – check for evening delivery not possible.

E2064

At least two LabelLines are required. LabelLines that are blank or only contain empty spaces are not permitted.

E2065

The checking of the evening delivery cannot be carried out at the moment. Please contact Support.

E2066

The service {0} is not available for this address.

E9991

The output format for single barcodes is currently not supported.

E9992

No valid web service call!

E9993

The Zubofi system is currently not available. Please try again later.

E9994

The Kurepo system is currently not available. Please try again later.

E9995

The output format ({1}) is currently not supported in resolution ({0} dpi).

E9996

Too many addressees were requested. A maximum of {3} addressees per request can be generated with the resolution ({0} dpi), output format ({1}) and addressee format ({2}).

E9997

The web service barcode was unable to generate a unique consignment number. If the problem reoccurs, please contact the Support team.

E9998

User {0} is not authorized for this service.

E9999

The service is not available at the moment.



#### 8.14.2 Warnings

Every warning consists of a four-digit warning code prefixed by “W” (W1234), beginning at W1000, plus an associated warning text. The web service returns the warning texts in the specified language (German, French, Italian or English). An operation may return more than one warning at a time.

If a warning is returned, the requested service is executed, taking the warning into account. Warnings help to optimize your use of the “Barcode” web service.

The curly brackets are placeholders and are replaced by the relevant values in the actual warning.

Warning code

Warning (English)

W2003

You have indicated a COD amount without requesting the additional service “COD” (BLN).

W2004

You have indicated an ISR reference number without requesting the additional service “Electronic COD” (BLN).

W2005

You have indicated a telephone number without requesting the delivery instruction “Notify delivery by telephone” (ZAW3213).

W2006

You have indicated a delivery date without requesting the delivery instruction “Specific delivery date: delivery on ...” (ZAW3217).

W2007

You have indicated a parcel number and/or a parcel total without requesting the delivery instruction “Deliver when all consignments have arrived” (ZAW3218).

W2008

You have indicated a deposit point without requesting the delivery instruction “Deposit consignments” (ZAW3219).

W2009

A text cannot be generated with the requested presentation type ({0}); the display will be suppressed. To display a text, please select a larger presentation type.

W2010

Generation of the addressee will require too much time with the requested presentation type ({0}) and resolution ({1}).

W2011

PP franking is ignored for the basic service {0}.

W2012

For in-house Swiss Post applications the weight field is optional. The weight field has not been filled in correctly (max. {0} grams, greater than or the same as 0) and has therefore not been returned.

W2013

Free text 2 cannot be used with SMS notifications. Free text 2 has been ignored.

W2014

Free text is not required in the notification {0}. Free text 1 and 2 have been ignored.

W2016

For the chosen presentation type the recipient is restricted to {0} lines. The title will be ignored. To display the title, please select a larger presentation type.

W2017

For the chosen presentation type the recipient is restricted to {0} lines. The AddressSuffix will be ignored. To display the AddressSuffix, please select a larger presentation type.

W2018

For an A6 label with 2 ZAWs, the sender’s address is truncated to 20 characters per line in ZPL2 format.

W2019

For the chosen presentation type the recipient is restricted to {0} lines. Name3 will be ignored. To display Name3, please select a larger presentation type.

W2020

For the chosen presentation type the recipient is restricted to {0} lines. Free text will be ignored. To display the free text, please select a larger presentation type.

W2021

For the chosen presentation type, the recipient is restricted to {0} lines. Excessive LabelLines will be ignored. To display all LabelLines, please select a larger presentation type.

W2022

The specified weight will not be printed on the label as this information is not relevant to the selected service group.

W9997

The Consignment code field may be filled in using Swiss Post applications only.

9\. pick@home
-------------

### 9.1 Prerequisits

*   OAuth access token for scope PICKHOME. This scope does not require the consent of the end user and can be obtained from an authorization code grant flow or an implicit grant flow or a client credential grant flow.
*   Bearer access token stored in the header of the request:  
    Authorization: Bearer <OAuth access token>
*   Pick@home API | INT | [h](https://serviceint2.post.ch/rhe/rhe-public/doc/assets/index.html?api=https://serviceint2.post.ch/rhe/rhe-public/doc/pah-order-api/v1/rest/swagger)[ttps://serviceint2.post.ch/rhe/rhe-public/doc/assets/index.html?api=https://serviceint2.post.ch/rhe/rhe-public/doc/rhe-order-api/v1/rest/swagger](https://serviceint2.post.ch/rhe/rhe-public/doc/assets/index.html?api=https://serviceint2.post.ch/rhe/rhe-public/doc/pah-order-api/v1/rest/swagger)
*   pick@home API | PROD | [https://service.post.ch/rhe/rhe-public/doc/assets/index.html?api=https://service.post.ch/rhe/rhe-public/doc/rhe-order-api/v1/rest/swagger](https://service.post.ch/rhe/rhe-public/doc/assets/index.html?api=https://service.post.ch/rhe/rhe-public/doc/rhe-order-api/v1/rest/swagger)

### 9.2 Order

#### 9.2.1 Order Resource Structure

A order resource has the following structure:

![](https://developer.post.ch/-/media/post-maxisites/developer/images/pickathome.jpg?mw=1600&vs=2&hash=F78BD0261DC4C6AD3EA370218C7D3153)

#### 9.2.2 Order Resource State Event Diagram

![](https://developer.post.ch/-/media/post-maxisites/developer/images/pickathome2.jpg?mw=1600&vs=2&hash=8DC76D37D816B5C2B36454276478F638)

#### 9.2.3 Common Sequence creating a order

*   Create a order resource (POST /orders) minimally providing the pickupAddress and a parcel.
*   Set the approval for the order (POST /orders/{orderKey}/approval). Order can't be updated after this call.

A created order that is not confirmed within 60 minutes will be deleted.

![](https://developer.post.ch/-/media/post-maxisites/developer/images/pickathome3.jpg?mw=1600&vs=2&hash=4FFAB7CEC99C825A8FFEF34D65515667)

### 9.3 Error and warning messages

Errors and warnings are returned in the following format:

    {
            "orderKey": "0fb4efdf-e8b1-4593-a20c-39ab7fc6bb36",
            "errors": [
                {
                    "code": 1000,
                    "description": "Pickup address is required"
                }
            ],
            "warnings": [
                {
                    "code": 1102,
                    "description": "The length of lastname is invalid"
                }
            ]
        }

Copy

**Important**: be aware that the status code of a request can be 200 but the response contains errors and warnings. Do not treat a status code 200 as a successful call per se. Always evaluate the error and warning arrays.

Missing required fields

Code

Description

PICKUP\_ADDRESS\_REQUIRED

1000

Pickup address is required

STREET\_REQUIRED

1001

The street is required

ZIP\_REQUIRED

1002

The zip is required

CITY\_REQUIRED

1003

The city is required

SENDER\_ADDRESS\_REQUIRED

1004

Sender address is required

SENDING\_TYPE\_REQUIRED

1005

Parcel sending type is required

WEIGHT\_TYPE\_REQUIRED

1006

Parcel weight type is required

SIZE\_TYPE\_REQUIRED

1007

Parcel size type is required

LANGUAGE\_REQUIRED

1008

"Order language is required

PICKUP\_PLACE\_REQUIRED

1009

Pickup place is required

Data validation messages

Code

Description

TITLE\_LENGTH\_INVALID

1100

The length of title is invalid

TITLE\_FREE\_TEXT\_LENGTH\_INVALID

1101

The length of title free text is invalid

NAME\_LENGTH\_INVALID

1102

The length of name is invalid

FIRSTNAME\_LENGTH\_INVALID

1103

The length of first name is invalid

COMPANY\_LENGTH\_INVALID

1104

The length of company is invalid

STREET\_LENGTH\_INVALID

1105

The length of street is invalid

HOUSENR\_LENGTH\_INVALID

1106

The length of houseNumber is invalid

ZIP\_LENGTH\_INVALID

1107

The length of zip is invalid

CITY\_LENGTH\_INVALID

1108

The length of city is invalid

POBOX\_LENGTH\_INVALID

1109

The length of poBox is invalid

ADDADR1\_LENGTH\_INVALID

1110

The length of additional address 1 is invalid

ADDADR2\_LENGTH\_INVALID

1111

The length of additional address 2 is invalid

EMAIL\_LENGTH\_INVALID

1112

The length of email is invalid

MOBILE\_LENGTH\_INVALID

1113

The length of mobile is invalid

SENDER\_REF\_LENGTH\_INVALID

1114

The length of sender reference is invalid

RECEIVER\_REF\_LENGTH\_INVALID

1115

The length of receiver reference is invalid

GOODS\_DESC\_LENGTH\_INVALID

1116

The length of goods description is invalid

CUST\_ORDER\_REF\_LENGTH\_INVALID

1117

The length of customer order reference is invalid

ADD\_PICKUP\_INFO\_LENGTH\_INVALID

1118

The length of addtional order information is invalid

ORDER\_DESC\_LENGTH\_INVALID

1119

The length of order description is invalid

PICKUP\_FLOOR\_LENGTH\_INVALID

1120

The length of pickup floor is invalid

PHONE\_NOTIFY\_LENGTH\_INVALID

1121

The length of phone notify is invalid

CONTACT\_PERSON\_LENGTH\_INVALID

1122

The length of contact person is invalid

Logical data validation messages

Code

Description

NAME\_AND\_FIRSTNAME\_AND\_COMPANY\_COMBINATION\_INVALID

1200

The combination last name/ first name and company is not allowed

ADDRESS\_NOT\_VALID

1201

Pickup address not valid.

SENDER\_ADDRESS\_NOT\_VALID

1202

Sender address not valid.

EMAIL\_NOT\_VALID

1203

Please enter a valid e-mail address

MOBILE\_NOT\_VALID

1204

Please enter a valid Swiss mobile number

PHONE\_NOT\_VALID

1205

Please enter a valid Swiss phone number

Profile validation messages

Code

Description

PICKUP\_PLACE\_NOT\_VALID

1300

The pickupPlace is not valid. Valid values are configured in profile.

SENDING\_TYPE\_NOT\_VALID

1301

SendingType ist not valid. Valid values are configured in profile.

PICKUP\_TIME\_NOT\_VALID

1302

The pickupTime is not valid. Valid values are configured in profile.

EMAIL\_FOR\_CONFIRMATION\_NOT\_PROVIDED

1303

You must provide an E-Mail. Behaviour configured in profile.

MOBILE\_FOR\_CONFIRMATION\_NOT\_PROVIDED

1304

You must provide an mobile number. Behaviour configured in profile.

CONFIG\_NOT\_FOUND

1305

With the given information we couldn't find the corresponding configuration.

Business validation messages

Code

Description

PICK\_UP\_SERVICE\_NOT\_OFFERED

2000

The collect domestic parcels for return service is not available at the selected pickup address.

ORDER\_LIMIT\_FOR\_EXTERNAL\_CUSTOMER\_EXCEEDED

2001

For security reasons, the number of orders per user per day is limited. This limit has been reached. Therefore, you cannot place any more orders today.

TOO\_MANY\_PARCELS\_PER\_ADDRESS\_CONCURRENT

2002

You can request collection for a maximum of 5 parcels per pick-up address and pick-up date. Another user may already have registered parcels for this pick-up address.

AT\_LEAST\_ONE\_PARCEL\_MUST\_BE\_ORDERED

2003

At least one parcel must be ordered.

AUXILIARIES\_NOT\_VALID

2004

The desired additional services can not be combined.

PICKUP\_DATE\_NOT\_VALID

2005

The pickupDate is not valid. You must choose a date in the future but not more than 10 working day (Swiss public holiday are not working days).

ORDER\_TOO\_LATE\_FOR\_PACKAGING

2006

After 17:00 the packaging cannot be ordered if the pickup-up date is the next day.

NOT\_ALL\_LABELS\_COULD\_BE\_GENERATED

2007

Not all labels could be generated.

FILE\_FORMAT\_NOT\_SUPORTED

2008

Given file format is not supported.

Identcode/licence validation messages

Code

Description

IDENT\_CODE\_ALREADY\_USED

2100

The consignment number has already been used once in the last 90 days, which means it is not valid.

IDENT\_CODE\_NOT\_VALID

2101

Invalid consignment number.

LICENCE\_01\_FOR\_GAS\_NOT\_ALLOWED

2102

GAS licence cannot begin with 01

IDENT\_CODE\_NOT\_NUMERICAL

2103

Consignment no. may only have digits

IDENT\_CODE\_LENGTH\_INVALID

2104

Consignment no. length must be 18

GAS\_LICENCE\_BLOCKED

2105

Blocked GAS franking licence.

LICENCE\_PATTERN\_MISSMATCH

2106

Franking licence cannot be empty.

LICENCE\_NOT\_EXISTENT

2107

Consignment no (second and third part of consignment number) cannot be found.

GAS\_LICENCE\_CASH\_PAYER

2108

Invalid franking licence. Please contact Customer Service on +41 58 338 37 77.

IDENT\_CODE\_NOT\_MATCH

2109

Given identcode in parameter and model are not equal.

Order messages

Code

Description

ORDER\_NOT\_FOUND

4000

Order not found

PARCEL\_NOT\_FOUND

4001

Parcel not found.

IDENTCODE\_MUST\_BE\_PROVIDED

4002

Valid identcode must be provided.

Process messages

Code

Description

ORDER\_ALREADY\_APPROVED

6000

The given order is already approved.

ORDER\_WRONG\_STATE

6001

The order is in a wrong state for this action.

PARCEL\_WRONG\_STATE

6002

The parcel is in a wrong state for this action.



10 Registration
---------------

A client of the Swiss Post Digital Commerce API must be registered at Swiss Post. No online registration feature supports the registration process at the moment. To obtain access to Digital Commerce API, please contact your Swiss Post Sales Representant or [digitalintegration@post.ch](mailto:digitalintegration@post.ch).

### 10.1 Artifacts

##### Artifact

*   Client Identifier (Secret-ID)
*   Client Secret (Token-ID)

##### Remark

These credentials have to be communicated when ordering an access token.

### 10.2 Required information

Attribute

Remark

Related API

Domain name

The domain name of the web shop

Login Post Connector / Alternative Delivery Addresses

i18n client text

A text for the web shop to be displayed in the consent screen

Login Post Connector / Alternative Delivery Addresses

Icon

An icon for the web shop to be displayed in the consent screen

Login Post Connector / Alternative Delivery Addresses

redirect\_uri

Used for returning the data when consuming both authorization and token endpoints. Domain name of the URI is expected to be the one specified above.

Login Post Connector / Alternative Delivery Addresses

scopes

The scopes configured for a client. Every scope gives access to an endpoint or to a set of personal attributes (OpenID). The end user may have to give his consent during the OAuth authorization flows.

all

Debitor number

The identification number of business clients

Barcode / pick@home



11 CORS
-------

**Cross Origin Resource Sharing** is a relaxing technique for the **cross origin policy**. This policy is imposed by web browsers in order to minimize cross origin problems. It does not concern requests initiated on the server side.

Any client who wants to consume an endpoint of the Swiss Post e-commerce API from the client-side (for example with Javascript) will face the cross origin policy because the domains where the client and the Swiss Post e-commerce API are running are different.

The domain of the client must be registered in the CORS configuration of the API proxy server. This step is part of the registration process.