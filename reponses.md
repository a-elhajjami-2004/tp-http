# Protocole HTTP - Travaux Pratiques

## TP 1 : Exploration avec les DevTools

### Observer une requête simple

- Le code de status est `200 OK`.
- ```http
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
  Accept-Encoding: gzip, deflate, br, zstd
  Accept-Language: en,en-US;q=0.9,fr;q=0.8,ar;q=0.7
  Cache-Control: no-cache
  Dnt: 1
  Pragma: no-cache
  Priority: u=0, i
  Sec-Ch-Ua: "Google Chrome";v="147", "Not.A/Brand";v="8", "Chromium";v="147"
  Sec-Ch-Ua-Mobile: ?0
  Sec-Ch-Ua-Platform: "Windows"
  Sec-Fetch-Dest: document
  Sec-Fetch-Mode: navigate
  Sec-Fetch-Site: none
  Sec-Fetch-User: ?1
  Upgrade-Insecure-Requests: 1
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.0.0 Safari/537.3
  ```
- Le `Content-Type` de la réponse est `application/json`.

### Observer les code de status

| URL                                                      | Méthode | Code        | `Content-Type`             |
| -------------------------------------------------------- | ------- | ----------- | -------------------------- |
| [httpbin.org/get](https://httpbin.org/get)               | GET     | 200 OK      | `application/json`         |
| [httpbin.org/post](https://httpbin.org/post)             | POST    | 200 OK      | `application/json`         |
| [httpbin.org/status/201](https://httpbin.org/status/201) | GET     | 201 Created | `text/html; charset=utf-8` |

## TP 2 : Maîtrise de cURL

### Requête GET simple

La différence entre les option `-i` et `-v` est :

- `-i` affiche le code de status, les headers de la réponse, et son corps ;
- `-v` affiche les informations de debug, telles que les addresses IP de l'hôte, la version de HTTP que cURL propose et
  que l'hôte accepte, et les headers de la requête, ainsi que les informations affichées par `-i`.

### Exercice avancé

```bash
curl \
  -H "Content-Type: application/json" \
  -H "X-Custom-Header: MonHeader" \
  -d '{"action": "test", "value": 42}' \
  https://httpbin.org/post
```

## TP 3 : API REST avec JavaScript

### Exercice pratique

<!--  -->

```js
function fetchWithRetries(url, options, maxRetries) {
	/*
	Effectue une requête HTTP
	En cas d'erreur 5xx, réessaie jusqu'à maxRetries fois
	Attend 1 seconde entre chaque tentative
	Retourne la réponse ou lève une erreur
	*/
}
```

## TP 4 : Analyse des Headers de Sécurité

| Site                                                     | `Strict-Transport-Policy`                      | `X-Frame-Options` | `Content-Security-Policy`                                                                                                                                           | Note |
| -------------------------------------------------------- | ---------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| [github.com](https://github.com)                         | `max-age=31536000; includeSubdomains; preload` | `deny`            | `default-src 'none'; base-uri 'self'; child-src github.githubassets.com github.com/assets-cdn/worker/ github.com/assets/ gist.github.com/assets-cdn/worker/; [...]` | A    |
| [google.com](https://google.com)                         | `max-age=31536000`                             | `SAMEORIGIN`      | Pas présent                                                                                                                                                         | C    |
| [store.steampowered.com](https://store.steampowered.com) | `max-age=63072000`                             | `DENY`            | `default-src blob: data: https: 'unsafe-inline' 'unsafe-eval'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://store.fastly.steamstatic.com/ [...]`         | B    |
