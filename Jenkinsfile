node {  
    stage('Checkout SCM') {
    
        checkout scm
        sh "git rev-parse --short HEAD > commit-id"
        tag = readFile('commit-id').replace("\n", "").replace("\r", "")
        appname = "hello-python:"
        registryHost = "127.0.0.1:30400/"
        env.imageName = "${registryHost}${appname}${tag}"

        env.BUILD_TAG=tag
    }

    stage ('Build') {
        /*sh "docker build -t aa/${appname}${tag} ."*/
        sh "docker build -t hello/python:1 ."
    } 
    
    docker.image('hello/python:1').inside {
        stage('Test') {
            /* sh 'python test_app.py'*/
            sh 'coverage run test_app.py'

            sh 'pytest --junitxml=reports/results.xml'
            /*sh 'python -m coverage xml -o ./coverage-reports/coverage.xml'*/
            junit 'reports/*.xml'
            cobertura coberturaReportFile: 'coverage.xml'
        }
    }
       
    stage('SonarQube') {
       
        def scannerHome = tool 'scanner';
        
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=PythonWebapp -Dsonar.sources=."
        }
    }

    stage('Rename image') {
        sh "docker tag hello/python:1 ${imageName}"
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
        
