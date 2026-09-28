# To Do Aplikacija

Jednostavna To Do aplikacija napravljena u **Vue 3** (Options API) uz **Vite**.

Omogućava dodavanje zadataka, izbor prioriteta, označavanje zadataka kao završenih, brisanje i filtriranje liste po statusu i prioritetu.

## Funkcionalnosti

- Dodavanje zadataka sa izborom prioriteta:
  - Visoko
  - Srednje
  - Nisko
- Označavanje zadataka kao završenih
- Poništavanje statusa završenog zadatka
- Brisanje zadataka
- Filteri po statusu:
  - Svi
  - Aktivno
  - Završeno
- Filteri po prioritetu
- Statistika:
  - Ukupno zadataka
  - Završeno
  - Aktivno
- Prazno stanje kada nema zadataka koji odgovaraju izabranim filterima
- Responzivan interfejs

## Tehnologije

- Vue 3
- Vite
- JavaScript
- HTML
- CSS

## Struktura projekta

```text
├── public/
│   └── favicon.ico
├── src/
│   ├── main.js              # Ulazna tačka aplikacije
│   ├── App.vue              # Glavna komponenta, stanje i logika aplikacije
│   └── components/
│       └── TaskItem.vue     # Prikaz pojedinačnog zadatka
├── .gitignore
├── index.html               # Glavni HTML dokument
├── jsconfig.json            # JavaScript editor/IntelliSense podešavanja
├── package.json             # Zavisnosti i npm skripte
├── package-lock.json
├── vite.config.js           # Vite konfiguracija
└── README.md
