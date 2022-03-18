pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vitokurre/AlternativJenkinsVerktygMarcusDavidssonMT21.git'
            }
        }
        stage('junit build') {
            steps {
                sh "mvn compile"
            }
        }
        stage('JUnit test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Newman') {
            steps {
                sh 'newman run LabbMarcusCollection.postman_collection.json --environment LabbMarcusEnviroments.postman_environment.json --reporters junit –reporter-junit-export "newman/newman_report.xml"'
            }
            post {
                always {
                        junit 'newman/newman-*.xml'
                    }
            }
        }
        stage('Robot Framework') {
            steps {
                sh 'robot --variable BROWSER:headlesschrome -d Results Tests'
            }
            post {
                always {
                    script {
                          step(
                                [
                                  $class              : 'RobotPublisher',
                                  outputPath          : 'results',
                                  outputFileName      : '**/output.xml',
                                  reportFileName      : '**/report.html',
                                  logFileName         : '**/log.html',
                                  disableArchiveOutput: false,
                                  passThreshold       : 50,
                                  unstableThreshold   : 40,
                                  otherFiles          : "**/*.png,**/*.jpg",
                                ]
                          )
                    }
                }
            }
        }
    }
    
}
