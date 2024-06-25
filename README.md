# private-docker-registry
creating a private docker registry and deploying on kubernetes with tls  

### create tls certificates  
$openssl genpkey -algorithm RSA -out registry.example.com.key  
$openssl req -new -key registry.example.com.key -out registry.example.com.csr  
$openssl x509 -req -in registry.example.com.csr -signkey registry.example.com.key -out registry.example.com.cer  

### create secret to pass the tls for ingress
$kubectl create secret tls registry-example-com-tls --cert=registry.example.com.cer --key=registry.example.com.key  

### create username & password file for registry
$mkdir -p /home/manish/registry/auth  
$cd /home/manish/registry/auth  
$htpasswd -Bc registry.passwd [username]  

### create daemon.json for insecure-registires  
$sudo vim /etc/docker/daemon.json  
{  
  "insecure-registries": ["registry.example.com"]  
}  

### restart docker  
Execute all manifest file  

### docker login  
$docker login https://registry.example.com/v2/  

### check repo images  
$https://registry.example.com/v2/_catalog  

### docker push  
$docker push registry.example.com/manish-hello-world:v1  
