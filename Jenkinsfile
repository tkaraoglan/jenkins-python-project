pipeline{
    agent any
    environment {
        MYSQL_DATABASE_HOST = "database-42.cbanmzptkrzf.us-east-1.rds.amazonaws.com"
        MYSQL_DATABASE_PASSWORD = "Clarusway"
        MYSQL_DATABASE_USER = "admin"
        MYSQL_DATABASE_DB = "phonebook"
        MYSQL_DATABASE_PORT = 3306
    }
    stages{
        stage('compile'){
            agent{
                docker{
                    image 'python:alpine'
                }
            }
            steps{
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'pip install -r requirements.txt'
                    sh 'python -m py_compile src/*.py'
                    stash(name: 'compilation_result', includes: 'src/*.py*')
                }   
            }
        stage('test') {
            agent {
                docker {
                    image 'python:alpine'
        }
    }
            steps {
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'python -m pytest -v --junit-xml results.xml src/appTest.py'
        }
    }
        stage('build'){
            agent any
            steps{
                sh "docker build -t tkaraoglan/ilkdeneme ."
                sh "docker tag tkaraoglan/ilkdeneme 950905626774.dkr.ecr.us-east-1.amazonaws.com/deneme1/tkaraoglan/ilkdeneme:latest"
    }
}
        stage('push'){
            agent any
            steps{
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 950905626774.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 950905626774.dkr.ecr.us-east-1.amazonaws.com/deneme1:latest"
    }
}
        stage('compose'){
        agent any
        steps{
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 950905626774.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker-compose up -d"
    }
}        
        post {
            always {
                junit 'results.xml'
        }
    }
}
        }
    }
}
