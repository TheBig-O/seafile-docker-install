# seafile-docker-install
This is a quick rundown of the steps I took to get up and running with Seafile using Docker.

Much of this comes from an excellent video from [DB Tech](https://www.youtube.com/watch?v=GZNuVcx-Akk). After you watch this video, you'll have a pretty good start on your setup.

Setting up your reverse proxy is a bit more involved and will take a bit of trial and error. If you're not using Apache, like Nginx or other webserver, I'm not sure I can be much help. Below is the basic setup I have for my site file. 

For Docker, I've provided a docker compose file. All you need to do is adjust the ports, passwords, and directories to suit your specific use case. 
Once you've adjust things to your liking, run the docker-compose file with `deploy with: docker-compose up -d`
> Do not use symlinks for any of your settings. Seafile Does Not Like Symlinks!
> Create directories on your host and point Docker to them.
> I use: `/mnt/dev-disk-by-label-xxx/docker-vols/seafile/` and add subfolders as needed.

A subdomain creates a pathway that can be used by both the web app and the dedicated apps for your phone or computer. [This](https://manual.seafile.com/deploy/deploy_with_apache/) site will give you some more information about reverse proxies and Seafile.
> Note: You need to make sure you set your SERVICE_URL and FILE_SERVER_ROOT either in the WebApp settings or through the `ccnet.conf` file.
> Mine are set to my backend server's ip as follows: http://192.168.1.xxx:8080 and http://192.168.1.xxx:8080/seafhttp respectively.
> **Failing to do this will prevent you from uploading/downloading any files!**

```
<VirtualHost *:443>

#Seafile  Proxy to OMV
        ServerName seafile.domain.com
        ServerAdmin email@gmail.com
        ServerAlias seafile.domain.com
        ProxyPass "/" "http://192.168.1.xxx:9080/"
        ProxyPassReverse "/" "http://192.168.1.xxx:9080/"
        ProxyPass "/seafhttp" "http://192.168.1.xxx:8000"
        ProxyPassReverse "/seafhttp" "http://192.168.1.xxx:8000"

... additional vhost settings

</VirtualHost>
```

If you need a Free DNS server that will allow all your subdomains, I recommend [FreeDNS](https://freedns.afraid.org/)
