---
paths:
  - "**/*.{xml,xsd,xsl,xslt,wsdl,pom}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: XML

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- XML files containing a prolog header should start with "<?xml" characters
- Dependencies should not have "system" scope
- Hibernate should not update database schemas
- "SingleConnectionFactory" instances should be set to "reconnectOnException"
- "DefaultMessageListenerContainer" instances should not drop messages during restarts
- Applications should have an APIKit error handler
- HTTP Requestor should have a configurable response timeout

### Vulnerabilities
- Default EJB interceptors should be declared in "ejb-jar.xml" **(critical)**
- Struts validation forms should have unique names **(critical)**
- Struts filters should not miss their corresponding filter-map
- Android broadcast receivers should not be registered without a permission
- Passwords should not be stored in plaintext or with a fast hashing algorithm
- Mule Credentials Vault should not use a hardcoded encryption key
- Trust Store should not have the insecure attribute
- Android application backup should be disabled or restricted
- Content providers should use separate read and write permissions
- Components should be explicitly exported
- TLS store configurations should use configurable paths
- Encryption keys should not be logged
- Mule Secure Properties should use AES-CBC algorithm
