# Gitea Engineered by OCTOPODAMI (GEBO) SSL

You can secure your Gitea installation with an SSL certificate by following the instructions below.

+ Login into your domain registrar.
+ Update your domain `A` or `CNAME` record to point to the EC2 Instance IP Address.
+ Access your Gitea admin panel at `http://your-instance-ip:3000` and update the **Server Domain** and **Gitea Base URL** settings under Configuration > Server. For example, change from `http://your-instance-ip:3000` to `https://mydomainname.com`
+ Wait for 15-20 minutes so DNS can propagate.
+ Login into the EC2 instance via the terminal.
+ Run these commands:

```sh
sudo su -
dnf update -y
amazon-linux-extras install epel -y
dnf install certbot python3-certbot-nginx -y
sudo certbot --nginx
## Answer all the questions (this is a comment)
## Then, specify your domain name (this is a comment)
sudo certbot renew --dry-run
```

+ Update Gitea configuration to use HTTPS:

```sh
sudo nano /etc/gitea/app.ini
```

Find and update these settings:
```ini
[server]
PROTOCOL = https
DOMAIN = mydomainname.com
ROOT_URL = https://mydomainname.com/
```

+ Restart Gitea:

```sh
sudo systemctl restart gitea
```

+ Access your domain name (`https://mydomainname.com`) in the browser.

## Alternative: Using Gitea with Let's Encrypt Directly

If you prefer to configure SSL directly through Gitea without nginx:

1. Ensure your domain points to your EC2 instance
2. Install certbot:
   ```sh
   sudo dnf install certbot -y
   ```
3. Stop Gitea temporarily:
   ```sh
   sudo systemctl stop gitea
   ```
4. Obtain certificate:
   ```sh
   sudo certbot certonly --standalone -d mydomainname.com
   ```
5. Update Gitea configuration:
   ```sh
   sudo nano /etc/gitea/app.ini
   ```
   Add or update:
   ```ini
   [server]
   PROTOCOL = https
   DOMAIN = mydomainname.com
   ROOT_URL = https://mydomainname.com/
   CERT_FILE = /etc/letsencrypt/live/mydomainname.com/fullchain.pem
   KEY_FILE = /etc/letsencrypt/live/mydomainname.com/privkey.pem
   ```
6. Restart Gitea:
   ```sh
   sudo systemctl start gitea
   ```

## Links

1. [Product Website](https://aws.amazon.com/marketplace/pp/prodview-iyn7nuvxxqcjg)
2. [EULA](./octopodamiEULA.txt)
3. [Knowledgebase](https://github.com/cloudeyalimited/gitea-engineered-by-octopodami/-/wikis/home)
4. [Issue Tracking](https://github.com/cloudeyalimited/gitea-engineered-by-octopodami/-/issues)
5. [Changelog](./changelog.md)

## Support

[Email](mailto:tech@cloudeya.org) support is available to Amazon Web Services Marketplace Customers. We do not offer refunds, but you may terminate your Gitea Engineered by OCTOPODAMI (GEBO) Stack at any time.

## License

The documentation is published under [BSD 3-Clause License](license.txt).

## Copyright

(c) 2020 - 2025 [Cloudeya Limited](https://cloudeya.org).