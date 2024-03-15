# README

Das kleine Progrämmchen nimmt die Daten von einer HTTP-Schnittstelle aus und
schickt sie mithilfe von der HTTP-Method "POST" zu Opensensemap.

## Socat

socat TCP-LISTEN:3000,fork TCP:10.1.106.36:80

sudo tailscale funnel --bg 3000

## Webhook

```shell
sudo apt install webhook
```

Create a secret for `/etc/webhook.conf`.

```python
import secrets

# Generate a random string with a length of 32 characters
secret = secrets.token_hex(32)
print(secret)
```

Edit `/etc/webhook.conf`

```json
[
  {
    "id": "redeploy",
    "execute-command": "/path/to/redeploy.sh",
    "command-working-directory": "/path/to",
    "trigger-rule":
    {
      "match":
      {
        "type": "payload-hash-sha1",
        "secret": "<your secret goes here>",
        "parameter":
        {
          "source": "header",
          "name": "X-Hub-Signature"
        }
      }
    },
  }]
```

Test

```shell
webhook -hooks /etc/webhook.conf -verbose
```

### Configuring tailscale funnel

```shell
sudo tailscale funnel --bg --set-path webhook 9000
```

Test tailcale funnel

```shell
sudo tailscale serve status
```

### Configuring Github

Open github repo and go to `Settings->Webhooks->Add webhook`.

Specify the payload url to be `https://what.ever/webhook/hooks/redeploy` and set
the secret generated for the `webhook.conf`.
