pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'master', url: 'https://github.com/ivancuadros1988/netcore50-api-test'
            }
        }
        stage('Build') {
            steps {
                bat(script: 'dir', returnStdout: true);
                bat(script: 'dotnet restore', returnStdout: true);
                bat(script: 'dotnet build', returnStdout: true);
                bat(script: 'dotnet test', returnStdout: true);
            }
        }
        stage('Docker') {
            steps {
                bat(script: 'docker login --username %UsernameDockerHub% --password %PasswordDockerHub%', returnStdout: true);
                bat(script: 'docker build -t ivancuadros1988/servicenet5 .', returnStdout: true);
                bat(script: 'docker push ivancuadros1988/servicenet5', returnStdout: true);
            }
        }
        stage('Deploy Dev') {
            steps {
                bat(script: 'az login --service-principal --username 05a7db38-0a16-47d0-ab89-4530f147d3ed --password WMJ9fo2.jg_ZwNoTEGDvPrm4FkZeL.Ladx --tenant 5dd0bef7-7843-442d-806f-0c18c1da4f53', returnStdout: true);
                bat(script: 'az account set --subscription "PPUAforo255"', returnStdout: true);
                bat(script: 'az container delete --resource-group Jenkins --name micro5testservice --yes', returnStdout: true);
                bat(script: 'az container create --resource-group Jenkins --name micro5testservice --image ivancuadros1988/servicenet5:latest --dns-name-label micro5testservice --ports 80', returnStdout: true);
            }
        }
        stage('Deploy Prod') {
            steps {
                bat(script: 'az aks get-credentials --resource-group Jenkins --name aforo255jenkins & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'kubectl config use-context aforo255jenkins --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
        }
    }
}