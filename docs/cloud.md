# Cloud Edition

Utilize QBO Kubernetes Engine (QKE) and QBO Container Engine (QCE) to efficiently create, deploy, and oversee resources in **QBO Cloud**. With QBO, experience high-performance computing while retaining cloud flexibility, granting direct access to GPUs, CPUs, and Disk resources. Execute cluster and instance operations swiftly, surpassing the speed of conventional cloud providers. Harness the capabilities of QCE and QKE effortlessly through the web interface, CLI, or QBO AsyncAPI, providing seamless accessibility to manage resources with utmost convenience.

# Console Access
QBO can authenticate access to the API using a web interface or a CLI interface. The web interface uses oauth2 Google authentication and the CLI uses either a temporary universally unique token (oauth2 authentication) or a service account. Both methods as described below

> Note that when you login to the Web console @ https://console.cloud.qbo.io the web console is already configured and there is no further configuration needed for authentication. See [Configuration priority](#configuration-priority) for more info.


> If you are using a Linux, Mac or Windows shell outside QBO's web console you can authenticate in two ways:

## Web Token
> This token is generated upon successful authentication with your Google account


> It can be retrieved by logging in to the web console @ https://console.cloud.qbo.io and getting user's CLI local configuration
```bash
qbo get user -l | jq .cli[]?
```

```json
{
    "qbo_port": 443,
    "qbo_host": "172.17.0.1",
    "qbo_uid": "33820cc1-d513-4fa8-88ac-1adb008c3864"
}
```


> Get `qbo_uid` web token from the output above and then export the environment variable: `QBO_UUID`.
> Note that `QBO_UID` is a temporary token and will expire with the web session expires.
```bash
export QBO_UID=33820cc1-d513-4fa8-88ac-1adb008c3864
```


## Service Account

> QBO service accounts use Elliptical curve cryptography (ECC) P-521 for encryption. A public key in json compact format `qbo_uid` is shared as well as an auxiliary token `qbo_aux` for authentication. 

> Before we can obtain and configure a service account a temporary web token is needed as described in [Temporary web token](#temporary-web-token) to retrieve the service account.

> Verify you can authenticate to the qbo API with the web token by getting qbo API version
```bash
qbo version | jq .version[]?
```
```json
{
    "qbo_cli": "stage-4.3.0-6efb3214c"
}
{
    "docker_api": "1.41",
    "docker_version": "20.10.23",
    "qbo_api": "cloud-dev-4.3.0-6efb3214c",
    "host": "lux.cloud.qbo.io"
}
```


> Download the CLI by cloning the repo @ https://git.eadem.com/alex/qbo-ce.git.
> QBO CLI runs inside a container and can be used in Linux, Mac and Windows OSes.


```bash
git clone https://github.com/alexeadem/qbo-ce.git
cd qbo-ce
```
> Add alias to run by typing `qbo`
```bash
. ./alias


```


> Get the user's configuration and pipe the output to the file `cli.json` under `$HOME/.qbo/`


```bash
qbo get user -c alex@qbo.io | jq .users[]?.cli.conf > ~/.qbo/cli.json
```
> `cli.json` is the file containing the service account used by the CLI to authenticate by the API. 


ee priorities
```bash
cat ~/.qbo/cli.json
```
```json
{
  "qbo_uid": {
    "crv": "P-521",
    "kty": "EC",
    "x": "AaEqw1Hs8N-mpb9wixDZo0Wj-qYm94kqYW7dN2ctZnuVpoASeMIss5RrS5kuXsW1XdGtlCzS5EGbWAaH-rdVTrGH",
    "y": "AQ3jayxn6XQi5yGzgNAQyn5cyihSSHamK9yrJ-VJxqZ6xyV8UceS46qJpy_LFhqTKKBe3HCiadwgnmlnVLg7oV78"
  },
  "qbo_aux": "a9285866-1a0d-4757-b5f8-601b57670738",
  "qbo_port": 443,
  "qbo_host": "nemo.cloud.qbo.io",
  "qbo_user": "alex@qbo.io"
}
```
> Unset the temporary web token to start using the service account.


```bash
unset QBO_UID
```
> Verify the service account by getting the user's local configuration.


```bash
qbo get user -l | jq .cli[]?
```
```json
{
  "qbo_port": 443,
  "qbo_host": "nemo.cloud.qbo.io",
  "qbo_uid": {
    "crv": "P-521",
    "kty": "EC",
    "x": "AaEqw1Hs8N-mpb9wixDZo0Wj-qYm94kqYW7dN2ctZnuVpoASeMIss5RrS5kuXsW1XdGtlCzS5EGbWAaH-rdVTrGH",
    "y": "AQ3jayxn6XQi5yGzgNAQyn5cyihSSHamK9yrJ-VJxqZ6xyV8UceS46qJpy_LFhqTKKBe3HCiadwgnmlnVLg7oV78"
  },
  "qbo_aux": "a9285866-1a0d-4757-b5f8-601b57670738"
}
```

> You should now be able to use the service account to authenticate to the qbo API


```bash
qbo version | jq .version[]?


```
```json
{
  "qbo_cli": "dev-4.3.0-76da24342"
}
{
  "docker_api": "1.41",
  "docker_version": "20.10.23",
  "qbo_api": "cloud-dev-4.3.0-6efb3214c",
  "host": "lux.cloud.qbo.io"
}

```

