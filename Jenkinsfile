pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/prapuldev/spring-boot-app.git'
            }
        }

        stage('Build Backend') {
            steps {
                sh '''
                export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
                export PATH=$JAVA_HOME/bin:$PATH

                cd spring-boot-server
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                docker stop spring_backend || true
                docker rm spring_backend || true

                docker run -d \
                --name spring_backend \
                --network my-angular-springboot-net \
                -p 8085:8085 \
                -v $WORKSPACE/spring-boot-server/target:/app \
                eclipse-temurin:17-jdk \
                java -jar /app/spring-boot-jpa-postgresql-0.0.1-SNAPSHOT.jar
                '''
            }
        }

        stage('Build Angular') {
            steps {
                sh '''
                cd angular-16-client
                npm install
                npm run build
                '''
            }
        }

        stage('Deploy Angular') {
            steps {
                sh '''
                docker stop angular_ui || true
                docker rm angular_ui || true

                docker run -d \
                --name angular_ui \
                --network my-angular-springboot-net \
                -p 80:80 \
                -v $WORKSPACE/angular-16-client/dist/angular-16-crud:/usr/share/nginx/html \
                -v /root/prapul/projects/nginx.conf:/etc/nginx/conf.d/default.conf:ro \
                nginx:alpine
                '''
            }
        }
    }
}
