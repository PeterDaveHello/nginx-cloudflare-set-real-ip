# nginx-cloudflare-set-real-ip

Generate config to set correct client IP address in nginx, based on Cloudflare's IP address and `CF-Connecting-IP` header.

The script will fetch the latest Cloudflare IP addresses and generate corresponding nginx config file in `/etc/nginx/conf.d/cloudflare-set-real-ip.conf`

Use a cronjob to trigger this IP update script periodically, and reload your nginx instance for the new config.

## Usage

Directly execute the script, you can clone this repository, or directly download the [script](https://github.com/PeterDaveHello/nginx-cloudflare-set-real-ip/raw/master/generate.sh) to where you want to execute it.

The generated config will be written to `/etc/nginx/conf.d/cloudflare-set-real-ip.conf`, please make you have the write permission to the locaion. You can also set the config location by using variable `$cf_ip_config_path`.

To prevent burst request in the same time, cron mode is supported, with parameter `--cron`, the script will randomly sleep within 15 mins before fetching the latest IP ranges.

## Example

### Oneliner script without download or git clone

```sh
wget -O- https://github.com/PeterDaveHello/nginx-cloudflare-set-real-ip/raw/master/generate.sh | cf_ip_config_path="/dev/shm/cloudflare_ip.conf" sh
```

### Cronjob

`/etc/cron.d/opt/nginx-cloudflare-set-real-ip`:

```cron
1 1 * * * root /opt/nginx-cloudflare-set-real-ip/generate.sh --cron && /usr/sbin/service nginx reload
```

## Reference

- https://www.cloudflare.com/ips/
- https://support.cloudflare.com/hc/en-us/articles/200170986-How-does-Cloudflare-handle-HTTP-Request-headers-
