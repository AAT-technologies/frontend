pipeline {
  agent any
  stages {
    stage ('Testing') {
      steps {
          git branch: 'dev', credentialsId: 'for-git', url: 'https://github.com/AAT-technologies/frontend.git'
          sh ''' sudo docker login -u delalixx -p dckr_pat_-dfSKHYHBVZNLTVX1R5sxmNGJwo
          '''
          sh ''' sudo docker system prune -af
          '''
         
         sh ''' cd app-front/frontend
                   ls
                   sudo docker build -t delalixx/frontend:v1 .
                   sudo docker push delalixx/frontend:v1
                   '''
         sh ''' sudo docker system prune -af
                  '''
         
      }
    }
     stage ('Create Deploy to Yaml file') {
       steps {
         sh ''' ls
         '''
           withCredentials([aws(credentialsId: 'aws-credentials', region: 'ca-central-1')]) {
           sh 'kubectl version --client --output=yaml'
           sh '''
                 aws eks --region ca-central-1 update-kubeconfig --name boot-dev
                 kubectl config current-context
                 kubectl config use-context arn:aws:eks:ca-central-1:487585538889:cluster/boot-dev 
                 kubectl apply -f cluster.yaml
                 kubectl get node
                 kubectl get service
                 kubectl get service frontend-external
                 '''
           }
        }
     }
  }
}
