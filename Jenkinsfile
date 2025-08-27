pipeline { agent any; stages { stage("Test"){ steps{ sh "mvn -B test" } } } post{ always{ junit "target/surefire-reports/**/*.xml" } } }
