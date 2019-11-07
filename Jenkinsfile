pipeline {
  agent {
    kubernetes {
      yamlFile 'JenkinsPod.yaml'
    }
  }
  environment {
    DOCKER_IMAGE_NAME = 'webtesting'
    DOCKER_HUB_ACCOUNT = 'hhoover'
  }
 stages {
    stage('Clone Repository') {
      steps {
        checkout scm
      }
    }
    stage('Build Hugo Site') {
      steps {
        container('hugo') {
          dir ("site") {
              sh "hugo"
          }
        }
      }
    }
    stage('Validate HTML') {
      steps {
        container('html-proofer') {
          dir ("site") {
              sh ("htmlproofer public --internal-domains ${env.JOB_NAME} --external_only --only-4xx")
          }
        }
      }
    }
    stage('Docker Build & Push Image') {
      steps {
        container('docker') {
            dir ("site") {
                sh ("docker build -t ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} .")
                sh ("docker push ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
                sh ("docker tag ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:latest")
                sh ("docker push ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:latest")
            }
        }
      }
    }
    // stage('Deploy to Staging') {
    //   when { not { branch "master" } }
    //   steps {
    //     container('kubectl') {
    //         dir ("manifests/overlays/staging") {
    //             sh """
    //                 kustomize edit set image ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER};
    //                 kustomize build . | kubectl apply --record -f -
    //                """
    //         }
    //     }
    //   }
    // }
    // stage('Deploy to Production') {
    //   when { branch "master" }
    //   steps {
    //     container('kubectl') {
    //         dir ("manifests/overlays/production") {
    //             sh """
    //                 kustomize edit set image ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER};
    //                 kustomize build . | kubectl apply --record -f -
    //                """
    //         }
    //     }
    //   }
    // }
  }
}