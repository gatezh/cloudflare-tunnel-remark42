# Host Remark42 comments on a Raspberry pi (or any other Debian based Linux) via Cloudflare Zero Trust Tunnel

## Prerequisits
You need to have:
- Cloudflare account (free account is OK)
- Raspberry Pi with OS installed (Raspberry Pi OS, or as in my case DietPi)
- SSH access


## Running containers

There are two ways to run this:
- Starting all containers together via the root `docker-compose.yml` file which references individual `docker-compose.yml` files for each service
- Starting conteiners one-by-one by using individual `docker-compose.yml` files


If creating containers using individual `docker-compose.yml` files make sure that:

- Shared Docker network is already created
- Each individual `docker-compose.yml` has:
```yml
networks:
  - cf_tunnel_net #  which networks to attach the container to
```

- Each config has:
```yml
networks:
  cf_tunnel_net:
    external: true # tells Compose to use the existing Docker network
```