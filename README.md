<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[circleci-image]: https://img.shields.io/circleci/build/github/nestjs/nest/master?token=abc123def456
[circleci-url]: https://circleci.com/gh/nestjs/nest

  <p align="center">A progressive <a href="http://nodejs.org" target="_blank">Node.js</a> framework for building efficient and scalable server-side applications.</p>
    <p align="center">

## Description

Nest.js Microservices with RabbitMQ, MongoDB

## Dockernize
```
Up:
docker-composer up -d

Down:
docker-compose down --rmi all -v --remove-orphans
```

## Development
Auth: http://localhost:3001/auth  
Order: http://localhost:3000  
RabbitMQ: http://localhost:15672  
(u: admin, p: password@01)  
MailHog: http://localhost:8025
