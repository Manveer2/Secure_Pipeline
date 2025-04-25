This project is an example of a secure, automated CI/CD for a simple web application. 
It uses open source tools and STIG-compliant containers from Iron Bank in its deployment. 

Project Overview:

This project relies on Docker Desktop as its Docker engine. Docker Desktop is configured to use Ubuntu as its WSL2 distro, meaning that the Ubuntu command
line must be used to deploy "docker-compose.yml" and create all the necessary containers and images. It consists of a basic Flask app which is accessible
on a browser at http://localhost:5000. Its metrics are scraped by a Prometheus image accessible at http://localhost:9090 which are then able to be
visualized by a Grafana image on http://localhost:3000. 

If changes are made to either the images or the application itself, then these can be pushed to the repository at https://github.com/Manveer2/Secure_Pipeline.git 
and then implemented by either rebuilding the images (docker-compose) or running the CI/CD pipeline on Jenkins at http://localhost:8080, depending on the type of 
changes made. In Jenkins, a pipeline project is available which runs inside the Docker container and pulls the GitHub repo before scanning the repo and Flask app 
for vulnerabilities. Each of the tools used will be discussed below; all are open source and available either on GitHub or able to be pulled using pip3.

STIG Benefits:
This project utilizes Iron Bank containers for Jenkins and Flask. They are STIG (Security Technical Implementation Guides) compliant, meaning that they
are designed to be as secure as possible. They are reliably secure since STIG compliant containers are used by the US Department of Defense to protect
their hardware and software. The benefits of using Iron Bank containers for Flask and Jenkins are that the flask app and the CI/CD pipeline can not be
maliciously accessed or sabotaged by an attacker. They are pre-hardened and contain little/no vulnerabilities and can be relied upon to be secure. This
means that the application and the pipeline are STIG compliant and adhere to security standards.

Syft (SBOM):
This tool is used to generate a software bill of materials (SBOM) which lists all packages and dependencies in the Iron Bank container. It is useful for
getting an understanding of which software is present and being used. This can help identify potential vulnerabilities while also providing a better 
understanding of what kinds of behavior to expect.

Trivy (Container Security):
Trivy is used to scan the Docker image for known vulnerabilities in a fast and efficient manner. It can identify any packages in the SDLC which are outdated
or insecure by matching them against vulnerability databases. 

Gitleaks (Secrets Scanning):
This is used to prevent credential leaks, which can of course lead to unauthorized access to sensitive files and information. It does this by detecting
hardcoded secrets such as API keys, passwords, and tokens in the project's source code.

Checkov (IaC Security):
Checkov scans IaC (Infrastructure as Code) files, in this case the YAML files, for any misconfigurations which can be exploited. This ensures that the 
infrastructure is secure and the attack surface is minimized.

Semgrep (SAST):
This tool scans the project's source code in a similar way as Gitleaks, but it instead searches for vulnerabilties instead of credentials. Basic logic flaws,
missing validations, or insecure function use can be detected and reported by Semgrep. It relates to SAST (Static Application Security Testing) be scanning
source code, which is in a static state.

Nikto (DAST):
Nikto is an example of DAST (Dynamic Application Security Testing) and scans the Flask app which is running on http://localhost:5000. It looks for common
vulnerabilities and exploits at runtime, making it a useful addition to Semgrep which scans the static source code.

