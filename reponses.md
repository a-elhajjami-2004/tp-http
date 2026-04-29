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

```js
async function fetchWithRetries(url, options, maxRetries) {
	let response = await fetch(url, options);
	for (let retry = 0; retry < maxRetries && response.status >= 500; retry++) {
		await new Promise((resolve) => setTimeout(resolve, 1000));
		response = await fetch(url, options);
	}
	if (!response.ok && response.status >= 500) {
		throw new Error(`Nombre de tentatives dépassé. Status ${response.status}`);
	}
	return response;
}
```

## TP 4 : Analyse des Headers de Sécurité

| Site                                                     | `Strict-Transport-Policy`                      | `X-Frame-Options` | `Content-Security-Policy`                                                                                                                                           | Note |
| -------------------------------------------------------- | ---------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| [github.com](https://github.com)                         | `max-age=31536000; includeSubdomains; preload` | `deny`            | `default-src 'none'; base-uri 'self'; child-src github.githubassets.com github.com/assets-cdn/worker/ github.com/assets/ gist.github.com/assets-cdn/worker/; [...]` | A    |
| [google.com](https://google.com)                         | `max-age=31536000`                             | `SAMEORIGIN`      | Pas présent                                                                                                                                                         | C    |
| [store.steampowered.com](https://store.steampowered.com) | `max-age=63072000`                             | `DENY`            | `default-src blob: data: https: 'unsafe-inline' 'unsafe-eval'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://store.fastly.steamstatic.com/ [...]`         | B    |

## Exercice Récapitualtifs

### Client HTTP minimaliste

Voir [le TP précédent](https://github.com/a-elhajjami-2004/tp-http-requests)

### Questions théoriques

1. La directive `no-cache` indique que la réponse peut être mise en cache, mais doit être validée avec le serveur
   d'origine à chaque réutilisation.

    La directive `no-store` indique que la réponse ne doit pas être mise en cache.

1. La méthode POST n'est pas idempotente car si elle est appelée plusieurs fois, une ressource peut être créée
   plusieurs fois. Donc si elle est appelée plus d'une fois, elle peut avoir un effet différent sur le serveur
   chaque fois.

1. Lorsque le serveur renvoie un code 301, le client sera redirégé au chemin donné dans le header `Location`.

1. Le header `Origin` indique l'origine de la requête. Il n'est pas ajouté au requêtes GET/HEAD vers le même hôte que
   l'origine (same-origin). Il est toujours ajouté au requête vers un hôte différent de l'origine (cross-origin).

1. L'attribut `HttpOnly` des cookies interdit d'accéder à ce cookie depuis JavaScript. Ceci protège contre les attaques
   XSS.
