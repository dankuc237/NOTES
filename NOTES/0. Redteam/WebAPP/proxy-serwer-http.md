```python
from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>', methods=['GET', 'POST'])
def proxy(path):
    # Utwórz adres URL docelowego serwera
    target_url = 'http://target:1234/' + path
    # Przekopiuj parametry z oryginalnego żądania
    params = {k: v for k, v in request.args.items()}
    # Ustaw nagłówek Content-Type na wartość oryginalną lub domyślną
    content_type = request.headers.get('Content-Type', 'text/plain')
    # Wykonaj żądanie do docelowego serwera
    response = requests.request(
        method=request.method,
        url=target_url,
        headers={key: value for (key, value) in request.headers if key != 'Host'},
        params=params,
        data=request.get_data(),
        cookies=request.cookies,
        allow_redirects=False,
    )
    # Przekopiuj nagłówki z odpowiedzi docelowego serwera
    headers = [(key, value) for (key, value) in response.raw.headers.items()]
    # Zwróć odpowiedź z docelowego serwera
    return response.content, response.status_code, headers

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)

```