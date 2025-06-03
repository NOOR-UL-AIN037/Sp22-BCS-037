pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/NOOR-UL-AIN037/Sp22-BCS-037.git', branch: 'main'
            }
        }

        stage('Scan Secrets with Gitleaks') {
            steps {
                sh 'gitleaks detect --source . --no-banner --report-path=gitleaks-report.json || true'
                script {
                    def report = readFile('gitleaks-report.json')
                    if (report.contains('"Rule":')) {
                        emailext(
                            to: 'your-email@gmail.com',
                            subject: '🚨 Jenkins Alert: Secrets found!',
                            body: 'Gitleaks detected secrets in the latest push. Deployment aborted.'
                        )
                        error("❌ Secrets found! Deployment blocked.")
                    } else {
                        echo "✅ No secrets found. Proceeding."
                    }
                }
            }
        }

        stage('Deploy HTML') {
            steps {
                sh 'copy ProjectForm\\index.html C:\\inetpub\\wwwroot\\index.html'
            }
        }
    }
}
