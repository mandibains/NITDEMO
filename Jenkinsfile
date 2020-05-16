pipeline {
    agent any
    parameters {
        string(name: 'project_name', defaultValue: 'Packer Pipeline', description: 'Jenkins Pipeline for terraform?')
    }
    environment {
        terraform_version = '0.12.20'
        packer_version = '1.4.3'
       access_key = ''
       secret_key = ''
    }

  stages {
          stage('Install Terraform') {
              steps {
                    
                 // sh "cd /var/jenkins_home/workspace/"
                    //#sh "sudo rm -rf SamplePipeline"
                   //sh "sudo rm /usr/bin/terraform"
                    //sh "sudo rm /usr/bin/packer"
                    sh "sudo apt-get install wget zip -y"
                    sh "cd /tmp"
                    sh "sudo curl -o bin_terraform.zip https://releases.hashicorp.com/terraform/'$terraform_version'/terraform_'$terraform_version'_linux_amd64.zip"
                    sh "ls -l; pwd;sudo unzip -o bin_terraform.zip"
                    sh "sudo mv terraform /usr/bin"
                    sh "sudo rm -rf bin_terraform.zip"
                    sh "sudo terraform version"
              }
          }
          stage('Install Packer') {
              steps {
                    sh "sudo apt-get install wget zip -y"
                    sh "cd /tmp"
                    sh "sudo curl -o bin_packer.zip https://releases.hashicorp.com/packer/$packer_version/packer_'$packer_version'_linux_amd64.zip"
                    sh "sudo unzip -o bin_packer.zip"
                    sh "sudo mv packer /usr/bin"
                    sh "sudo rm -rf bin_packer.zip"
                    sh "sudo packer version"
              }
          }
           stage('code checkout') {
               steps {
                    git branch: 'master', url: 'https://github.com/mandibains/NITDEMO.git'
                    }
          }
          stage('Build AMI') {
                steps {
                    dir('./packer'){
                     sh 'ls -la; pwd; packer build template.json'
                    }
                }
          }
          stage('Deploy??') {
                steps {
                    script {
                       timeout(time: 2, unit: 'MINUTES') {
                          input(id: "Deploy Gate", message: "Want to Deploy ${params.project_name}?", ok: 'Deploy??')
                       }
                    }
                }
          }
         stage('Terraform Deploy'){
             steps {
                 dir('./terraform'){

                 sh  """
                 terraform init; terraform plan; terraform apply -auto-approve
                     """

                 }

             }
         }
    }
}
