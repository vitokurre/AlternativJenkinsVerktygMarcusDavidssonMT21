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
        stage('Cobertura coverage') {
            steps {
                sh "mvn -B cobertura:cobertura"
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
    post {
         always {
            cobertura autoUpdateHealth: false, autoUpdateStability: false,
            coberturaReportFile: '**/target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0',
            enableNewApi: true, failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0',
            maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII',
            zoomCoverageChart: false
            //emailext attachLog: true, attachmentsPattern: '**/TEST*xml',
            //body: 'Bod-DAy!', recipientProviders: [culprits()], subject:
            //'$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!'
         }
    }
    
}
