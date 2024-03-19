pipeline{
    agent{
        label{
            label "built-in"
            customWorkspace "/opt/git"
        }
    }
    environment {
        url = "https://github.com/gk-aws-dev/project_java.git"
        url2 = "https://github.com/gk-aws-dev/docker-example.git"
        devip = "172.31.33.102"
        
    }
    stages{
        stage ("clone repo"){
            steps{
                sh "rm -rf *"
                sh " git clone $url"
            }
        }
        stage ("buil-war"){
            steps{
                sh "cd project_java && mvn clean package -DskipTests=true"
            }
        }
        stage ("copy war"){
        steps{
            sh "cp project_java/target/LoginWebApp.war /opt/war"
            sh "scp /opt/war/LoginWebApp.war devops@${devip}:/opt/war"
        }
        }
        stage("docker-compose")
        {
            agent{
                node{
                label 'slave'
                customWorkspace "/opt/docker"
                }
            }
        steps{
            sh "rm -rf *"
            sh "git clone $url2"
            sh "cd docker-example && docker-compose down -v"
            sh "docker system prune -af"
            sh "cd docker-example && docker-compose up -d"
        }
        }
    }
}
