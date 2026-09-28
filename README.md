# To Do Aplikacija
Jednostavna To Do aplikacija napravljena u **Vue 3** (Options API) uz **Vue CLI 5** i webpack 5.
Zadaci se mogu dodavati, označavati kao završeni i brisati, a lista se može filtrirati
po **statusu** i po **prioritetu**.

## Funkcionalnosti
- Dodavanje zadataka sa izborom prioriteta (Visoko / Srednje / Nisko)
- Označavanje zadataka kao završenih (i poništavanje)
- Brisanje zadataka
- Filteri po statusu (Svi / Aktivno / Završeno) i po prioritetu
- Statistika: ukupno, završeno, aktivno
- Prazno stanje kada nema zadataka koji odgovaraju filterima
## Struktura projekta
```
├── public/
│   ├── index.html        # HTML šablon (build ga obrađuje i ubacuje bundle fajlove)
│   └── favicon.ico       # kopira se nepromenjen u dist/
├── src/
│   ├── main.js           # ulazna tačka, montira App u #app
│   ├── App.vue           # glavna komponenta (stanje, filteri, logika zadataka)
│   └── components/
│       └── TaskItem.vue  # prikaz jednog zadatka (props + emitovani događaji)
├── jsconfig.json         # editor/IntelliSense podešavanja i @/* alias
├── vue.config.js         # Vue CLI konfiguracija (publicPath, naslov, source map)
└── babel.config.js       # Babel preset za Vue CLI
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

## Napomene
- **`publicPath: './'` u `vue.config.js`** — u produkciji se generišu relativne putanje
  (`./js/...`), tako da `dist/index.html` može da se otvori i direktno sa diska (`file://`),
  bez servera. Pažnja: ovo nije kompatibilno sa `vue-router` u `history` modu — ako se
  ikada doda rutiranje, treba koristiti `hash` mod ili apsolutni `publicPath`.
- **`public/` vs `dist/`** — `public/` je izvor statičkih fajlova koji se kopiraju u build izlaz.
  `dist/` se generiše pri svakom buildu, nije u git-u i ne treba ga ručno menjati.
  `public/index.html` je šablon koji build obrađuje i u koji ubacuje bundle fajlove.
- **`@/` alias** — pokazuje na `src/`, definisan u `jsconfig.json`.

## Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
