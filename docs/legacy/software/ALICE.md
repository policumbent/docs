# Alice

## Introduzione

Alice<sup class="note">[1](#note1)</sup> è la dashboard [SPA] per la telemetria della bici, usata sia nei test in pista che durante la competizione a Battle Montain.  
All'indirizzo [alice.policumbent.it](https://alice.policumbent.it) è accessibile liberamente anche da chi ci segue sui social.

## Frontend

Usa [preact] con compatibilità automatica verso React, quindi per lo sviluppo bisogna far riferimento alla documentazione [preact][p_doc] o a quella ufficiale [React][r_doc].

Il progetto è visibile su [github] ed ha la seguente struttura:

```treeview
.
├── ...
├── public
│   ├── ...
│   ├── index.html
│   └── manifest.json
└── src
    ├── ...
    ├── api.ts
    ├── App.tsx
    ├── App.scss
    ├── App.test.js
    ├── assets
    ├── components
    │   ├── notifications.tsx
    │   ├── footer.tsx
    │   └── utils.ts
    ├── containers
    │   └── DefaultLayout
    ├── index.tsx
    ├── routes.js
    ├── service-worker.js
    ├── serviceWorkerRegister.js
    └── views
        ├── Credits
        ├── Dashboard
        ├── Login
        ├── Logout
        └── Pages
```

- in `api.ts` sono dichiarate le chiamate alle api del [server](#backend) per le varie configurazioni
- `App.jsx` è il component principale di default che viene renderizzato dall'`index.jsx`
- in `App.scss` contiene tutto il css custom che non è facilmente implementabile con [reactstrap]
- in `components` risiedono tutti i component che implementano le funzionalità principali
- in `utils.ts` ci sono le funzioni base, comuni a tutti i component
- `DefaultLayout` è il component di root della vista che instanzia la <u>topbar</u> e il <u>footer</u>
- `routes.js` definisce le route dell'applicazione, ogni vista è renderizzata come una diversa route
- in `views` sono definite le viste, divise in diversi package javascript

Le funzionalità [PWA] di React sono implementate in `service-worker.js` e `serviceWorkerRegister.js` e la configurazione in `manifest.json`.

<!-- Link markdown -->

[spa]: https://it.wikipedia.org/wiki/Single-page_application
[preact]: https://preactjs.com/
[p_doc]: https://preactjs.com/guide/v10/getting-started
[r_doc]: https://it.reactjs.org/docs/getting-started.html
[github]: https://github.com/policumbent/alice
[reactstrap]: https://reactstrap.github.io/
[pwa]: https://it.wikipedia.org/wiki/Progressive_Web_App

## Backend

<!-- TODO: backend doc -->

Il backend si trova su [github][b].

<!-- Link markdown -->

[b]: https://github.com/policumbent/SyncServer3

---

<!-- Note piè di pagina -->

1. <a id="note1"></a> A volte anche ALICE
