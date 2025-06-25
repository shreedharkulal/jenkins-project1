pipeline {
    agent any

    environment {
        SONARQUBE = credentials('sonarqube') // secret token from Jenkins credentials
        DOCKER_CREDS = 'docker-cred'
        DOCKER_IMAGE = 'shreedhar13/ultimate-cicd'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shreedharkulal/jenkins-project1.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=jenkins-project1 \
                          -Dsonar.host.url=http://13.126.111.204:9000 \
                          -Dsonar.login=$SONARQUBE
                    '''
                }
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDS}") {
                        def image = docker.build("${DOCKER_IMAGE}:latest")
                        image.push()
                    }
                }
            }
        }
    }
}
