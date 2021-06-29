# Testing Application for Push Notifications

## Requirements

- NodeJS
- NPM
- Android SDK

### 1.a. Docker Node Environment

If you have node environment on you local machine, you can skip this part.
Otherwise, you can utilize docker by running this command first:
```bash
docker run -it --rm -v $PWD:/app -w /app -p 5000:5000 -p 35729:35729 node:latest bash
```

### 1.b. Create Svelte Project
And then start creating svelte project.
```bash
npx degit sveltejs/template pushnotif
cd pushnotif
npm install
npm run dev
```

To be able to test it on browser, open `package.json`, locate the `scripts` > `start` and add `--host`. It should look like this:
```json
{
    ...
    "scripts": {
        ...
        "start": "sirv public --no-clear --host"
    },
    ...
}
```

Now if you change anything inside `src/App.svelte` the browser will automatically refresh itself.

### 2. Create Firebase Project

TODO, to create `google-services.json`

### 3. Adding capacitor

Run the following command to add CapacitorJS:

```bash
npm install @capacitor/core @capacitor/cli @capacitor/android @capacitor/push-notifications
npx cap init
```

When asked about project name, you can put `PushNotf Test` or whatever you prefer. For Package ID, make sure it is the same as what you use in Firebase project. You can say no to the rest.

Next, add this part into `capacitor.config.js`:
```json
{
    ...
    "plugins": {
        "PushNotifications": {
            "presentationOptions": ["badge", "sound", "alert"]
        }
    }
}
```

Then add android platform:
```bash
npx cap add android
```

Copy the `google-services.json` into `android/` folder.


### 4. Adjust The Web

Make changes to the `src/App.svelte` to test push notif functionality. You can copy the content of `App.example.svelte` that comes with this `Readme.md`

After making changes to the web, run this command:
```bash
npx cap sync
```

### 5.a. Android Environment

If you have Android environment on your local, you can skip this part.

If you want to install Android environment on your local, check this [guide](https://capacitorjs.com/docs/getting-started/environment-setup#android-development).

Otherwise, you can utilize docker by running this command from `pushnotif` directory:

```bash
mkdir -p .cache/build-tools
mkdir -p .cache/gradle
docker run -it --rm -v $PWD:/app -w /app -v $PWD/.cache/build-tools:/opt/sdk/build-tools -v $PWD/.cache/gradle:/root/.gradle alvrme/alpine-android:android-30-jdk11 sh
```

Note that there's still room for improvement for using docker to avoid downloading required packages again and again.

### 5.b. Build APK

```bash
cd android
./gradlew assembleRelease
```

The process will take a while as it downloading and installing some dependencies.
When it finished, you can find the apk at `android/app/build/outputs/apk`.


And that's it. Enjoy! ^_^