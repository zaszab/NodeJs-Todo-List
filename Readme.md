## Todo List.

Build a client-only todo list with VueJs & Google Cloud Firestore and deploy it with Firebase Hosting and Cloud Functions.


**Prerequisites**

1. Clone the repo.

2. Create and login to your Firebase account:
https://console.firebase.google.com/

3. Create a new project

4. On the welcome page click on the "Add Firebase to your web app" button. Copy the snippet to the public/index.html.

5. On Firebase console go to the Database menu and click Get Started button under Cloud Firestore Beta. Then choose the "Start in test mode" and Enable.

6. You have to Firebase CLI installed on your local computer. Install it in console:
```
npm i -g firebase-tools
```

7. Run this command in the working dir:
```
firebase use --add
```

Choose your new project from the list and add "default" az alias.

**Install**

```

npm install

```

**Build**

```

npm run build

```

**Run**

For local development and testing:
```

firebase serve --only hosting

```

Navigate to http://localhost:5000

For deploy:
```

firebase deploy --only hosting

```

If you change anything in your public/ directory, you have to rebuild it before running again.
