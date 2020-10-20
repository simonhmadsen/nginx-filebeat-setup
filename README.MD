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

_NB. This setup has only been tested with Docker for Mac, but it probably wouldn't require much to change it to work elsewhere._  

_NB. This guide also assumes that you have an installation of Humio running locally on port 8080. It could probably work just as well on another installation, but that isn't tested.
A good place to start, if you want to run it differently, would be to change the entry in the "hosts"-array in the file ```filebeat.docker.yml```in the ./config/filebeat folder, to point to the desired Humio installation._


