def yc = '/var/lib/jenkins/yc'
def key = '/var/lib/jenkins/key.json'

pipeline {
    agent any

    environment {
        YC_FOLDER_ID = 'b1g967dd97uj20idd9uc'
        YC_CLOUD_ID = 'b1g2u0o136euitr7923q'  
        SA_KEY = credentials('iam-key')
    }

    stages {
        stage('Setup profile serv acc') {
            steps {
                script {
                    sh """
                        ${yc} config profile create sa-profile || true
                        ${yc} config set folder-id ${env.YC_FOLDER_ID}
                        ${yc} config set cloud-id ${env.YC_CLOUD_ID}
                        ${yc} config set service-account-key ${key}
                        ${yc} config profile activate sa-profile
                    """
                }
            }
        }

        stage('Test YC CLI') {
            steps {
                script {
                    sh "${yc} config list" // Проверка текущей конфигурации
                }
            }
        }
    }
}
