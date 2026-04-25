pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh '''
                JAR_FILE=$(ls target/*.jar | head -n 1)

                curl -v -u admin:CSIT1234567890 \
                  -F "maven2.groupId=com.example" \
                  -F "maven2.artifactId=springboot-jenkins-midterm" \
                  -F "maven2.version=1.0.0" \
                  -F "maven2.generate-pom=true" \
                  -F "maven2.asset1=@${JAR_FILE}" \
                  -F "maven2.asset1.extension=jar" \
                  "http://nexus-service:8081/repository/maven-releases-final/"
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build successful and uploaded to Nexus!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
