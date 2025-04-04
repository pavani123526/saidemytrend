pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }

    tools {
        maven 'M2_HOME' // Use the Maven name configured in Jenkins
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "📦 Checking out source code..."
                git url: 'https://github.com/pavani123526/saidemytrend.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "🔧 Build started..."
                sh 'mvn clean install -DskipTests=true'
                echo "✅ Build completed!"
            }
        }

        stage('Unit Tests') {
            steps {
                echo "🧪 Running Unit Tests..."
                sh 'mvn test'
                junit 'target/surefire-reports/*.xml'
                echo "✅ Tests completed!"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "🔍 Running SonarQube Analysis..."
                withSonarQubeEnv('saidemy-sonarqube-server') {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=saidemy01-keyys_saidemytrend \
                            -Dsonar.organization=saidemy01-keyys
                    '''
                }
            }
        }
    }
}

