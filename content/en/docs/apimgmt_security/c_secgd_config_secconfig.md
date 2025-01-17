{
"title": "Security configuration",
"linkTitle": "Security configuration",
"weight": 60,
"date": "2019-11-25",
"description": "Describes the main configurable security features of API Gateway, API Manager, and API Portal."
}

## API Gateway inbound SSL configuration

API Gateway supports mutual SSL connections on most inbound interfaces, such as HTTPS interfaces, SMTP services, FTP pollers, and JMS services.  Each of these interfaces allows an administrator to configure the server certificate, the list of certificates that are considered trusted to validate the client certificate, and the cipher suite to use in SSL connections.

The primary interface on API Gateway is the HTTPS interface, where it is possible to configure a number of HTTP/SSL specific options, including SSL server name identification, ciphers, SSL session cache size, SSL protocol version, and ephemeral Diffie Hellman parameters.

By default the HTTPS interface:

* Enables FIPS compliant cipher suites only.
* Explicitly blocks cipher suites that require SSLv3.
* Forces the use of TLSv1.2 only.
* Blocks unauthenticated ciphers.

For more information on configuring HTTPS interfaces, see the
[HTTP and HTTPS interfaces](/docs/apigw_poldev/gw_instances/general_services/#http-and-https-interfaces).

## API Portal inbound SSL configuration

For API Portal, the primary interface is the HTTPS interface on port 443. For TLS, API Portal uses Apache’s `mod_nss` module; therefore, the settings of the interface are configured through the `mod_nss` settings, located here:

```
/etc/apache2/vhosts.d/apiportal.conf
```

By default the the HTTPS interface forces the use of TLSv1.2 only.

For more information on `mod_nss`, including a list of possible options, see <http://iws.cs.uni-magdeburg.de/~elkner/tmp/mod_nss.html>.

Additionally, Joomla! is configured to allow secure connections only. For example, if Apache allows insecure connections due to a change in the default configuration, Joomla! will not allow them.

## API Gateway outbound SSL configuration

As well as receiving messages over SSL-enabled interfaces, API Gateway can also route messages over a mutually authenticated channel to back-end services.  The primary means of doing this is with either the **Connect to URL** filter or the **Connection** filter.  Both of these filters allow you to configure trusted CA certificates, client certificates, SSL/TLS protocols, and client preferences for SSL cipher suites.

Similarly, other filters allow you to make outbound connections over SSL, for example, **File Upload** and **File Download** filters, **SMTP** filter, **Read from JMS** filter, and **Send to JMS** filter.

For more information, see [Routing filters](/docs/apigw_polref/routing_common/).

## API Manager outbound SSL configuration

The **Connect To URL** filters provided in the API Manager custom routing policy templates use TLS version 1.2 with strong FIPS-compliant ciphers by default for SSL/TLS connections. You can modify the SSL/TLS protocols and ciphers in custom routing policies as needed.

For more details, see [Add custom API Manager routing policies](/docs/apimgr_admin/api_mgmt_custom_policies/#add-custom-api-manager-routing-policies).

## API Portal outbound SSL configuration

By default, API Portal performs secure outbound requests to API Manager and API Gateway. To configure the API Portal to verify API Manager’s certificates and host name, see [Connect API Portal to API Manager](/docs/apiportal_install/connect_to_apimgr/).

## Secure Apache Cassandra configuration

Apache Cassandra is required to store data for API Manager. Apache Cassandra can also be used for custom KPS data, for OAuth client application data, or for API keys in API Gateway.

To secure Apache Cassandra you can:

* Reset your default user name and password
* Enable node-to-node SSL traffic encryption
* Enable client-to-node SSL traffic encryption
* Configure the `cqlsh` command for client-to-node SSL encryption

For more details, see [Administer Apache Cassandra](/docs/cass_admin/).

## Configure Java security property in API Gateway

You can configure the Java security property in API Gateway `jvm.xml` file with the `SecurityProperty` node in `JVMSettings`, for example:

```
<SecurityProperty name="jdk.tls.disabledAlgorithms" value="SSLv3"/>
```

## Import certificates and keys to the API Gateway trusted certificate store

Certificates and key pairs can be imported into the API Gateway's trusted certificate store so that they can be used in trusted channel communications, signing and verifying data, encrypting and decrypting data, and so on.

{{< alert title="Note" color="primary" >}}All private keys stored in the certificate store can be encrypted with the Entity Store passphrase.{{< /alert >}}

For more information on the certificate store, see [Manage certificates and keys](/docs/apigtw_security/general_certificates/). Certificates and keys can also be stored in a Hardware Security Module (HSM), for example, Thales nShield Solo or Safenet Luna SA.

API Gateway can also trust certificates in a Java keystore by adding the following line to the `/system/conf/jvm.xml` file:

```
<VMArg name=”-Djavax.net.ssl.trustStore=cacerts/jks"/>
```

## Configure certificates for API Gateway internal SSL channels

After installing API Gateway, it is necessary to run the `managedomain` utility to create a topology (for example, groups and instances) in which the API Gateways will run.  As part of this process, when instances are created, a number of certificates are created to use in mutually authenticated SSL channels between internal components (for example, Admin Node Manager and API Gateway instances). The default signing algorithm used when generating these certificates is SHA256.

Rather than using the default auto-generated certificates (that are signed by a domain-level CA certificate), you can use alternative, custom certificates that have been generated out of band using a PKI, for example.

[Configure Admin Node Manager high availability](/docs/apigtw_admin/admin_node_mngr/) describes how to use the `managedomain` utility to do this.

## API Gateway and Axway PassPort integration

API Gateway can authenticate users against Axway PassPort, which provides a centralized, identity broker for enterprise solutions.

For more information, see the
[API Gateway PassPort Interoperability Guide](/bundle/APIGateway_77_PassPort_InteropGuide_allOS_en_HTML5)
, and the Axway PassPort documentation available on Axway Support at [https://support.axway.com](https://support.axway.com/).

## API Gateway authentication

API Gateway supports a large range of authentication mechanisms and protocols, including HTTP basic and digest authentication, HTML form-based authentication, SSL, session-based authentication, SAML, WS-Security UsernameToken, Kerberos, and many more.  For more information on these filters, see [Authentication filters](/docs/apigw_polref/authn_common/).

API Gateway can also integrate with a number of third party Identity Management products to authenticate users, such as Axway PassPort, Entrust GetAccess, Oracle Entitlements Server, Oracle Access Manager, CA SiteMinder, RADIUS, RSA Access Manager, Sun Access Manager, Tivoli repositories, LDAP, database, Key Property Store (KPS), and the local user store.  For more information on authentication repositories, see [Configure authentication repositories](/docs/apigw_poldev/external_connections/common_user_store/).

## API Portal authentication

API Portal supports only HTML form-based authentication.

## API Gateway authorization

API Gateway also integrates with a number of third-party Identity Management products to provide authorization services for end users.  API Gateway can authorize clients against Axway PassPort, RSA Access Manager, CA SOA Security Manager, CA SiteMinder, Oracle Access Manager, Oracle Entitlements Server, Entrust GetAccess, Tivoli, and Sun Access Manager.

It also supports standard authorization schemes, including LDAP RBAC, SAML, SAML PDP, XACML, and X.509 certificate attributes.  OAuth 2.0 and OpenID Connect are also supported, and are discussed in the following section.

For details on how to configure these authorization services, see [Configure external connections](/docs/apigw_poldev/external_connections/).

## API Gateway OAuth and OpenID Connect

API Gateway can act as an OAuth authorization server by exposing OAuth token management endpoints, which are used by trusted clients to obtain access tokens. Resources exposed by the API Gateway can then use token validation filters to ensure calls can only be performed by authorized clients.

API Gateway can also expose OpenID Connect User Info standard endpoints and supports the generation of OpenID tokens when the `openid` scope is present in the OAuth token request.

By default, OAuth tokens are generated from a random byte array produced by the `java.security.SecureRandom` class. The OAuth token length (in bytes) depends on the type of token and is configurable in all relevant OAuth filters. The default lengths per OAuth token type are as follows:

* OAuth authorization code: 30 bytes
* OAuth access token: 54 bytes
* OAuth refresh tokens: 46 bytes

Depending on your configuration, tokens can be stored in a database, EhCache, or Key Property Store (KPS). The tokens are stored at rest as SHA-256 digests and the token data can be encrypted using the Entity Store passphrase.

See the
[API Gateway OAuth User Guide](/bundle/APIGateway_77_OAuthUserGuide_allOS_en_HTML5/)
for more information on OAuth and OpenID Connect.

## API Gateway message-level encryption and integrity

There are a number of different encryption filters available in API Gateway, for example, **XML Encryption**, **PGP Encrypt and Sign**, and **SMIME Encryption**.  Similarly, API Gateway can sign and verify messages using XML signature, SMIME, or PGP.

For more information, see [Encryption filters](/docs/apigw_polref/encryption_common/), [XML integrity filters](/docs/apigw_polref/integrity_xml/), and [Additional integrity filters](/docs/apigw_polref/integrity_additional/).

## API Gateway certificate validation

While the API Gateway's SSL stack will implicitly check the validity of a certificate in terms of its trusted certificate chain and expiry date, it is also possible to validate client certificates against a CRL, OCSP responder, or XKMS server.  These additional validation mechanisms are especially useful in cases where the certificate is extracted from the message itself, rather than the SSL session.

See [Certificate validation filters](/docs/apigw_polref/cert_validation/) for more information on these certificate validation mechanisms.

## API Gateway Entity Store passphrase

Sensitive data, such as passwords, private keys, and tokens, can be encrypted using the Entity Store passphrase.  The data is encrypted using PBE with SHA1 and 3DES in CBC mode.

For more information on passphrases, see [Configure encryption passphrase](/docs/apigtw_security/general_passphrase/).

### Set Entity Store passphrase for automated startup

If you are using an Entity Store passphrase and are starting API Gateway automatically as a service, you must store the passphrase in certain configuration files.

It is crucial to maintain strict privileges so that only the user that installed API Gateway (or some other specific administrator user) can manually edit the Entity Store passphrase in the files.

## API Gateway Manager and API Gateway password policy

You can configure the following rules for API Gateway administrator user passwords:

* Restrictions on using account user name in passwords.
* Restrictions on password length.
* Restrictions on using passwords that are similar to passwords used previously.
* How long a password is valid.
* The minimum number of uppercase, lowercase, numeric, and special characters a password must contain.

For more details, see [Configure a password policy for admin users](/docs/apigtw_admin/manage_user_access/#configure-a-password-policy-for-admin-users).

## API Manager password policy

In API Manager, you can enable password expiry intervals and configure custom validation rules for API Manager user passwords. For more details, see:

* [Enforce password changes](/docs/apimgr_admin/api_mgmt_admin/#enforce-password-changes)
* [Customize API Manager password validation](/docs/apimgr_admin/api_mgmt_custom/#customize-api-manager-password-validation)

## API Portal password policy

For API Portal user passwords, API Portal enforces any password expiry intervals and custom validation rules that you have configured for user passwords in API Manager.

Since Joomla! administrators are Joomla! specific users, a separate password policy can be configured for them.

You can configure the following rules for Joomla! administrators:

* Restrictions on password length.
* Restrictions on integers that need to be contained within the password.
* Restrictions on special symbols (such as `!@#$`) that need to be contained within the password.
* Restrictions on upper-case symbols.

Additionally, the following settings are available for password reset of Joomla! admin users:

* Maximum times a password can be reset for a specified time period.
* The time period for password resets.

You can set the administrator's password during installation.

API Portal forces a Joomla! administrator password change on first login.

## API Gateway signing the audit trail

An important aspect of maintaining the integrity of the product audit trail is the ability to sign audit records.  API Gateway can sign audit data written to text files, XML files, and databases.

For more details on how to sign transaction logs, see the
[Transaction audit log settings](/docs/apigtw_ref/log_global_settings/#transaction-audit-log-settings).

## API Gateway HSM integration

API Gateway can offload private key storage and operations to a hardware security module (HSM) that has been installed on the host machine. For more information on storing certificates and keys on a HSM, see [Manage certificates and keys](https://axway-open-docs.netlify.com/docs/apigtw_security/general_certificates/).

## API Gateway API firewalling configuration

API Gateway embeds Apache ModSecurity to protect the API traffic that goes through the API Gateway against common HTTP attacks, such as cross-site scripting, SQL injection, command injection, cross-site request forgery, and many others.  Apache ModSecurity is a toolkit for real-time HTTP traffic monitoring, logging, and access control.

For more information on configuring API firewalling, see [Manage API firewalling](/docs/apigtw_security/admin_waf/).

## Policy Studio SSL settings

If Policy Studio is required to make outbound SSL connections, it must trust the server certificate for the SSL session to be established. For example, if Policy Studio must retrieve WSDLs over SSL, test database connections over SSL, and test a connection to an LDAP directory over SSL, it is necessary to trust the server certificate by importing it into Policy Studio's trusted certificate store.

For security reasons, Policy Studio will warn users about the following potential issues with the server's SSL certificate:

* Host name verification issues where the subjectAltName (SAN) or common name (CN) of the certificate does not match the host name in the requested URL.
* Where the certificate is self-signed.
* Where the key size is less than 2048 bits.
* Where the certificate has either expired or is not yet valid.

{{< alert title="Caution" color="warning" >}}To prevent potential man-in-the-middle, spoofing, or eavesdropping attacks, it is strongly recommended that the administrator immediately addresses these issues.{{< /alert >}}

You can access the SSL settings by selecting the **Window > Preferences > SSL Settings** option in Policy Studio.

## Deploy configuration in Policy Studio

When deploying configuration to API Gateway instances in Policy Studio, you must enter the administrator user name and password to authenticate to the Admin Node Manager. This ensures that only an administrator user can push policies to API Gateway instances.

## Set API Gateway administrator password during installation

In line with security best practices, the default behavior of the API Gateway installer is to force the user to set an administrator password during the installation process.

## API Gateway FIPS mode

When running in FIPS mode, all cryptographic operations are performed by the embedded OpenSSL FIPS Object Module and the Entrust Authority Security Toolkit.  Furthermore, the API Gateway runtime will disable cryptographic algorithms that are not FIPS compliant, such as DES and MD5.

The ability to operate in FIPS mode is determined by the type of license used for API Gateway. If a FIPS-enabled license is used, API Gateway can be configured to run in FIPS mode or standard mode.  For more details on running in FIPS mode, see [Run API Gateway in FIPS mode](/docs/apigtw_security/admin_fips/).

It is also possible to operate Policy Studio in FIPS mode. This enables FIPS-certified cryptographic modules and ensures that all cryptographic operations (for example, SSL) are performed by these modules. To enable FIPS mode, select **Window > Preferences > FIPS Mode** in Policy Studio.

## FIPS compliance validation

You can check an API Gateway configuration for FIPS compliance using the FIPS Validation Tool in Policy Studio. It is available from the **Tools > Check Security Constraints > FIPS** menu option.

## NIST SuiteB and SuiteB Top Secret compliance validation

You can check an API Gateway configuration for Suite B and Suite B Top Secret compliance using the SuiteB/SuiteBTS Validation Tools in Policy Studio. These tools are available from the **Tools > Check Security Constraints > SuiteB** and the **Tools > Check Security Constraints > SuiteBTS** menu options.

## API Gateway advisory banner

You can enable an advisory banner in API Gateway Manager, which displays when a user logs in to API Gateway Manager or Policy Studio.

For more information, see [Configure an advisory banner](/docs/apigtw_security/advisory_banner/).

## API keys in API Manager and API Portal

API Manager and API Portal enable you to generate and store API keys for client applications. API keys are stored in a Key Property Store (KPS) and can be managed using the API Manager web UI, API Portal web UI, or the `kpsadmin` tool. Alternatively, you can also generate and store API keys for client applications using the Client Application Registry.

For more information on API key generation and storage, see [Manage client applications](/docs/apimgr_admin/api_mgmt_consume/#manage-client-applications).

For more information on the `kpsadmin` tool, see the [API Gateway Key Property Store User Guide](/bundle/APIGateway_77_KPSUserGuide_allOS_en_HTML5).

## API Gateway audit events

API Gateway exposes an interface in API Gateway Manager that allows you to enable or disable different types of events, including authentication, SSL session establishment, external audit offload, and so on.

For more information, see [Configure events displayed in domain audit log](/docs/apigtw_admin/logging/#configure-events-displayed-in-domain-audit-log).

### External audit offload

API Gateway offers the ability to offload domain and transaction logs to a remote audit server periodically.

For more information, see [Offload audit log files to an external audit server](/docs/apigtw_admin/logging/#offload-audit-log-files-to-an-external-audit-server).

## API Gateway redaction

API Gateway enables you to remove sensitive content from messages monitored in the API Gateway Manager web console and traffic monitoring database. You can redact sensitive content message content types such as HTTP headers, JSON, XML, HTML form, and plain text. For details, see [Hide sensitive data](/docs/apigtw_security/admin_redactors/).
