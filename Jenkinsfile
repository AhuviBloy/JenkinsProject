pipeline {
    agent {label 'verisoft-2'}

    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/AhuviBloy/JekinsProject.git', description: 'GitHub repository URL')
        string(name: 'NAME_BRANCH', defaultValue: 'main', description: 'Branch name to build from')
    }

    environment {
        MAIN_BRANCH = 'main'
    }
    
    triggers {
        cron('30 5 * * 1') // כל יום שני 05:30 בבוקר
        cron('0 14 * * *') // כל יום 14:00 בצהריים
    }

    stages {
        stage('Clone code') {
            steps {
                script {
                    if (params.BRANCH_NAME == env.MAIN_BRANCH) {
                        checkout scm
                    } else {
                        git branch: "${params.BRANCH_NAME}", url: "${params.REPO_URL}"
                    }
                }
            }
        }

        stage('Compilation') {
            steps {
                echo 'Starting compilation stage'
                timeout(time: 5, unit: 'MINUTES') {
                    sh returnStatus:true,script:'mvn compile'
                }
                echo 'Compilation stage completed successfully'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Starting test stage'
                timeout(time: 5, unit: 'MINUTES') {
                    sh returnStatus:true,script:'mvn test'
                }
                echo 'Test stage completed successfully'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }

 triggers {
     cron('30 5 * * 1\n0 14 * * *')
 }

}
