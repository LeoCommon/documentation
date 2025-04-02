# Security Guide

This security guide gives an overview of the security mechanisms, that the LeoCommon system uses for securing the system and its communication.

## Update (Hawkbit) Server

For authenticating a ground station a 32-digit-hex-value (128 bit), the so called HAWKBIT_SECURITY_TOKEN, is used. This token and the HAWKBIT_CONTROLLER_ID are stored in the .hawkbit file on the configuration USB-stick. 
Also the server uses Let's Encrypt to provide only authenticated TLS connections (TLS1.3, 128 bit AES, 2048 bit RSA) to connecting devices.

## C&C Server

The Server uses Let's Encrypt to provide only authenticated TLS connections (TLS1.3, 128 bit AES, 2048 bit RSA) to connecting devices.

### Server to User Connections
The website requests a user password between 6 and 64 characters. On the server side the password is UTF-8 encoded, salted and hashed. Salt and hash are stored in the user data base. For the cryptography routines the python library bcrypt is used.
After a login via name and password, a user receives two JWTs: one access token and one refresh token. These tokens are stored in the browser as session cookies. To mitigate XXS we use the same-origin policy, by setting the policy in the nginx-config.
The access token is used for accessing the REST API that provides the data for the website. Once the access token is expired, the refresh token can be used once to get a new access-refresh-token-pair. 

User access tokens have a short lifetime of only 5 minutes. Their refresh tokens have a lifetime of 2 hours. This combination is a good trade-off between security and performance: It allows short blacklists for blocking revoked access tokens and holds only a linear sized (linear in number of users + groundstations) white-list for refresh tokens. 

TODO: On the server side the user password length is not checked!

### Server to Ground Station Connections
The ground station has two JWTs: one access token and one refresh token. These tokens are stored on the config.toml on the configuration USB-stick. 

Ground station access tokens have a lifetime of 24 hours and refresh tokens have 3 years. This again is a acceptable trade-off between security, performance and usability. The long lifetime of refresh tokens allow long ground station disconnects (up to 3 years) without a manual token update of the owner. This is important for the usability/reliability.

### Server REST API
The endpoints of the REST API (and the method behind it) require a authentication. Nearly all endpoints require a valid access token (JWT). Only the two endpoints 'login/userlogin' and 'login/refresh' require a username-password combination and a refresh token (JWT).
Besides this, the actions normal users and ground stations can perform are limited, to reduce the potential harm in case a token gets leaked. (If an access token gets leaked it will expire relatively soon. If a refresh token gets leaked it can be used only once. Both reduces the attack surface and increases the effort for attackers.)

One exception that works without authentication is the sensors/get_locations' endpoint, which returns two lists of locations where the online and offline ground stations are located. The positions are given in latitude/longitude and are rounded to the second decimal after the dot (x.xx°) to not reveal the precise position of the ground station to the unauthorized public.

## Ground Station

The login credentials of the ground station are stored on the configuration USB-stick. This allows to build one image and individualize only the configuration files on the USB-sticks. A malicious actor with access to the USB-stick is considered out of scope for prevention. The proper reaction on such an attacker is the revocation of the refresh token. This will automatically blacklist the access token that belongs to this refresh token.

### C&C Server Connection
This connection is created and utilized by the ground station client, that is running inside the redundant operating systems.

The code that is running in the ground station operating system is reduced to a minimum to keep the potential attack surface as small as possible. This is also the reason why there is no SSH client currently available on the ground station. 
The ground station is regularly pulling for jobs on the C&C server. Only a defined list of jobs can be executed on the ground stations. This design choice was made to prevent a scenario where a ground station could turn into a jamming-bot of a bot-net, even if the C&C server could be corrupted. 

### Update Server Connection
To enable updates, the RAUC updater is maintaining a connection to the update server (Hawkbit). When a update is available, it will be downloaded. After this the signature of the update is checked. Only if the signature of the update matches the installed certificate, the update will be rolled out. The certificate is installed in RAUC during the compiling (building) of the system. This prevents malicious updates from an untrusted source.

