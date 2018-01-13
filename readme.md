![alibaba cloud](https://github.frapsoft.com/top/open-source-v1.png)

# Terraform Consul Backend Docker
[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.svg)](https://github.com/ellerbrock/open-source-badges/) [![Gitter Chat](https://badges.gitter.im/frapsoft/frapsoft.svg)](https://gitter.im/frapsoft/frapsoft/) [![MIT Licence](https://badges.frapsoft.com/os/mit/mit.svg?v=103)](https://opensource.org/licenses/mit-license.php)

## Configuration

Quick and dirty Setup to use [Consul](https://www.consul.io/) as [Backend](https://www.terraform.io/docs/backends/types/consul.html) for [Terraform](https://www.terraform.io/).  
I currently need a quick solution for Alibaba Cloud to store my state.  
This is just a first test without SSL or any deep dive in security or stability.  
But it works and from there we can take it futher next time ...


Run with `docker-compose up` and you can see the WebGUI on <http://localhost:8500>.

### `docker-compose.yml`

```
version: '3'

services:
  consul:
    image: consul:latest
    container_name: consul
    volumes:
      - consul-data:/consul/data
    environment:
      - CONSUL_HTTP_TOKEN="supersecure"
    ports:
      - 8300:8300/tcp
      - 8301:8301/tcp
      - 8301:8301/udp
      - 8302:8302/tcp
      - 8302:8302/udp
      - 8400:8400/tcp
      - 8500:8500/tcp
      - 8600:8600/tcp
      - 8600:8600/udp
    networks:
      - devops

networks:
  devops:
    driver: bridge

volumes:
  consul-data:
```

### `main.tf`

```
terraform {
  required_version = ">= 0.11.2"

  backend "consul" {
    address         = "localhost:8500"
    path            = "tf/state"
    access_token    = "supersecure"
    lock            = true
  }
}
```

### What's next

- secure transfere via ssl certificates
- further look how to use vault to store secrets
- cluster setup

## Contact

[![Github](https://github.frapsoft.com/social/github.png)](https://github.com/ellerbrock/)[![Docker](https://github.frapsoft.com/social/docker.png)](https://hub.docker.com/u/ellerbrock/)[![npm](https://github.frapsoft.com/social/npm.png)](https://www.npmjs.com/~ellerbrock)[![Twitter](https://github.frapsoft.com/social/twitter.png)](https://twitter.com/frapsoft/)[![Facebook](https://github.frapsoft.com/social/facebook.png)](https://www.facebook.com/frapsoft/)[![Google+](https://github.frapsoft.com/social/google-plus.png)](https://plus.google.com/116540931335841862774)[![Gitter](https://github.frapsoft.com/social/gitter.png)](https://gitter.im/frapsoft/frapsoft/)

## License 

[![MIT license](https://badges.frapsoft.com/os/mit/mit-125x28.png?v=103)](https://opensource.org/licenses/mit-license.php)

This work by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/ellerbrock" property="cc:attributionName" rel="cc:attributionURL">Maik Ellerbrock</a> is licensed under a <a rel="license" href="https://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a> and the underlying source code is licensed under the <a rel="license" href="https://opensource.org/licenses/mit-license.php">MIT license</a>.
