pipeline {
    agent any  // Runs on any available agent

    environment {
        PATH = "/opt/maven/bin:$PATH"  // Ensure Maven is available
    }

    stages {
        stage("Build") {
            steps {
                script {
                    echo "----------- Build started ----------"
                    def status = sh(script: 'mvn clean deploy -Dmaven.test.skip=true', returnStatus: true)
                    if (status != 0) {
                        error "Build failed with exit code: ${status}"
                    }
                    echo "----------- Build completed ----------"
                }
            }
        }

        stage("Test") {
            steps {
                script {
                    echo "----------- Unit test started ----------"
                    def testStatus = sh(script: 'mvn surefire-report:report', returnStatus: true)
                    if (testStatus != 0) {
                        error "Tests failed with exit code: ${testStatus}"
                    }
                    echo "----------- Unit test completed ----------"
                }
            }
        }

        stage("SonarQube Analysis") {
            environment {
                scannerHome = tool 'saidemy-sonar-scanner'  // Set the SonarQube scanner tool
            }
            steps {
                script {
                    echo "----------- SonarQube analysis started ----------"
                    withSonarQubeEnv('saidemy-sonarqube-server') {
                        def sonarStatus = sh(script: "${scannerHome}/bin/sonar-scanner", returnStatus: true)
                        if (sonarStatus != 0) {
                            error "SonarQube analysis failed with exit code: ${sonarStatus}"
                        }
                    }
                    echo "----------- SonarQube analysis completed ----------"
                }
            }
        }
    }

    
