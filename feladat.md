# Projekt: **gettest**


## 2) Csomagok telepítése

```ps
npm i express
npm i -D nodemon
```



* **express** → a Node.js keretrendszer, kell a szerverhez
* **nodemon** → figyeli a fájlokat és automatikusan újraindítja a szervert

---

## 3) Fájlstruktúra

- Hozd létre az alábbi filokat és mappákat:

```
repomappa/
 ├─ src/
 │   ├─ data.json
 │   └─ index.js
```

---

## 4) JSON adat (példa)

`src/data.json`

```json
[
  { "id": 1, "nev": "Péter", "kor": 17 },
  { "id": 2, "nev": "Anna", "kor": 19 },
  { "id": 3, "nev": "Laci", "kor": 16 }
]
```

---

## 5) Express szerver

`src/index.js`

```js
const express = require("express");
const { readFileSync } = require("fs");

const app = express();
const PORT = 3000;

app.get("/adatok", (req, res) => {
  const raw = readFileSync("./src/data.json", "utf-8");
  const json = JSON.parse(raw);
  res.json(json);
});

app.listen(PORT, () => {
  console.log(`Szerver fut: http://localhost:${PORT}`);
});
```



* `fs.readFileSync` → beolvassa a `data.json` tartalmát
* `JSON.parse` → objektummá alakítja
* `res.json` → JSON formátumban visszaküldi a kliensnek

---

## 6) package.json – scripts

Nyisd meg a `package.json`-t, és egészítsd ki:

```json
{
  "name": "gettest",
  "version": "1.0.0",
  "description": "Egyszerű Express projekt, ami egy JSON fájlt ad vissza",
  "main": "src/index.js",
  "type": "module",
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "node src/index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^5.1.0"
  },
  "devDependencies": {
    "nodemon": "^3.1.10"
  }
}
```



* `npm run dev` → fejlesztői mód (nodemon figyel)
* `npm start` → sima futtatás node-dal

---

## 7) Indítás

```ps
npm run dev
```

Konzolban:

```
[nodemon] starting `node src/index.js`
Szerver fut: http://localhost:3000
```

Böngésző/Postman:
`http://localhost:3000/adatok` → visszaadja a JSON-t.

---

# 🔎 Postman tesztelés

1. Új **GET** kérés:

   ```
   http://localhost:3000/adatok
   ```

2. **Send** → látod a JSON adatokat.

3. Menj a **Tests** fülre, és írd be:

```js
pm.test("Státuszkód 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Válasz JSON", function () {
    pm.response.to.be.withBody;
    pm.response.to.be.json;
});

pm.test("Tartalmaz id mezőt", function () {
    const body = pm.response.json();
    pm.expect(body[0]).to.have.property("id");
});
```


 Ez 3 dolgot ellenőriz:

* a válasz **200 OK**
* a válasz **JSON formátumú**
* az első elemnek van **id** mezője

Ha minden oké, zöld pipákat látsz. ✅


