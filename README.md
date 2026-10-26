# CS305Project2
# CS 305 – Software Security: Artemis Financial

## Client and requirements
Artemis Financial is a consulting company that builds individualized financial plans for its customers, covering savings, retirement, investments, and insurance. They wanted to modernize their web-based software, and because it handles sensitive financial data, they asked me to secure it. Specifically, they needed a way to verify data integrity with a checksum, secure communication between client and server over HTTPS, and a check that their code and dependencies weren't introducing known vulnerabilities.

## What I did well and why secure coding matters
I worked through the application in layers instead of jumping straight to fixes. I added a SHA-256 checksum endpoint, generated a self-signed certificate, moved the server to HTTPS, and then ran a static dependency scan. I also verified my own work. I computed the SHA-256 of my data string separately and confirmed it matched the server's output, and I checked that the exported .cer file's fingerprint matched the certificate the browser received. Secure coding matters because a single weakness in a financial application can expose customer data, cause regulatory penalties, and destroy client trust. Security adds value by protecting the company's reputation and reducing the cost of breaches, which are far more expensive to fix after release than during development.

## Challenging or helpful parts
The most challenging part was getting the tooling to work. The server first failed to start because Spring couldn't find the keystore until I changed the path to `classpath:keystore.jks`. The OWASP dependency-check plugin also failed until I requested and activated an NVD API key. The most helpful part was the dependency-check report itself. It showed 254 known vulnerabilities across 14 dependencies, all inherited from the outdated Spring Boot 2.2.4 framework rather than from my code. That showed me how much risk comes from third-party libraries.

## Increasing layers of security
I added security in layers: cryptographic hashing (SHA-256) for data integrity, TLS/HTTPS with an RSA-2048 certificate for encrypted communication, and static dependency analysis to catch known CVEs. In the future I would keep using OWASP Dependency-Check alongside the OWASP Top 10, the vulnerability assessment process flow, and CVSS severity scores to prioritize fixes. Critical, exploitable issues come first, and the usual mitigation for inherited issues is upgrading the framework to a supported version.

## Ensuring functionality and security
I confirmed the application was functional by running it and checking the console for "Tomcat started on port(s): 8443 (https)". I loaded `https://localhost:8443/hash` in the browser and ran the Maven unit test, which passed with 0 failures and 0 errors. To check whether I had introduced new vulnerabilities, I reran the dependency-check after refactoring. My changes only used built-in JDK classes (`MessageDigest`, `StandardCharsets`) and added no new dependencies, so the refactoring introduced no new vulnerabilities.

## Resources, tools, and practices
Java Keytool for certificates, Java's `MessageDigest` for hashing, Spring Boot's SSL configuration, Maven, and the OWASP Dependency-Check plugin with the NVD database. Useful practices: relying on vetted cryptography libraries instead of writing my own, keeping secrets like API keys out of source code, reading stack traces from the bottom `Caused by` line to find the real error, and independently verifying results.

## What I would show employers
I would show the refactored Spring Boot application and the Practices for Secure Software Report. Together they demonstrate that I can implement hashing and HTTPS, generate and manage certificates, run and interpret a dependency vulnerability scan, and clearly document security findings and recommendations for a client.
