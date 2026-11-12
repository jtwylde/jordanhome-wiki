The portfolio website frontend is hosted on GitHub Pages. This allows pushes to the repo which will build and deploy it automatically.

## Blog
A wordpress blog is hosted within the homelab. It runs both the database and the frontend. We use a cloudflare tunnel to point a blog subdomain of the portfolio website towards the internal IP of the Wordpress VM within the cluster.