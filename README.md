# remark42-cloudflare-tunnel

Host Remark42 comments on a Raspberry Pi (or any other Debian based Linux) via Cloudflare Zero Trust Tunnel



## Prerequisits
You need to have:
- Cloudflare account (free account is OK)
  - Domain name managed by Cloudflare
- Raspberry Pi with OS installed (Raspberry Pi OS, or as in my case DietPi)
  - SSH access to it



## Clone the repo

The easiest way I find to do that is by using [GitHub CLI (gh)](https://cli.github.com) to create a repository using an existing repository as a template:

```bash
gh repo create remark42 --template gatezh/remark42-cloudflare-tunnel --private --clone
```

This will:

- Create a local folder `reamark42` with this repo
- Create a private GitHub repository named `remark42`



## Preparation

> ⚠️ Here and further replace `example.com` with your domain name



### Cloudflare

1. In Cloudflare navigate to [`Dashboard`](https://dash.cloudflare.com) > `Zero Trust` > `Networks` > `Tunnels `> `+ Create a tunnel`

2. Select Cloudflared

3. Enter `Tunnel name` > `Save tunnel`

4. On the **Choose your environment** step select `Docker`

5. The suggested command will have `--token` value to use in the next step

6. Navigate to `Public hostnames` tab > `+ Add a public hostname`

7. Fill out the fields

   - Hostaname
     - Subdomain: `commnets`
     - Domain: `example.com`
     - Path: *leave blank*

   - Service
     - Type: `HTTP`
     - URL: `comments.example.com:8080`

8. Press **Complete setup**



### `cloudflared` container

In `cloudflared` directory create `.env` file with the following values:

```
TUNNEL_TOKEN=...
```

`TUNNEL_TOKEN` value is from step 5 of the previous section.



### `comments.example.com` container

> ✅ First rename directory to according to your domain name

In `comments.example.com` directory create `.env` file with the following values:

```
# comments.example.com

SECRET=...
ADMIN_PASSWD=...

# Admins
ADMIN_SHARED_ID=...

# GitHub OAuth
AUTH_GITHUB_CID=...
AUTH_GITHUB_CSEC=...
```




## Running containers

1. Create `remark42_tunnel` docker network

   ```
   docker network create remark42_tunnel
   ```

2. Navigate to `cloudflared` directory and start the container

   ```
   docker compose up -d
   ```

3. Navigate to `comments.example.com` directory and start the container

   ```
   docker compose up -d
   ```



## Scaling up

You can run multiple `remark42` instances. For that:
- [ ] duplicate the `comments.example.com` directory template and make configuration changes
- [ ] add another public hostname in Cloudflare
- [ ] start the container