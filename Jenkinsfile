  node{
      def dockerImageName= 'mohdmuneer/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/Mohd-muneer/java-groovy-docker.git' 
      }
      stage('Build'){
         // Get maven home path and build
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "/usr/bin/mvn package -Dmaven.test.skip=true"
      }       
     
     stage ('Test'){
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "/usr/bin/mvn verify; sleep 3"
      }
      
     stage('Build Docker Image'){         
           sh "docker build -t ${dockerImageName} ."
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: variable: 'dockerPWD')]) {
              sh '''docker login -u mohdmuneer -p Muneer@7860'''

         }
        sh "docker push ${dockerImageName}"
      }
      
    stage('Run Docker Image'){
            def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            def changingPermission='sudo chmod +x stopscript.sh'
            def scriptRunner='sudo ./stopscript.sh'           
            def dockerRun= "sudo docker run -p 8085:8080 -d --name ${dockerContainerName} ${dockerImageName}" 
            withCredentials([string(credentialsId: 'deploymentserverpwd', variable: 'dpPWD')]) {
            //      sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196" 
            //      sh "sshpass -p ${dpPWD} scp -r stopscript.sh devops@52.76.172.196:/home/devops" 
            //      sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${changingPermission}"
            //      sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${scriptRunner}"
            //      sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${dockerRun}"
                    sh "${dockerRun}"
            }
            
      
      }

  }
