- you can use webhooks to exfiltrate data (e.g. can be used when working with a web app that has a **blind [server-side template injection](https://portswigger.net/web-security/server-side-template-injection)**)
- [webhook.site](https://webhooks.site)
- example of a Webhook in Flask:

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/hook", methods=["POST"])
def hook():
    data = request.get_json()
    print(data)
    return "<h3>hook</h3>"

app.run(host="127.0.0.1", port="8000")
#NOTE: Content-Type here is: "application/json", doesn't work for "application/x-www-form-urlencoded" data
```