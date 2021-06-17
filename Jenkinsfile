pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/sandyvis34/demo.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("sandyvis34/demo:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig"){
          
                  sh 'kubectl create -f $WORKSPACE/hellowhale.yml'
                  sh 'kubectl get pods'
                  sh 'kubectl get svc'
                  echo 'open minikube ip and svc port which is 31113 in browser'
                  echo 'deployment completed ........'

          }
        }
      }
    }

  }

}
