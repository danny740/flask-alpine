node {

    
    stage('Checkout SCM') {
    
        checkout scm
        sh "git rev-parse --short HEAD > commit-id"
        tag = readFile('commit-id').replace("\n", "").replace("\r", "")
        appname = "hello-python:"
        registryHost = "127.0.0.1:30400/"
        imageName = "${registryHost}${appname}${tag}"

        env.BUILD_TAG=tag
    }
    /*
    stage('SonarQube') {
       
        def scannerHome = tool 'scanner';
        
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=PythonWebapp -Dsonar.sources=."
        }
    }
    */
    stage ('Build') {
        /*sh "docker build -t aa/${appname}${tag} ."*/
        sh docker build -t 'hello/python:1' .
    } 
    
    docker.image('hello/python:1').inside {
        stage('Test') {
            /* sh 'sudo -H pip install --upgrade pip' */
            /* sh 'python test-app.py' */
            /* sh 'pip install -r requirements.txt' */
            sh 'python test_app.py'
        }
    }
    /*
    stage ('Push') {
        sh "docker push ${imageName}"
    }
    stage ('Deploy') {
        sh "sed 's#127.0.0.1:30400/hello-python:version#127.0.0.1:30400/hello-python:'$BUILD_TAG'#' python-deploy.yaml | kubectl apply -f -"
    }
    */
}
        
