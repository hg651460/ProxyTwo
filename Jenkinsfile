pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
            stage('Clean') {
                steps {
					dir('forecastweatherapi') {
					sh "mvn clean"
				}
			}
		}

		    stage('Static Code Analysis, Unit Test and Coverage') {
            
                steps {
                    dir('forecastweatherapi') {
                    sh "mvn test -Pproxy-unit-test "
                }
            }
        }		

         stage('Pre-Deployment Configuration - Caches') {
                steps {
                    dir('forecastweatherapi') {
                    println "Predeployment of Caches "
                    sh "mvn apigee-config:caches " +
                            "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                            "    -Dusername=${params.apigee_user} " +
                            "    -Dpassword=${params.apigee_pwd}"

                }
            }
        }

            stage('Pre-Deployment Configuration - targetservers') {
                steps {
                    dir('forecastweatherapi') {
                    println "Predeployment of targetservers "
                    sh "mvn apigee-config:targetservers " +
                            "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                            "    -Dusername=${params.apigee_user} " +
                            "    -Dpassword=${params.apigee_pwd}"

                }
            }
        }

            stage('Pre-Deployment Configuration - keyvaluemaps ') {
                steps {
                    dir('forecastweatherapi') {
                    println "Predeployment of keyvaluemaps  "
                    sh "mvn apigee-config:keyvaluemaps " +
                            "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                            "    -Dusername=${params.apigee_user} " +
                            "    -Dpassword=${params.apigee_pwd}"

                }
            }
        }

            stage('Build proxy bundle') {
                steps {
                    dir('forecastweatherapi') {
                    sh "mvn package -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org}"

                }
            }
        }

            stage('Deploy proxy bundle') {
                steps {
                    dir('forecastweatherapi') {
                    sh "mvn apigee-enterprise:deploy -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} -Dusername=${params.apigee_user} -Dpassword=${params.apigee_pwd}"
                }
            }
        }

            stage('Post-Deployment Configurations for API ') {
                steps {
                    dir('forecastweatherapi') {
                    println "Post-Deployment Configurations for API Products Configurations, App Developer and App Configuration "
                    sh "mvn -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                            "    -Dapigee.config.options=create " +
                            "    -Dusername=${params.apigee_user} -Dpassword=${params.apigee_pwd} " +
                            "    apigee-config:apiproducts " +
                            "    apigee-config:developers apigee-config:apps apigee-config:exportAppKeys"
                }
            }
        }

    }
}
