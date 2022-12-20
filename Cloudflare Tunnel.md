## Cloudflare Tunnel


I do everthing in the cli

Lets install the cloudflare tunnel service.
in my case i will do the install on een ubuntu machine.

```
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && sudo dpkg -i cloudflared-linux-amd64.deb
```

When you run the flowing command you get a url. login to cloudflare
```
cloudflared tunnel login
```

when cloudflare is connected you get a cert.pem.
make a note of the location ;)

Lets make the tunnel
by name fill the name that you want for the tunnel.

```
cloudflared tunnel create <NAME>
```

Now we will create a configuration file in the `.cloudflared` directory (the same one that our **cert.pem** file was written to)

```
nano /home/$USER/.cloudflared/config.yaml
```

there is an emty file.
Add the following lines.
```
tunnel:  # tunnel id
credentials-file: /home/$USER/.cloudflared/1d4537b6-67b9-4c75-a022-ce805acd5c0a.json # the location of your credentials file from the previous step
```

add your first site example.com

```
cloudflared tunnel route dns <name of the tunnel> <example.com>
```

Now lets create the ingress.
past it in your config.yml file

```
ingress:
  - hostname: example.com 
    service: http://internalip:80
  - hostname: sub.example.com
    service: http://internalip:88
  - service: http_status:404 # this is required as a 'catch-all'
```

Lets start the tunnel

```
cloudflared tunnel run <name of your tunnel>
```

Make a service to run automatic

```
cloudflared service install
```

start en enable the service

```
systemctl enable --now cloudflared
```

## Setup complete!