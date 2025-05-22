# ASYNC / AWAIT -- l'asynchrone en JS
---

## Pourquoi `async` / `await` ?

En JS, certaines opérations prennent du temps (ex: accès API, lecture de fichier, timer).
On ne veut **pas bloquer** le reste du code.

C’est là qu’intervient le **code asynchrone**, souvent basé sur des **Promises**.

---

## `async` / `await` = syntaxe moderne pour les Promises

* `async` transforme une fonction en **fonction asynchrone** (elle retourne toujours une `Promise`)
* `await` **attend** qu'une `Promise` se résolve avant de continuer

---

## Exemple simple

```js
function attendreDeuxSecondes() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("C'est bon !");
    }, 2000);
  });
}

async function demo() {
  console.log("Attente...");
  const resultat = await attendreDeuxSecondes();
  console.log(resultat);
}

demo();
```

**Sortie :**

```
Attente...
(2 secondes plus tard)
C'est bon !
```

---

## Important : `await` fonctionne **seulement dans une fonction `async`**

```js
// Erreur : 'await' outside of async function
const data = await fetch("https://api.com");
```

Il faut l'encapsuler :

```js
async function loadData() {
  const response = await fetch("https://api.com");
  const data = await response.json();
  console.log(data);
}

loadData();
```

---

## Exemple complet avec `fetch`

```js
async function getUser() {
  const res = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await res.json();
  console.log(user.name);
}

getUser();
```

---

## Plusieurs `await` à la suite

```js
async function getInfos() {
  const userRes = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await userRes.json();

  const postRes = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  const post = await postRes.json();

  console.log(user.name, "a écrit :", post.title);
}

getInfos();
```

---

## Gérer les erreurs avec `try` / `catch`

```js
async function loadUser() {
  try {
    const res = await fetch("https://api.invalide.com/user");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error("Erreur attrapée :", err.message);
  }
}

loadUser();
```

---

## Exécuter en parallèle : `Promise.all`

```js
async function getData() {
  const [user, post] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
    fetch("https://jsonplaceholder.typicode.com/posts/1").then(r => r.json())
  ]);

  console.log(user.name, "a posté :", post.title);
}

getData();
```

---

## Résumé

| Mot-clé     | Rôle                                                     |
| ----------- | -------------------------------------------------------- |
| `async`     | Déclare une fonction qui retourne une Promise            |
| `await`     | Attend le résultat d’une Promise dans une fonction async |
| `try/catch` | Gère les erreurs dans du code `await`                    |

---
