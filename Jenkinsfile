pipeline{
    agent any
    parameters{
        string(name: 'bucketname', defaultValue: 'bucket name', description: 'bucket name')
        string(name: 'hostname', defaultValue: 'host name', description: 'host name')
    }
    stages{
        stage('Build') {
            steps {
                script{
                sh '''
                echo 'Building Flask application...'
                rm -fr *.zip
                zip -r FlaskApp-$BUILD_NUMBER.zip *
                aws s3 cp FlaskApp-$BUILD_NUMBER.zip s3://${bucketname}/
                scp dependencies.sh ec2-user@${hostname}:/home/ec2-user/
                rm -fr *
                echo 'Flask application built successfully!'
                '''
                }
            }
        }
        stage('Deploy') {
            steps {
                script{
                sh '''
                echo 'Deploying Flask application...'
                ssh ec2-user@${hostname} "cd /home/ec2-user/ && sudo rm -rf *"
                aws s3 cp s3://${bucketname}/FlaskApp-${BUILD_NUMBER}.zip .
                scp FlaskApp-${BUILD_NUMBER}.zip ec2-user@${hostname}:/home/ec2-user/
                ssh ec2-user@${hostname} "cd /home/ec2-user/ && unzip FlaskApp-${BUILD_NUMBER}.zip"
                ssh ec2-user@${hostname} "cd /home/ec2-user/ && sudo rm -rf *.zip"
                rm -fr *.zip
                echo 'Flask application deployed successfully!'
                '''
                }
            }
        }
        stage('Check Dependencies') {
            steps {
                script {
                    sh '''
                        echo 'Checking if dependencies are installed...'
                        ssh ec2-user@${hostname} 'sudo python3 -c \"import flask\"' || exit 1
                        echo 'Dependencies already installed. Skipping installation stage...'
                    '''
                }
                post {
                    failure {
                        stage('Installation') {
                            steps {
                                script {
                                    sh '''
                                        echo 'Installing dependencies...'
                                        ssh ec2-user@${hostname} "sh dependencies.sh"
                                        echo 'Dependencies installed successfully!'
                                    '''
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Run'){
            steps{
                script{
                sh '''
                echo 'running the flask application'
                ssh ec2-user@${hostname} "sudo nohup python3 app.py &"
                echo 'completed successfully'
                '''
                }
            }
        }
    }
}
