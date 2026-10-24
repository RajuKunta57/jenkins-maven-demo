
pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-amazon-corretto.x86_64'
        PATH = "/usr/lib/jvm/java-21-amazon-corretto.x86_64/bin:${env.PATH}"
        SONAR_ORGANIZATION = 'rajukunta57'
        SONAR_PROJECT_KEY = 'RajuKunta57_jenkins-cicd-demo'
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from GitHub + Jenkins!'
            }
        }

        stage('Show Files') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Check Java and Maven') {
            steps {
                sh '''
                    java -version
                    javac -version
                    mvn -version
                '''
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Cloud Analysis') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'sonarcloud-token',
                        variable: 'SONAR_TOKEN'
                    )
                ]) {
                    sh '''
                        mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                          -Dsonar.organization=$SONAR_ORGANIZATION \
                          -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build, tests, SonarQube analysis, and artifact archiving completed.'
        }

        failure {
            echo 'Pipeline failed. Check the first error in Console Output.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}
