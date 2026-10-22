---
paths:
  - "**/*.{ps1,psm1,psd1}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: PowerShell

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- DSC class "Test" methods should return boolean values **(critical)**
- Cmdlets should be invoked with all mandatory parameters
- Parameter sets should have at most one parameter accepting pipeline input by value
- Reserved common parameters should not be redefined in advanced functions
- DSC class "Set" methods should return void
- "HelpMessage" parameter attributes should not be null or empty
- DSC resource functions should have identical parameters
- DSC resources should implement all required functions
- Parameters should have only one type specifier
- Backticks used for line continuation should be followed by a newline

### Vulnerabilities
- Credentials should not be sent over unencrypted connections **(critical)**

### Code smells
- Hash algorithms MD5 and SHA-1 should not be used
- "$null" should be placed on the left side of comparison operators
- Credential parameters should use the PSCredential type
- "Invoke-Expression" should not be used
