node {

	def pom = readMavenPom file: 'pom.xml'
    def developmentArtifactVersion = ''
    developmentArtifactVersion = "${pom.version}"
    def name = "${pom.name}"
    def jarName = "${name}-${developmentArtifactVersion}"

def check() {
    env.CI_SKIP = "false"
    result = sh (script: "git log -1 | grep '.*\\[devs\\].*'", returnStatus: true)
    if (result == 0) {
        env.CI_SKIP = "true"
        error "'[devs]' found in git commit message. Aborting."
    }
}

def postProcess() {
    if (env.CI_SKIP == "true") {
        currentBuild.result = 'NOT_BUILT'
    }
}
    
    echo "Version del POM = $jarName" 
		
	checkout scm
  	
	  result = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim() 

	  echo "El valor de Result es = $result"
  	
	  if (result == "[devs]") {

		echo "Develoop Stages..."

		stage('Iniciando Ejecución'){
		    echo 'Tardara unos minutos...'
		}

		stage('Compilacion'){
			    echo '****************************************************'
				echo 'Version de Java '
				sh 'java -version'
				sh 'mvn -version'
				echo '****************************************************'
				echo 'Limpiando y Compilando aplicacion'	
				echo '****************************************************'
				sh 'mvn clean package -Dmaven.test.skip=true'
		}
		
		stage('Sonarqube Coverage'){
				echo '****************************************************'
				echo 'Calificando Covertura de codigo'
				echo '****************************************************'
				sh 'mvn test'
				sh 'mvn sonar:sonar -Dsonar.host.url=http://10.51.251.49:9000 -Dsonar.login=50d71f88d26ade7fde4b2479c78049317b9a27ff'
		}

		stage('Construccion'){

				echo '****************************************************'
				echo '******************Inhabilitado**********************'
				echo '****************************************************'
				
		}

		stage('Dockerizando'){

				echo '****************************************************'
				echo '******************Inhabilitado**********************'
				echo '****************************************************'
				
		}

		stage('Upload JAR to Nexus repository'){
				
				echo '****************************************************'
				echo '******************Inhabilitado**********************'
				echo '****************************************************'
		}

		stage('Upload Docker Image to Nexus repository'){
				echo '****************************************************'
				echo '******************Inhabilitado**********************'
				echo '****************************************************'
		}

		stage('Upload to Harbor repository'){
		
		        echo 'Subiendo a Harbor'
				//sh 'docker push 10.51.251.49:8082/data-persistence'
				
		}
		
		stage('Cerrando Ejecución'){
		    echo 'Completado'
		}


  } else if(result == "[prod]") {

	  echo "Production Stages..."

	  	stage('Iniciando Ejecución'){
		    echo 'Tardara unos minutos...'
		}

	  	stage('Compilacion'){
				echo 'Version de Java'
				sh 'java -version'
				sh 'mvn -version'
				echo 'Limpiando y Compilando aplicacion'	
				sh 'mvn clean package -Dmaven.test.skip=true'
		}
		
		stage('Sonarqube Coverage'){
				echo 'Calificando Covertura de codigo'
				sh 'mvn test'
				sh 'mvn sonar:sonar -Dsonar.host.url=http://10.51.251.49:9000 -Dsonar.login=50d71f88d26ade7fde4b2479c78049317b9a27ff'
		}
		
		stage('Construccion'){
				echo 'Creando JAR'
				sh 'mvn install -Dmaven.test.skip=true'
		}

		stage('Dockerizando'){
				echo 'Dockerizando aplicacion'
				sh 'docker images'
				sh 'mvn package dockerfile:build'
		}

		stage('Upload JAR to Nexus repository'){
				
				echo 'Subiendo JAR'
				sh 'mvn deploy'
		}

		stage('Upload Docker Image to Nexus repository'){
				echo 'Subiendo Imagen docker'
				sh 'docker push 10.51.251.49:8082/spring-admin'
		}

		stage('Upload to Harbor repository'){
		
		        echo 'Subiendo a Harbor'
				//sh 'docker push 10.51.251.49:8082/data-persistence'
				
		}
		
		stage('Cerrando Ejecución'){
		    echo 'Completado'
		}

	}
    
}
