node {
//   def mvnHome
   stage('Get repo') { 
      git 'https://github.com/dburianov/opsworksio.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      // mvnHome = tool 'M3'
   }
//   stage('Make Docker container and compile in it nginx and upload from it') {
      // 
//      sh 'cd ./build && sudo docker build -t nginx-lua-latest .'
//      sh 'sudo docker run --name temp-container-name nginx-lua-latest /bin/true'
//      sh 'sudo docker cp temp-container-name:/usr/src/nginx/nginx-lua_0-1_amd64.deb ./deb/nginx-lua_0-1_amd64.deb'
//      sh 'sudo docker rm temp-container-name'
//   }
//   stage('Create Docker container and install nginx-lua.deb to it and add configs to it') {
//      sh 'cd ./docker && sudo docker build -t nginx-lua-latest .'
//   }
   stage('allinone') {
      sh 'cd ./allinone && docker build -t nginx-lua-latest .'
   }
   stage('Publish to docker hub') {
      sh 'docker tag nginx-lua-latest dburianov/nginx-lua-latest'
      sh 'docker push dburianov/nginx-lua-latest'
//      sh 'docker rmi dburianov/nginx-lua-latest'
   }
   stage('Deploy to aws') {
      sh 'docker-machine create --driver amazonec2 --amazonec2-open-port 80 --amazonec2-region us-east-1 vm-nginx-lua-latest && docker-machine env vm-nginx-lua-latest && eval $(docker-machine env vm-nginx-lua-latest) && docker run -d -p 80:80 -v /logs:/usr/local/nginx/logs --name webserver dburianov/nginx-lua-latest'
//      sh 'docker run -d -p 80:80 -v /logs:/usr/local/nginx/logs --name webserver dburianov/nginx-lua-latest'
   }
}

