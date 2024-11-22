def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]
pipeline {
    agent any
    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }
    
    environment {
        registryCredential = 'ecr:ca-central-1:awscreds'
        appRegistry = "211125311039.dkr.ecr.ca-central-1.amazonaws.com/devops_project"
        groupRegistry = "https://211125311039.dkr.ecr.ca-central-1.amazonaws.com"
        cluster = "devops_project"
        service= "my_group_project_svc"
    }

    stages {
        stage('Fetch code') {
            steps {
                git branch: 'docker', url: 'https://github.com/yash91066/docker_jenkins_auto_trigger.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn install -DskipTests'
            }
        }

        stage('UNIT TEST') {
            steps {
                sh 'mvn test'
            }
        }

    //     stage('Checkstyle Analysis') {
    //         steps {
    //             sh 'mvn checkstyle:checkstyle'
    //         }
    //     }

    //     stage("Sonar Code Analysis") {
    //         environment {
    //             scannerHome = tool 'sonar6.2'
    //         }
    //         steps {
    //             withSonarQubeEnv('sonarserver') {
    //                 sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=group_project \
    //                   -Dsonar.projectName=group_project \
    //                   -Dsonar.projectVersion=1.0 \
    //                   -Dsonar.sources=src/ \
    //                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
    //                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
    //                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
    //                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
    //             }
    //         }
    //     }

    //     // stage("Quality Gate") {
    //     //     steps {
    //     //         timeout(time: 1, unit: 'HOURS') {
    //     //             waitForQualityGate abortPipeline: true
    //     //         }
    //     //     }
    //     // }

    //     stage('Build App Image') {
    //         steps {
    //             script {
    //                 dockerImage = docker.build("${appRegistry}:$BUILD_NUMBER", "./Docker-files/app/multistage/")
    //             }
    //         }
    //     }

    //     stage('Upload App Image') {
    //         steps {
    //             script {
    //                 docker.withRegistry(groupRegistry, registryCredential) {
    //                     dockerImage.push("$BUILD_NUMBER")
    //                     dockerImage.push('latest')
    //                 }
    //             }
    //         }
    //     }

    //     stage("Remove Container Images") {
    //         steps {
    //             sh 'docker rmi -f $(docker images -a -q) || true'
    //         }
    //     }
        
    //     stage('Deploy to ecs') {
    //       steps {
    //         withAWS(credentials: 'awscreds', region: 'ca-central-1') {
    //         sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
    //           }
    //       }
    //     }
    // }
    
    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#myprojectgroup',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
}
