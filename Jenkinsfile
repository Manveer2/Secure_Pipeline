pipeline {
    agent {
        dockerContainer {
            image 'my-secure-sdlc-security-tools:latest' 
            args '-u root' 
        }
    }

    environment {
        FLASK_IMAGE = 'my-secure-sdlc-flask-app'
        WORKDIR = "${env.WORKSPACE}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Manveer2/Secure_Pipeline.git'
            }
        }

        stage('Generate SBOM with Syft') {
            steps {
                sh "syft ${FLASK_IMAGE} -o json > syft-report.json"
            }
        }

        stage('Vulnerability Scan with Trivy') {
            steps {
                sh "trivy image --severity HIGH,CRITICAL --format json ${FLASK_IMAGE} > trivy-report.json"
            }
        }

        stage('Secrets Scan with Gitleaks') {
            steps {
                sh "gitleaks detect --source=. --report-path=gitleaks-report.json"
            }
        }

        stage('IaC Scan with Checkov') {
            steps {
                sh "checkov -d . --output json > checkov-report.json"
            }
        }

        stage('SAST with Semgrep') {
            steps {
                sh "semgrep scan --config auto --json > semgrep-report.json"
            }
        }

        stage('DAST with Nikto') {
            steps {
                sh "sleep 10 && perl /opt/nikto/program/nikto.pl -host http://host.docker.internal:5000 -output nikto-report.txt || true"
            }
        }
    }
}
