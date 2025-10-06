pipeline {
  agent any

  parameters {
    choice(name: 'IMAGE', choices: ['ubuntu-2404-lts-oslogin', 'debian-12'])
    string(name: 'CPU', defaultValue: '2')
    string(name: 'MEM', defaultValue: '4')
    string(name: 'DISK_SIZE', defaultValue: '20')
    string(name: 'VM_NAME', , defaultValue: 'test-vm')
  }

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
            yc config profile create sa-profile || true
            yc config set folder-id ${env.YC_FOLDER_ID}
            yc config set cloud-id ${env.YC_CLOUD_ID}
            yc config set service-account-key ${env.SA_KEY}
            yc config profile activate sa-profile
         """
        }
      }
    }

   stage('Create VM') {
     steps {
       script {
         def vmName = params.VM_NAME
         def image = params.IMAGE
         def cpu = params.CPU
         def mem = params.MEM
         def diskSize = params.DISK_SIZE

         sh """
            yc compute instance create \
            --name $vmName \
            --zone ru-central1-b \
            --network-interface subnet-name=default-ru-central1-b,nat-ip-version=ipv4 \
            --create-boot-disk image-folder-id=stadard-images,size=$diskSize,image-family=$image \
            --memory $mem \
            --cores $cpu \
            --metadata-from-file user-data=metadata.yaml
         """
         timeout(time: 5, unit: 'MINUTES') {
           waitUntil {
             def status = sh(script: "yc compute instance get --name $vmName --format json | jq -r '.status'", returnStdout: true).trim()
             echo "VM status ${status}"
           }
          }
        }
      }
    }
  }
}
