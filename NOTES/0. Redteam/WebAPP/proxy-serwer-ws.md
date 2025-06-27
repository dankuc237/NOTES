- [Flask proxy serwer to websocket endpoint](#flask-proxy-serwer-to-websocket-endpoint)
  - [Pomocne](#pomocne)
  - [Code](#code)
  - [How it works](#how-it-works)

# Flask proxy serwer to websocket endpoint
## Pomocne
`SecLists/Discovery/Web-Content/burp-parameter-names.txt`  
seclista zawierająca parametry do iterowania, np.:
```
...
alt  
alter
alternate
...
```

## Code
```python
from flask import Flask, request
from websocket import create_connection
import json

app = Flask(__name__)

@app.route("/", defaults={"path": ""})
#app.route("/<konwerter, np. int, float, path : nazwa zmiennej>")
@app.route("/<path:path>")
def proxy(path):
    ws_server = "ws://qreader.htb:5789"
    ws_path = "/" + path  # sciezka do websocketa
    data = {k: v for k, v in request.args.items()}  # przepisz parametry
    ws = create_connection(ws_server + ws_path)
    ws.send(json.dumps(data))
    resp = ws.recv()
    ws.close()
    return resp

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8085)
```
## How it works
1. Uruchamia serwer proxy
2. @app.route() do przekierowania requestów HTTP na ścieżkę bazową '/' oraz na dowolną ścieżkę `<path>`. Następnie scieżka path jest używana do zbudowania ścieżki WebSocket '/' + `<path>`.
3. Następnie parametry requestu HTTP są przepisywane do zmiennej data i wysyłane jako JSON do serwera WebSocket. Ostatecznie 
4. Odpowiedź z serwera WebSocket jest zwracana jako odpowiedź na request HTTP.
5. Przykłądowe zapytanie (url) 
   - `http://localhost:8085/?id=1`
   - `http://localhost:8085/kck`
   - `http://localhost:8085/kck?id=1&name=xXx`
