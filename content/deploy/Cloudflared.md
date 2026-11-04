Cloudflared is used to create tunnels for exposing internal apps to the public internet, proxied through Cloudflare.

## Creating a route
Assuming a tunnel is already created in Cloudflare, the below steps will create a new route to an internal service. For these steps, we're assuming we're publishing the internal service `service.jordanhome.dev` on the public domain `service.mydomain`

1. Navigate to **Networks** > **Tunnels** and select the tunnel
2. Select **Routes** > **Add Route** > **Published Application**
    1. Subdomain: Subdomain for service on the public domain. `service`
    2. Domain: Domain to use. `mydomain`
    3. Service URL: local URL for the published service. `https://service.jordanhome.dev`
    4. Additional application settings:
        1. HTTP Host Header: `service.jordanhome.dev`
        2. TLS > Origin Server Name: `service.jordanhome.dev`
3. Select **Save Changes**