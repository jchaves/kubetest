pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/jchaves/kubetest.git', branch:'main'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    helloapp = docker.build("thejchaves/node-hello:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            helloapp.push("latest")
                            helloapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "simple-deploy.yaml", kubeconfigId: "hello-test-kubeconfig")
        }
      }
    }

  }

}
