# mod_sts
A security token exchange module for Apache HTTP Server 2.x which allows for exchanging arbitrary security tokens by calling into a remote Security Token Service (STS).

## Overview
This Apache module allows for exchanging a security token (aka. "source token") that is presented on an incoming HTTP request for another security token (aka. "target token") by calling into a Security Token Service (STS) and then include that target token on the propagated HTTP request to the content or origin server.

This can be used in scenario's where an Apache server is put in front of a backend service as a Reverse Proxy/Gateway that handles tokens presented by *external* clients but needs to forward those requests using some internal security token format, acting as an *internal* client to the backend service. Note that the backend service can also be an application that is hosted on the Apache server itself, e.g. a PHP application.

## Rationale
The split between external tokens and internal tokens may be enforced for security reasons i.e. separating external requests from internal requests/tokens whilst keeping "on-behalf-of-a-user" semantics, or for legacy reasons i.e. when your backend only supports consuming a proprietary/legacy token format/protocol and you don't want to enforce support for that legacy onto your external clients (or vice versa).

## Tokens

##### Source
An source (or: incoming) token can be presented in a header (e.g. an `Authorization: bearer` header for OAuth 2.0 bearer access tokens), a query parameter or a cookie. Alternatively the token can be consumed from an environment variable set by a another Apache (authentication) module such as a validated access token set by [mod_auth_openidc](https://github.com/zmartzone/mod_auth_openidc) in OAuth 2.0 Resource Server mode.

Sample supported - incoming/external - source tokens:
- an OAuth 2.0 bearer access token presented by an external OAuth 2.0 Client
- a generic JWT presented in a header or query parameter
- a generic cookie
- a vendor specific token - e.g. an OpenToken produced by PingFederate - or a vendor specific cookie such an SSO cookie produced by CA SiteMinder or Oracle Access Manager

##### Target
A target (or: outgoing) token can be appended in a header (e.g. an `Authorization: bearer` header for OAuth 2.0 bearer access tokens), a query parameter or a cookie but the token can also be set as an environment variable so it can be consumed by another Apache module or by an application that is served from the Apache server, e.g. a PHP application.

Sample supported - outgoing/internal - target tokens:
- an OAuth 2.0 bearer access token, scoped to an internal service security domain
- a generic JWT put in a header
- a generic cookie
- a vendor specific token - e.g. an OpenToken produced by PingFederate - or a vendor specific cookie such an SSO cookie produced by CA SiteMinder or Oracle Access Manager

## Security Token Service Protocols
This module supports a number of different protocols for interfacing with a Security Token Service:

##### WS-Trust
XML/SOAP based OASIS standard, see: [https://en.wikipedia.org/wiki/WS-Trust](https://en.wikipedia.org/wiki/WS-Trust)

##### OAuth 2.0 Token Exchange
REST based IETF standard, see: [https://www.ietf.org/id/draft-ietf-oauth-token-exchange](https://www.ietf.org/id/draft-ietf-oauth-token-exchange)

##### OAuth 2.0 Resource Owner Password Credentials (ROPC)
Essentially a workaround for communicating with servers that don't support any of the two options above but can be configured/programmed to validate a token presented in the `password` parameter of the OAuth 2.0 Resource Owner Password Credentials grant and return a target token in the `access token` claim of the token response.

## Configuration
For an exhaustive description of all configuration options, see the file `sts.conf`
in this directory. This file can also serve as an include file for `httpd.conf`.

## Support

#### Community Support
For generic questions, see the Wiki pages with Frequently Asked Questions at:  
  [https://github.com/zmartzone/mod_sts/wiki](https://github.com/zmartzone/mod_sts/wiki)  
Any questions/issues should go to issues tracker.

#### Commercial Services
For commercial Support contracts, Professional Services, Training and use-case specific support you can contact:  
  [sales@zmartzone.eu](mailto:sales@zmartzone.eu)  


Disclaimer
----------
*This software is open sourced by ZmartZone IAM. For commercial support
you can contact [ZmartZone IAM](https://www.zmartzone.eu) as described above in the [Support](#support) section.*
