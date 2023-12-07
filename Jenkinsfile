pipeline {
    agent any

     stages {
        stage('Get Code') {
            steps {
                // Obtener código del repo
                git 'https://github.com/carlosbarreto-info/helloworld.git'
            }
        }
    
        stage('Build') {
           steps {
              echo 'Eyyy, esto es Python. No hay que compilar nada!!!'
	          echo WORKSPACE
              bat 'dir'
           }
        }
        
        stage('Unit') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat '''
                        set PYTHONPATH=C:\\Users\\cbrr3\\AppData\\Local\\Programs\\Python\\Python312\\Scripts\\
                        C:\\Users\\cbrr3\\AppData\\Local\\Programs\\Python\\Python312\\Scripts\\pytest --junitxml=result-unit.xml test\\unit
                    '''
               }
            }
        }   


        stage('Rest') {
            steps {
                bat '''
                    set FLASK_APP=app\\api.py
                    set FLASK_ENV=development
                    start flask run
                    start java -jar E:\\Caso1\\helloworld\\wiremock\\wiremock.jar --port 9090 --root-dir E:\\Caso1\\helloworld\\wiremock\\
                    set PYTHONPATH=%WORKSPACE%
                    C:\\Users\\cbrr3\\AppData\\Local\\Programs\\Python\\Python312\\Scripts\\pytest --junitxml=result-rest.xml test\\rest
                '''
            }    
        }

        stage('Results') {
            steps {
                junit 'result*.xml' 
            }
        }
     
    }
}

