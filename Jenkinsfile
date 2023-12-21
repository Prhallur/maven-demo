pipeline {
    agent any
    environment {
        SONAR_URL = 'http://34.125.191.127:9000 '
        SONAR_TOKEN = credentials('sonar-jenkins')
        SONAR_PROJECT_KEY = 'demo'
        }
    stages {
        stage('gitclone') {
            steps {
                git branch: 'main', url: 'https://github.com/Prhallur/maven-demo.git'
            }
        }
        
        stage('SonarQube Scan') {
            steps {
                script {
                    withSonarQubeEnv('Sonar-server') {
                        // Run SonarQube scanner with parameters
                        sh "mvn sonar:sonar \
                            -Dsonar.host.url=${SONAR_URL} \
                            -Dsonar.login=${SONAR_TOKEN} \
                            -Dsonar.projectKey=${SONAR_PROJECT_KEY}"
                    }
                }
            }
        }
        
        stage ('build') {
            steps { 
                sh 'mvn clean package'
            }
        }
        
        stage ('deploytotomcat') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat-jenkins', path: '', url: 'http://34.125.239.64:8080')], contextPath: '/sampleabc', war: '**/*.war'
            }
        }
    }
}
