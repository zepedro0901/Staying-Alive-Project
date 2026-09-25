# Staying Alive

A symptom-checker single-page app built in 24 hours at a hackathon. Describe how you feel, and it suggests related symptoms, a likely condition and the kind of doctor to see.

> **Not medical advice.** This is a hackathon project built on a sandbox API. Always consult a healthcare professional.

## About the project

This was my final project at the [Code for All_](https://codeforall.com/) bootcamp: a **24-hour hackathon** in teams of four to five, where every project was themed around a song from the '70s or '80s. Our song was *Stayin' Alive* by the Bee Gees, so we built an app to help keep you alive.

It runs on the [ApiMedic Symptom Checker API](https://apimedic.com/).

## How it works

**1. Start.** You're greeted with the home screen.

![Home screen](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/d90190bf-b3fc-43d7-94c7-c46f737b91df)

**2. Your details.** Enter your age and gender; they matter for the diagnosis.

![Details form](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/a94f6612-aeb2-483d-bbdb-ce5b1335403d)

**3. Your symptoms.** Start typing, and the search bar suggests matching symptoms as you type. Each symptom you pick is added to your list.

![Symptom search](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/58219c03-b624-490d-88ef-08a3523f16c8)
![Selected symptoms](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/75c1a61b-ec57-48b0-ada6-ce312da18b55)

**4. Related symptoms.** Based on what you've selected, the app suggests other symptoms that often appear alongside them.

![Related symptoms](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/256c1c83-9de7-4103-a3ff-d880955ccdef)

**5. Diagnosis.** Click **Get Diagnose** to see the most likely condition, its medical (ICD) name, how accurate the match is, and which specialist to visit.

![Diagnosis](https://github.com/zepedro0901/Staying-Alive-Project/assets/116742735/0574c353-df59-4390-8fbf-869148598680)

Click the **Staying Alive** logo at any time to start over.

## Getting started

### 1. Get an API token

The app needs a token for the ApiMedic sandbox:

1. Create a free sandbox account at [apimedic.com](https://apimedic.com/).
2. Request a token with your sandbox API credentials, following ApiMedic's authentication documentation.
3. Paste it into the `token` variable at the top of `js/app/services/symptoms-service.js`.

Sandbox tokens expire after two hours, so you'll need a fresh one for each session.

### 2. Run it locally

The app uses JavaScript modules, which browsers won't load straight from the file system, so it needs a local web server. From the `Staying Alive Project` folder:

```bash
python3 -m http.server 8000
```

or

```bash
npx serve .
```

Then open [http://localhost:8000](http://localhost:8000) (or the address `serve` prints).

## Under the hood

The app is written in plain JavaScript with no framework, using a small **model-view-controller** structure:

```
js/app/
├── main.js                  Entry point: starts the router
├── router.js                Hash-based router (#home, #form, #symptoms, ...)
├── routes.js                Maps each route to its controller and view
├── controllers/             Wires user actions to the service and views
├── views/                   Builds and updates the page (jQuery)
└── services/
    └── symptoms-service.js  Talks to the ApiMedic REST API
```

- **Routing.** A listener on the browser's `hashchange` event picks the route and loads its controller on demand with dynamic `import()`, so each screen's code is fetched only when it's needed.
- **Search as you type.** Each keystroke fetches the symptom list and filters it on the client, showing the top five matches.
- **REST API calls.** Three ApiMedic endpoints do the work: symptom search, related ("proposed") symptoms, and diagnosis. They're all called with `async`/`await` and `fetch`.

## What I'd improve today

It was built in 24 hours, so there's plenty to tidy up:

- **Use the details the user enters.** The diagnosis request currently sends fixed demo values for age and gender rather than the ones from the form.
- **Keep API credentials out of the browser.** A small backend should handle authentication with ApiMedic and refresh the token automatically, instead of it being set in client-side code.
- **Show errors to the user.** When a request fails, the error only appears in the browser console, and the screen gives no feedback.
- **Remove leftover template code.** There are unused controllers and routes from the starter template the project was built on.

## Built with

- JavaScript (ES modules)
- jQuery
- Bootstrap
- [ApiMedic Symptom Checker API](https://apimedic.com/)
