# Process for generating SSL certificates and enable auto renewal with the help of Certbot for Nginx
  ## Step 1: Install Certbot
  * The first step is to install Certbot on your server. This will allow you to get an SSL certificate to use with Let’s Encrypt.
  ```bash
        $ sudo add-apt-repository ppa:certbot/certbot
  ```
  * To run an update:
  ```bash
        $ sudo apt update
  ```
  * Now you can add the Nginx package for Certbot straight from the command line, again using apt:
  ```bash
        $ sudo apt install python-certbot-nginx
  ```
  ## Step 2: Configure and Confirm Nginx
  * First, find your server block, and open it using your favorite text editor, again as su.
  ```bash
  $ sudo nano /etc/nginx/sites-available/chinkara.gazelle.vision
  ```
  * With the file open, look or search for the server_name line. It should look like this:
  ```bash
  server_name api-chinkara.gazelle.vision www.api-chinkara.gazelle.vision
  ```
  ```bash
  $ sudo nginx -t
  ```
  * The next step is to re-start Nginx so it will use the correct server block. To do this, you will need to make a system call, but don’t worry. Run this command:
  ```bash
  $ sudo systemctl reload nginx
  ```
  ## Step 3: Allow HTTPS Traffic Through your Firewall
  * The next step is to allow HTTPS traffic through your existing firewall.
    * You can check your current ufw settings directly from the command line
  ```bash
  $ sudo ufw status
  ```
  * If HTTP traffic is allowed through your server, then you need to tell ufw to allow HTTPS through. Nginx already comes with a profile that will allow this, so all you need to do is enable it:
  ```bash
  $ sudo ufw allow 'Nginx Full'
  ```
  * Then disable the obsolete Nginx HTTP profile by deleting it:
  ```bash
  $ sudo ufw delete allow 'Nginx HTTP'
  ```
  * To check that these commands worked, you can verify the configuration settings for ufw in the same way as before. Run the same status command:
  ```bash
  $ sudo ufw status
  ```
  So now you have Nginx installed, with a ufw set up that will allow HTTPs traffic through.
  ## Step 4: Get an SSL Certificate
  * To run the Nginx plugin for Certbot, use this command:
  
     Here, you are running Certbot with the –nginx tag to tell it to use the plugin, and adding a -d tag in order to tell it which domains you want the certificate to be valid for.
     ```bash
     $ sudo certbot --nginx -d api-chinkara.gazelle.vision
     ```
  * The next step is to configure your HTTPs settings, and Certbot will prompt you to do so using the following output:
    ```bash
    Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
    1: No redirect – Make no further changes to the web server configuration.
    2: Redirect – Make all requests redirect to secure HTTPS access. Choose this for
    new sites, or if you’re confident your site works on HTTPS. You can undo this change by editing your web server’s configuration.
    ```
  * Please select option 2 as it re-directs to secure HTTPS access.
  * If you’ve reached this point, your certificates should be downloaded, stored, and loaded. You can test if this is the case, though, by simply reloading your website and looking for the green ‘lock’ symbol
  ## Step 5: Verifying Auto-Renewal for Certbot
  * The final step in installing any new software is to check that it works, you can check that everything is working properly by running the following command:
  ```bash
  $ sudo certbot renew --dry-run
   ```



