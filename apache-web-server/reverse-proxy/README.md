# Reverse Proxy Configuration for Apache2

In most online guides, reverse proxy configuration is often done using Nginx. However, if you already have Apache running as your web server, you can configure a reverse proxy on Apache as well. Below is a summarized guide on configuring a reverse proxy in Apache2, tested and compiled after thorough research and testing.

---

## Assumptions

- The service you want to reverse proxy to is running on `127.0.0.1:3000`.

---

## 1. Configure HTTP Virtual Host

To configure the HTTP Virtual Host, follow these steps:

1. Open the Apache configuration file for your default virtual host:

   ```bash
   sudo vim /etc/apache2/sites-available/000-default.conf
   ```

2. Add the following lines inside the `<VirtualHost>` block, above `</VirtualHost>`:

   ```apache
   ProxyPass / http://localhost:3000/
   ProxyPassReverse / http://localhost:3000/
   ```

This will redirect traffic coming to `http://yourdomain.com` to `http://localhost:3000`.

---

## 2. Configure HTTPS Virtual Host

To configure the HTTPS Virtual Host, follow these steps:

1. Open the Apache SSL configuration file:

   ```bash
   sudo vim /etc/apache2/sites-available/default-ssl.conf
   ```

2. Add the following lines above `</VirtualHost>`:

   ```apache
   ProxyRequests     Off
   ProxyPreserveHost On
   ProxyPass        / http://localhost:3000/
   ProxyPassReverse / http://localhost:3000/
   RequestHeader set X-FORWARDED_PROTO "https"
   RequestHeader set X-Forwarded-Port "443"
   SetEnv proxy-nokeepalive 1
   SetEnv proxy-initial-not-pooled 1
   #SetEnv force-proxy-request-1.0 1
   ```

This will redirect traffic coming to `https://yourdomain.com` to `http://localhost:3000` and preserve the original host.

---

## 3. Enable Necessary Modules

To make the reverse proxy work, you need to enable several Apache modules:

1. Enable the proxy and related modules:

   ```bash
   sudo a2enmod proxy
   sudo a2enmod proxy_http
   sudo a2enmod headers
   sudo a2enmod proxy_balancer
   sudo a2enmod lbmethod_byrequests
   ```

2. Ensure that SSL is enabled if you are configuring the reverse proxy for HTTPS:

   ```bash
   sudo a2enmod ssl
   ```

---

## 4. Restart Apache2

Once all configurations are done, restart the Apache service to apply the changes:

```bash
sudo systemctl restart apache2.service
```

---

## Final Result

After completing the steps above, any traffic received on `http://yourdomain.com` or `https://yourdomain.com` will be forwarded to `127.0.0.1:3000`, and the application webpage/portal will be displayed.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

---

### Notes:
- The `README.md` has a clean structure with headings, code blocks, and steps for clarity.
- The final section includes a license (MIT License). If you need to use a different license, adjust the text accordingly.
