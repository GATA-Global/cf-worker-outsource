For deploying the Worker on Cloudflare, you have two main options:
### Option 1: Direct deployment via Cloudflare Dashboard

Log in to your Cloudflare dashboard
Navigate to "Workers & Pages"
Click "Create application"
Select "Create Worker"
In the editor that appears, delete any default code and paste in the complete index.js code
Click "Save and Deploy"
Give your worker a name (e.g., "notion-proxy")
Once deployed, go to "Triggers" and add a route:

Set the route to outsource.gataglobal.com.vn/*
Make sure it's set to "Production"



### Option 2: Using Wrangler CLI (more advanced)

If you prefer using the command line:

Install Wrangler: npm install -g wrangler
Login to Cloudflare: wrangler login
Create a new directory for your project
Create an index.js file with the code
Create a wrangler.toml file with this content:
tomlCopyname = "notion-proxy"
main = "index.js"
compatibility_date = "2023-08-15"

[triggers]
routes = [
  { pattern = "outsource.gataglobal.com.vn/*", zone_name = "gataglobal.com.vn" }
]

Deploy with: wrangler publish

DNS Configuration
Make sure your domain outsource.gataglobal.com.vn is properly configured in Cloudflare:

Ensure the domain is added to your Cloudflare account
Set up an A record for the subdomain that points to 192.0.2.1 (or any placeholder IP)
Make sure the DNS record has the orange cloud enabled (Proxied status)

There are no specific build commands needed for this Worker since it's a simple JavaScript file without dependencies. Cloudflare handles the execution environment for you.

## Add CName
Add a CNAME Record for the Subdomain
Type: CNAME

Name: outsource

Target: You typically have two options:

Option A: Point it to workers.dev if instructed by Cloudflare (i.e., the record should be “outsource.gataglobal.com.vn CNAME workers.dev”).
Option B: More commonly, point it to your Workers subdomain, which in your case is notion-proxy.gata-global-head-hunting.workers.dev.
For example:

Name: outsource
Target: notion-proxy.gata-global-head-hunting.workers.dev
TTL: Auto

Proxy status: Make sure it’s set to Proxied (orange cloud) unless you have a specific reason not to proxy.
