pipeline { 
    // Defines the start of the Jenkins declarative pipeline
    agent any 
    // Pipeline can run on any available Jenkins node

    environment {
        // Adds Maven binary path to system PATH
        PATH = "/opt/maven/bin:${env.PATH}"
    }

    stages {
        stage('Build') {
            // This stage compiles the Java source code
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            // This stage runs unit test cases
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy WAR') {
            // This stage packages the application into a WAR file
            // WAR file will be generated inside target/ directory
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube Analysis') {
            // SonarQube static code analysis stage
            environment {
                // Gets Sonar Scanner tool configured in Jenkins
                scannerHome = tool 'sonar-qube-scanner-tool'
            }

            steps {
                // Connects Jenkins with SonarQube server
                withSonarQubeEnv('sonar-qube-scanner-server') {
                    // Executes SonarQube scanner
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}

