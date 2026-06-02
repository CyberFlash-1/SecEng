# SNYK
Analyzes the open-source libraries and packages your code depends on. Finds known CVEs in third-party code, not your own logic.


<p>Software Composition Analysis (SCA) is the practice of automatically inventorying all open-source components in an application and checking each one against databases of known security vulnerabilities (CVEs). Modern applications typically include hundreds of transitive dependencies — libraries that your direct dependencies themselves depend on — making manual tracking impossible.</p>

## What SCA finds
•	Known CVEs in direct dependencies (packages you explicitly imported)
•	Known CVEs in transitive dependencies (packages your packages imported)
•	License violations — copyleft licenses (GPL) that may conflict with commercial use
•	Outdated packages that are past their supported lifecycle
•	Malicious packages — typosquatting or dependency confusion attacks

## Lab Setup

Step 1 — Install the Snyk extension in Docker Desktop
1.	Open Docker Desktop
2.	Navigate to Extensions in the left sidebar
3.	Click Manage → Browse
4.	Search for Snyk Security
5.	Click Install
6.	Once installed, the Snyk extension appears in the left sidebar under Extensions

Authentication
Snyk requires a free account at snyk.io. On first launch, the extension will prompt you to log in or create an account. The free tier allows scanning of public repositories and local images with no limit on scan count for individual use.

Step 2 — Run a container image scan
7.	In Docker Desktop, navigate to Images in the left sidebar
8.	Find the image you want to scan (in this lab, the SonarQube image used in Lab 1 is a good candidate)
9.	Click on the image name to open the image details
10.	Click the Snyk button or navigate to the Snyk extension and select the image
11.	Click Scan — the extension pulls the image manifest and submits it to Snyk for analysis
12.	Results appear within 30–60 seconds

![snyk docker](https://github.com/khadijahW/Flash028/blob/6e0813e51c1e8778958461fc67dad14687ed9f56/SonarSnyk/imasnyk/snyk%20docker)




![snyk critical](https://github.com/khadijahW/Flash028/blob/25d94a293353399e3aadc03c9305204515ae1d0d/SonarSnyk/snyk%20critical)

## Remediation Workflow
Finding vulnerabilities is only the first step. A mature SCA workflow includes a structured remediation process:

## Step	Action
1. Inventory	Generate a Software Bill of Materials (SBOM) listing all dependencies and versions
2. Triage	Filter findings by severity, exploit maturity, and reachability. Discard theoretical findings.
3. Prioritize	Fix Critical and High findings with active exploits first. Group by package to minimize upgrade cycles.
4. Remediate	Update the dependency version in package.json, requirements.txt, pom.xml, or Dockerfile
5. Verify	Re-scan after patching to confirm the CVE is resolved and no regression was introduced
6. Monitor	Configure Snyk to re-scan automatically on a schedule so newly disclosed CVEs are caught
