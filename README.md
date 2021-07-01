# LibreTranslate

[Try it online!](https://libretranslate.com) | [API Docs](https://libretranslate.com/docs) | [Community Forum](https://community.libretranslate.com/)

[![Python versions](https://img.shields.io/pypi/pyversions/libretranslate)](https://pypi.org/project/libretranslate) [![Run tests](https://github.com/uav4geo/LibreTranslate/workflows/Run%20tests/badge.svg)](https://github.com/uav4geo/LibreTranslate/actions?query=workflow%3A%22Run+tests%22) [![Publish to DockerHub](https://github.com/uav4geo/LibreTranslate/workflows/Publish%20to%20DockerHub/badge.svg)](https://hub.docker.com/r/libretranslate/libretranslate) [![Publish to GitHub Container Registry](https://github.com/uav4geo/LibreTranslate/workflows/Publish%20to%20GitHub%20Container%20Registry/badge.svg)](https://github.com/uav4geo/LibreTranslate/actions?query=workflow%3A%22Publish+to+GitHub+Container+Registry%22) [![Awesome Humane Tech](https://raw.githubusercontent.com/humanetech-community/awesome-humane-tech/main/humane-tech-badge.svg?sanitize=true)](https://github.com/humanetech-community/awesome-humane-tech)

Free and Open Source Machine Translation API, entirely self-hosted. Unlike other APIs, it doesn't rely on proprietary providers such as Google or Azure to perform translations.

![image](https://user-images.githubusercontent.com/1951843/121782367-23f90080-cb77-11eb-87fd-ed23a21b730f.png)

[Try it online!](https://libretranslate.com) | [API Docs](https://libretranslate.com/docs)

## API Examples

Request:

```javascript
const res = await fetch("https://libretranslate.com/translate", {
	method: "POST",
	body: JSON.stringify({
		q: "Hello!",
		source: "en",
		target: "es"
	}),
	headers: { "Content-Type": "application/json" }
});

console.log(await res.json());
```

Response:

```javascript
{
    "translatedText": "¡Hola!"
}
```

## Install and Run

You can run your own API server in just a few lines of setup!

Make sure you have installed Python (3.8 or higher is recommended), then simply issue:

```bash
pip install libretranslate
libretranslate [args]
```

Then open a web browser to http://localhost:5000

If you're on Windows, we recommend you [Run with Docker](#run-with-docker) instead.

## Build and Run

If you want to make some changes to the code, you can build from source, and run the API:

```bash
git clone https://github.com/uav4geo/LibreTranslate
cd LibreTranslate
pip install -e .
libretranslate [args]

# Or
python main.py [args]
```

Then open a web browser to http://localhost:5000

### Run with Docker

Simply run:

```bash
docker run -ti --rm -p 5000:5000 libretranslate/libretranslate
```

Then open a web browser to http://localhost:5000

### Build with Docker

```bash
docker build [--build-arg with_models=true] -t libretranslate .
```

If you want to run the Docker image in a complete offline environment, you need to add the `--build-arg with_models=true` parameter. Then the language models get downloaded during the build process of the image. Otherwise these models get downloaded on the first run of the image/container.

Run the built image:

```bash
docker run -it -p 5000:5000 libretranslate [args]
```

Or build and run using `docker-compose`:

```bash
docker-compose up -d --build
```

> Feel free to change the [`docker-compose.yml`](https://github.com/uav4geo/LibreTranslate/blob/main/docker-compose.yml) file to adapt it to your deployment needs, or use an extra `docker-compose.prod.yml` file for your deployment configuration.

## Arguments

| Argument      | Description                    | Default              |
| ------------- | ------------------------------ | -------------------- |
| --host        | Set host to bind the server to | `127.0.0.1`          |
| --port        | Set port to bind the server to | `5000`               |
| --char-limit        | Set character limit | `No limit`               |
| --req-limit        | Set maximum number of requests per minute per client | `No limit`               |
| --batch-limit        | Set maximum number of texts to translate in a batch request | `No limit`               |
| --ga-id        | Enable Google Analytics on the API client page by providing an ID | `No tracking`               |
| --debug      | Enable debug environment | `False`           |
| --ssl        | Whether to enable SSL | `False`               |
| --frontend-language-source | Set frontend default language - source | `en`          |
| --frontend-language-target | Set frontend default language - target | `es`          |
| --frontend-timeout | Set frontend translation timeout | `500`         |
| --api-keys | Enable API keys database for per-user rate limits lookup | `Don't use API keys` |
| --require-api-key-origin | Require use of an API key for programmatic access to the API, unless the request origin matches this domain | `No restrictions on domain origin` |
| --load-only   | Set available languages    | `all from argostranslate`    |

## Manage API Keys

LibreTranslate supports per-user limit quotas, e.g. you can issue API keys to users so that they can enjoy higher requests limits per minute (if you also set `--req-limit`). By default all users are rate-limited based on `--req-limit`, but passing an optional `api_key` parameter to the REST endpoints allows a user to enjoy higher request limits.

To use API keys simply start LibreTranslate with the `--api-keys` option.

### Add New Keys

To issue a new API key with 120 requests per minute limits:

```bash
ltmanage keys add 120
```

### Remove Keys

```bash
ltmanage keys remove <api-key>
```

### View Keys

```bash
ltmanage keys
```

## Language Bindings

You can use the LibreTranslate API using the following bindings:

 - Rust: https://github.com/DefunctLizard/libretranslate-rs
 - Node.js: https://github.com/franciscop/translate
 - .Net: https://github.com/sigaloid/LibreTranslate.Net
 - Go: https://github.com/SnakeSel/libretranslate
 - Python: https://github.com/argosopentech/LibreTranslate-py

More coming soon!

## Discourse Plugin

You can use this [discourse translator plugin](https://github.com/LibreTranslate/discourse-translator) to translate [Discourse](https://discourse.org) topics. To install it simply modify `/var/discourse/containers/app.yml`:

```
## Plugins go here
## see https://meta.discourse.org/t/19157 for details
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
          - git clone https://github.com/discourse/docker_manager.git
          - git clone https://github.com/LibreTranslate/discourse-translator
	  ...
```

Then issue `./launcher rebuild app`. From the Discourse's admin panel then select "LibreTranslate" as a translation provider and set the relevant endpoint configurations.

## Deploy to Google App Engine
### build image
build image for cloud registry since cloud build will timeout; note: for windows wsl2 on ubuntu is required to build the image
docker build . -t gcr.io/vlybytranslate/vlybytranslate

### push image
gcloud docker push gcr.io/vlybytranslate/vlybytranslate
Note:requires authentication - for windows use authentication is required outside of linux wsl 2 box
https://cloud.google.com/container-registry/docs/advanced-authentication#gcloud-helper
sudo usermod -a -G docker ${USER} if you work in linux

### deploy app
gcloud app deploy app.yaml --image-url=eu.gcr.io/vlybytranslate/vlybytranslate

## Mirrors

This is a list of online resources that serve the LibreTranslate API. Some require an API key. If you want to add a new URL, please open a pull request.

URL |API Key Required|Contact|Cost
--- | --- | --- | ---
[libretranslate.com](https://libretranslate.com)|:heavy_check_mark:|[UAV4GEO](https://uav4geo.com/contact)| $9 / month
[libretranslate.de](https://libretranslate.de/)|-|-
[translate.mentality.rip](https://translate.mentality.rip)|-|-
[translate.astian.org](https://translate.astian.org/)|-|-


## Roadmap

Help us by opening a pull request!

- [x] A docker image (thanks [@vemonet](https://github.com/vemonet) !)
- [x] Auto-detect input language (thanks [@vemonet](https://github.com/vemonet) !)
- [X] User authentication / tokens
- [ ] Language bindings for every computer language
- [ ] [Improved translations](https://github.com/argosopentech/argos-parallel-corpus)

## FAQ

### Can I use your API server at libretranslate.com for my application in production?

The API on libretranslate.com should be used for testing, personal or infrequent use. If you're going to run an application in production, please [get in touch](https://uav4geo.com/contact) to get an API key or discuss other options.

### Can I use this behind a reverse proxy, like Apache2?

Yes, here is an example Apache2 config that redirects a subdomain (with HTTPS certificate) to LibreTranslate running on a docker at localhost. 
```
sudo docker run -ti --rm -p 127.0.0.1:5000:5000 libretranslate/libretranslate
```
You can remove `127.0.0.1` on the above command if you want to be able to access it from `domain.tld:5000`, in addition to `subdomain.domain.tld` (this can be helpful to determine if there is an issue with Apache2 or the docker container). 

Add `--restart unless-stopped` if you want this docker to start on boot, unless manually stopped.

<details>
<summary>Apache config</summary>
<br>
	
Replace [YOUR_DOMAIN] with your full domain; for example, `translate.domain.tld` or `libretranslate.domain.tld`. 

Remove `#` on the ErrorLog and CustomLog lines to log requests.

```ApacheConf
#Libretranslate

#Redirect http to https
<VirtualHost *:80>
    ServerName http://[YOUR_DOMAIN]
    Redirect / https://[YOUR_DOMAIN]
    # ErrorLog ${APACHE_LOG_DIR}/error.log
    # CustomLog ${APACHE_LOG_DIR}/tr-access.log combined
 </VirtualHost>

#https
<VirtualHost *:443>
    ServerName https://[YOUR_DOMAIN]
    
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/[YOUR_DOMAIN]/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/[YOUR_DOMAIN]/privkey.pem
    SSLCertificateChainFile /etc/letsencrypt/live/[YOUR_DOMAIN]/fullchain.pem
    
    # ErrorLog ${APACHE_LOG_DIR}/tr-error.log
    # CustomLog ${APACHE_LOG_DIR}/tr-access.log combined
</VirtualHost>
```

Add this to an existing site config, or a new file in `/etc/apache2/sites-available/new-site.conf` and run `sudo a2ensite new-site.conf`. 

To get a HTTPS subdomain certificate, install `certbot` (snap), run `sudo certbot certonly --manual --preferred-challenges dns` and enter your information (with `subdomain.domain.tld` as the domain). Add a DNS TXT record with your domain registrar when asked. This will save your certificate and key to `/etc/letsencrypt/live/{subdomain.domain.tld}/`. Alternatively, comment the SSL lines out if you don't want to use HTTPS.
</details>

## Credits

This work is largely possible thanks to [Argos Translate](https://github.com/argosopentech/argos-translate), which powers the translation engine.

## License

[GNU Affero General Public License v3](https://www.gnu.org/licenses/agpl-3.0.en.html)
