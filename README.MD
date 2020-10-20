# Nginx and Filebeat setup

This is a demo setup for shipping Nginx access logs through Filebeat to Humio.  
The setup was originally intended to be used to create the first version of the ```humio/nginx``` package for Humios Package Marketplace.

## Usage

To use this setup, open the file ```filebeat.docker.yml``` in the ./config/filebeat folder, and change the "password"-field to a valid API-token.

After that is done start the docker containers by using running 
```bash
docker-compose up
```
in the terminal.

After that, A simple Nginx server should be available at ```http://localhost:8081/```.  

Endpoints have been added to the Nginx-config that returns different error-codes. These endpoints can be found at ```http://localhost:8081/<errorcode>``` (for example: ```http://localhost:8081/404``` returns a 404 status code). Not all error codes have been added, but the most commonly used ones have been, to allow for different status codes to be logged to Humio.

The repo contains different config files for nginx, all located at ./config/nginx. 
The ```default.conf``` file is a substitute for the nginx-config file located at /etc/nginx/conf.d/default.conf in the nginx container.  
The two files ```nginx.apm.conf``` and ```nginx.combined.conf``` are both substitutes for the nginx-config file located at /etc/nginx/nginx.conf in the nginx container. These are different log formats (apm format and combined format respectively). The combined format is the default nginx log format, and can be parsed using the builtin accesslog parser in Humio or the "Combined"-parser in the ```humio/nginx``` package. The apm (Application Performance Metrics) format is an extended format, that contains some performance metrics for the Nginx-server (See https://www.nginx.com/blog/using-nginx-logging-for-application-performance-monitoring/ for more information). The apm format can be parsed using the builtin kv parser in Humio or the "APM"-parser in the ```humio/nginx``` package.

To enable either log format, comment in the correct volume in the ```docker-compose.yml``` file and comment out the other one.

_NB. This setup has only been tested with Docker for Mac, but it probably wouldn't require much to change it to work elsewhere._  

_NB. This guide also assumes that you have an installation of Humio running locally on port 8080. It could probably work just as well on another installation, but that isn't tested.
A good place to start, if you want to run it differently, would be to change the entry in the "hosts"-array in the file ```filebeat.docker.yml```in the ./config/filebeat folder, to point to the desired Humio installation._


