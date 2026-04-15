# Security

Sources:
* OWASP Top 10 (https://owasp.org/Top10/2025/)
* https://www.ncsc.gov.uk/cyberessentials/overview
* https://ce-knowledge-hub.iasme.co.uk/space/CEKH/2650538068/Securing+your+cloud+services
* MITRE ATT&CK : https://attack.mitre.org/

It is essential that we have the following things:

1. Accurate asset inventories

This means any AWS resources, including S3 buckets, and IAM roles, need to be listed out explicitly. It should be possible to create the whole project from scratch in a new AWS account with no existing IAM roles, only by changing the AWS account number.

2. Robust access controls

All IAM permissions should be least-privilege and scoped to the service that requires them. Detailed descriptions of all the permissions in IAM roles should be made available to the developer. They should also explain how aspects of IAM may interact, e.g. through assuming a role.

3. Secure configuration

Set up computers securely to minimise ways that a cyber-criminal can find a way in. 

4. Comprehensive logging

There should be logs of when different resources are accessed, including calls to external services.

5. Security update management

We should use automated scanning services and tools like dependabot. These should then feed PRs, which run tests and are (maybe?) automatically merged as long as all tests pass.