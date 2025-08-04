







# Deploy To Kubernetes: 

```yaml
        stage('Deploy To Kubernetes') {
            steps {
               withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.16.131:6443') {
                        sh "kubectl apply -f deployment-service.yaml"
                }
```
step 1 - create service account and secete and configure Secrete in jenkins (Refer  Phase 3 - SVCAccount )

step 2 - Pipeline Syntax take help

```yaml
- Sample Step - withKubeconfig: Configure Kubeernets CLI (Kubectl)
- Credentials - k8-cred

- Kubernetes server endpoint - https://192.168.16.131:6443

goto kubernets master server used below command  for server end point 
root@Master:/home/natraj# cd ~/.kube/
root@Master:~/.kube# ls
cache  config
root@Master:~/.kube# cat config

- Cluster name - kubernetes --- ( Copy from above command )
- Namespace - webapps
```

#  After that we need to create in GitHub Repo file is deployment-service.yaml

cjange docker name -natrajwadhai13/boardshack:latest
 name: boardgame-deployment
 app: boardgame
 - name: boardgame
   refer github file

# imp concepts
- Cluser ip - its interner ip
- load balencer - outside of word 
- node port - no need to used - we are using worker ip
