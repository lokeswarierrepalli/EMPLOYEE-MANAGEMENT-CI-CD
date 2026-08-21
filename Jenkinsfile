pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/lokeswarierrepalli/EMPLOYEE-MANAGEMENT-CI-CD.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t employee-management:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f employee-management-app || true'
                sh '''
                    docker run -d \
                    --name employee-management-app \
                    --add-host=host.docker.internal:host-gateway \
                    -p 8081:8080 \
                    -e SPRING_DATASOURCE_URL=jdbc:postgresql://host.docker.internal:5432/employee_db \
                    -e SPRING_DATASOURCE_USERNAME=employee_user \
                    -e SPRING_DATASOURCE_PASSWORD=employee123 \
                    employee-management:latest
                '''
            }
        }
    }
}
