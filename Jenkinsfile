pipeline {
  angens any

  parameters {
    choice(name: 'IMAGE', choices: ['ubuntu-2404-lts-oslogin', 'debian-12'])
    string(name: 'CPU', dafaultValue: '2')
    string(name: 'MEM', dafaultValue: '4')
    string(name: 'DISK_SIZE', dafaultValue: '20')
  }

  environment {
    YC_FOLDER_ID = 'b1g967dd97uj20idd9uc'
    YC_CLOUD_ID = 'b1g2u0o136euitr7923q'
    SA_KEY = credentials('iam-key')
  }

  stages {
    stage('Setup profile serv acc') {
     steps{
       script {
         sh """
            yc config profile create sa-profile || true
            yc config set folder-id ${env.YC_FOLDER_ID}
            yc config set cloud-id ${env.YC_CLOUD_ID}
            yc config set service-account-key ${env.SA_KEY}
         """
       }
     } 
    }    
  }
}
