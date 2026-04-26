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
                JAR_FILE=$(ls target/*.jar)
        
                curl -v -u admin:da23d000-a7e7-4716-bb99-59d1590631c7 \
                -F "maven2.groupId=com.example" \
                -F "maven2.artifactId=demo" \
                -F "maven2.version=1.0.0" \
                -F "maven2.asset1=@${JAR_FILE}" \
                -F "maven2.asset1.extension=jar" \
                -F "maven2.generate-pom=true" \
                http://nexus-service:8081/service/rest/v1/components?repository=maven-releases-final
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
