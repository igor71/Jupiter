pipeline {
  agent {label 'yi-tflow-vnc'}
    stages {
        stage('Import ubuntu:latest Docker Image') {
            steps {
                sh '''#!/bin/bash -xe
                   # Bacic Docker Image For Jupiter
                     image_id="$(docker images -q ubuntu:latest)"
                     echo "Available Basic Docker Image Is: $image_id"
                    
                   # Check If Docker Image Exist On Desired Server 
		                 if [ "$image_id" == "" ]; then
                        echo "Docker Image Does Not Exist!!!"
                        docker pull ubintu:latest
                     elif [ "$image_id" != "2ca708c1c9cc" ]; then
		                    echo "Old Docker Image!!! Removing..."
                        docker rmi -f ubuntu:latest
                        docker pull ubintu:latest
                     else
                        echo "Docker Image Already Exist"
                     fi
		   ''' 
            }
        }
        stage('Build Docker Image ') {
            steps {
                sh '''#!/bin/bash -xe
	       		    docker build -f Dockerfile-Jupiter -t yi/jupiter:latest .
		            ''' 
            }
        }
	    stage('Testing Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		            echo 'Hello, PyTorch_Docker'
                image_id="$(docker images -q yi/jupiter:latest)"
                  if [[ "$(docker images -q yi/jupiter:latest 2> /dev/null)" == "$image_id" ]]; then
                     docker inspect --format='{{range $p, $conf := .RootFS.Layers}} {{$p}} {{end}}' $image_id
                  else
                     echo "It appears that current docker image corrapted!!!"
                     exit 1
                  fi 
                   ''' 
		    }
		}
		stage('Save & Load Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		               echo 'Saving Docker image into tar archive'
                   docker save yi/jupiter:latest | pv | cat > $WORKSPACE/yi-jupiter-latest.tar
			
                   echo 'Remove Original Docker Image' 
	                 CURRENT_ID=$(docker images | grep -E '^yi/jupiter.*'latest'' | awk -e '{print $3}')
			             docker rmi -f yi/jupiter:latest
			
                   echo 'Loading Docker Image'
                   pv -f $WORKSPACE/yi-jupiter-latest.tar | docker load
			             docker tag $CURRENT_ID yi/jupiter:latest
                        
                   echo 'Removing temp archive.'  
                   $WORKSPACE/yi-jupiter-latest.tar
			
			            echo 'Removing Basic Docker Image'
			            docker rmi -f ubuntu:latest
                   ''' 
		    }
		}
    }
	post {
            always {
               script {
                  if (currentBuild.result == null) {
                     currentBuild.result = 'SUCCESS' 
                  }
               }
               step([$class: 'Mailer',
                     notifyEveryUnstableBuild: true,
                     recipients: "igor.rabkin@xiaoyi.com",
                     sendToIndividuals: true])
            }
         } 
}
