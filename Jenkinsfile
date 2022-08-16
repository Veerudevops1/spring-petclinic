pipeline {
    agent { label 'jdk11' }
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
    }
    triggers {
        cron('0 * * * *')
    }
    parameters {
        choice(name: 'GOAL', choices: ['compile', 'package', 'clean package'])
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/Veerudevops1/spring-petclinic.git', 
                branch: 'main'
            }

        }
        stage('Build the Code and sonarqube-analysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST') {
                    sh script: "mvn ${params.GOAL} sonar:sonar"
                }

            }
        }
        stage('reporting') {
            steps {
                junit testResults: '**/*.xml'
            }
        }

    }

}