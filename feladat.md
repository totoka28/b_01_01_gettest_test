Oké 💡, akkor vegyük végig **lépésről lépésre** a **gettest** nevű projektet, a nullától addig, hogy a Postmanben látod a JSON adatot.

---

# 1️⃣ Projektmappa létrehozás

```ps
mkdir D:\Dev\gettest
cd D:\Dev\gettest
npm init -y
```

👉 Ez létrehoz egy `package.json` fájlt alapértelmezett beállításokkal.

---

# 2️⃣ Express telepítése

```ps
npm install express
```

👉 Ezzel bekerül a `node_modules` mappába, és a `package.json`-ban megjelenik a `dependencies` között.

---

# 3️⃣ Nodemon (kényelmi eszköz)

Telepítsd fejlesztői csomagként:

```ps
npm install -D nodemon
```

Majd módosítsd a `package.json`-t (scripts rész):

```json
"scripts": {
  "dev": "nodemon src/index.js",
  "start": "node src/index.js"
}
```

---

# 4️⃣ Forrásmappa + JSON adat

Hozd létre a mappát:

```ps
mkdir src
```

Készíts egy fájlt: **`src/data.json`**

```json
[
  { "id": 1, "nev": "Péter", "kor": 17 },
  { "id": 2, "nev": "Anna", "kor": 19 },
  { "id": 3, "nev": "Laci", "kor": 16 }
]
```

---

# 5️⃣ Express szerver

**`src/index.js`**

```js
import express from "express";
import { readFileSync } from "fs";

const app = express();
const PORT = 3000;

// GET végpont: visszaadja a data.json tartalmát
app.get("/adatok", (req, res) => {
  const raw = readFileSync("./src/data.json", "utf-8");
  const json = JSON.parse(raw);
  res.json(json);
});

// Szerver indítása
app.listen(PORT, () => {
  console.log(`Szerver fut: http://localhost:${PORT}`);
});
```

---

# 6️⃣ Szerver indítása

```ps
npm run dev
```

👉 Ekkor a nodemon figyeli a fájlokat, és újraindítja a szervert minden mentésnél.
👉 Ha nodemon nélkül futtatod:

```ps
node src/index.js
```

---

# 7️⃣ Tesztelés Postmanben

1. Nyisd meg a **Postman**-t.
2. Új `GET` kérés:

   ```
   http://localhost:3000/adatok
   ```
3. Küldd el → vissza kell kapnod a `data.json` tartalmát JSON válaszként:

```json
[
  {
    "id": 1,
    "nev": "Péter",
    "kor": 17
  },
  {
    "id": 2,
    "nev": "Anna",
    "kor": 19
  },
  {
    "id": 3,
    "nev": "Laci",
    "kor": 16
  }
]
```

---

✅ Ezzel a **gettest** projekt teljesen működőképes:

* Node.js futtatja
* Express kezeli a kérést
* JSON fájlból olvas adatot
* Postmanben lekérdezhető

---

Akarod, hogy hozzátegyem **ugyanezt XAMPP MySQL-lel** is (ugyanez a logika, csak JSON fájl helyett adatbázisból jön az adat)?
