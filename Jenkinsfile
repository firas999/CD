pipeline{
    agent any
    
    stages{
       stage('GetCode'){
            steps{
                script{
                    checkout   ([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/FatmaDaas/CD-Project.git']]])
	sh "whoami"               
 }
               
            }
         }      
 stage('build'){
steps{
script{
  sh "npm install"

sh "ansible-playbook ansible/build.yml  -i ansible/inventory/host.yml -e 'ansible_become_password=ansible' -vvv   " 

}
}
}      
       
stage ('Docker'){
steps{
script{
sh "ansible-playbook ansible/docker.yml  -i ansible/inventory/host.yml -e 'ansible_become_password=ansible'     "
}
}
}
	    stage ('Docker hub'){
steps{
script{
sh "ansible-playbook ansible/docker-registry.yml  -i ansible/inventory/host.yml -e 'ansible_become_password=ansible'     "
}
}
}
	     stage ('Monitoring'){
steps{
script{
sh "/usr/local/bin/docker-compose up --build  -d"
}
}
}
	    
}
}
