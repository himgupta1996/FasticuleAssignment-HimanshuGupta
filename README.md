## Hello and welcome!

This sandbox requires Docker and Python 3.9 or greater. A virtual Python environment is recommended!

Install dependencies:
```bash
pip install -e '.[dev]'
```
Run the service:
```bash
uvicorn --app-dir service main:service --reload
```
Run the tests:
```bash
pytest service
```
Access the service from your browser:

http://localhost:8000

Build a Docker image:
```bash
docker build -t fasticule:$(git rev-parse --short HEAD) fasticule:latest .
```
Run a corresponding container:
```bash
docker run --rm -it -p 8001:8001 fasticule
```
Access the Dockerized service from your browser:

http://localhost:8001

## Challenges

Select any 2 of the following 3 challenges. Once a challenge is completed, commit it with an appropriate comment so we can find your work. 

1. The service provides an http endpoint - use the provided self-signed certificate and key (in the `localhost-cert` directory) to create an https endoint for the service. Update the README to describe how to do this and how to test that it works.
1. By default, Docker containers run as root. Following the principle of least privilege, update the Dockerfile to run the service as a non-root user. 
1. Create a GitHub Actions workflow to build the docker image on each commit to the main branch. 

## Challenge 1
Using the provided self-signed certificate and key (in the `localhost-cert` directory) to create an https endoint for the service.

# Process
For running the FAST API web application, we are using Uvicorn as ASGI server. By default the service provides an http endpoint. Using SSL certificates present in the server we can make the communication between cliend and the server secure. For the server the know the presence of the self-signed certificate, we provide the location of the certificate and the public key while running the uvicorn server.

We can make the dockerized service secure using SSL by following the below steps:
1. Add the lines `COPY localhost-cert/cert.pem .` and `COPY localhost-cert/key.pem .` to the Dockerfile to copy the certificate and the public-key to the docker container where the webserver will run.
2. Add command line arguments `--ssl-keyfile key.pem` and `--ssl-certfile cert.pem` to tell Uvicorn the location of these files for it to expose HTTPS endpoint.
3. Now run and build and run the docker container the way same as described above.
4. To test that the webserver is now exposed via HTTPS endpoint and not HTTP, one can go to the browser and access the site using https://localhost:8001. As the CA for the certificate is not verified(because the certificate is self-signed) the site might not be trusted and can signal `Not Secure` site.
