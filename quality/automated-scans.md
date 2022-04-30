# Automated scans

A best practice is to use various types of scans to automate often boring, sometimes hard, sometimes also mandated requirements e.g. for compliance and security aspects.

**🎯 Example 1**: We use [Trivy](https://github.com/aquasecurity/trivy) to check for vulnerabilities in packages (among other places).

**🎯 Example 2**: Then, we use [Checkov](https://www.checkov.io) to scan for misconfigurations, and also create an infrastructure-as-code SBOM (["software bill-of-materials"](https://en.wikipedia.org/wiki/Software_bill_of_materials)).

**🎯 Example 3**: Finally, we use [gitleaks](https://github.com/zricethezav/gitleaks) for detecting hard-coded secrets in our repository.
