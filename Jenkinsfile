pipeline{
    agent any
    tools{
        maven 'MAVEN'
    }
    environment{
        APP_DIR="/opt/springboot-app"
        JAR_NAME="app.jar"
        BUILD_JAR="target/demo-0.0.2-SNAPSHOT.jar"
        IMAGE_NAME="demo-app"
        CONTAINER_NAME="demo-container"
    }
    stages{
        stage('Checkout'){
            steps{
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/narasimhamarla/demo.git']])
        }
        }
        stage('Build maven project'){
            steps{
                sh "mvn clean install -DskipTests=true"
            }
        }
        stage('Test') {
            steps {
                    sh 'mvn test'
                }
            }
        stage('Docker Build & Run'){
            steps{
        

                    
                    sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    '''

                    // Build Docker image
                    sh '''
                    docker build -t $IMAGE_NAME .
                    '''

                    // Run new container
                    sh '''
                    docker run -d \
                    --name $CONTAINER_NAME \
                    -p 8081:8080 \
                    $IMAGE_NAME
                    '''
            }
        }
     stage('Deploy') {
         
         steps {
                sh 'cp $BUILD_JAR $APP_DIR/$JAR_NAME'
            }
        }
    }
}



