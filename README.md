# Turning an Angluar 6 app into a Progressive Web App

This article shows how to add PWA (Progressive Web App) capabilities to your Angular 6 app.

![PWA logo](assets/pwa_diekus.svg 'pwa logo')
![angular logo](assets/angular.svg 'angular logo')

We will go through the following steps:

1.  Adding PWA capabilities to an Angular app
2.  Filling the manifest
3.  Configuring the service worker
4.  Verifying and testing

In the following, we will be using Angular 6 and we support that we already have an Angular project.

We will start right away by adding PWA capabilities.

## Adding PWA capabilities to an Angular app

Angular CLI 6 allows to add PWA capabilities to an existing PWA app. To do that, simply open a terminal on the root of the project and type the following command.

```shell
ng add @angular/pwa
```

You should see an output similar to this:

```shell
-> % ng add @angular/pwa
Installing packages for tooling via npm.
+ @angular/pwa@0.6.8
added 2 packages from 2 contributors and audited 26689 packages in 23.992s
CREATE ngsw-config.json (392 bytes)
CREATE src/assets/icons/icon-128x128.png (1253 bytes)
CREATE src/assets/icons/icon-144x144.png (1394 bytes)
CREATE src/assets/icons/icon-152x152.png (1427 bytes)
CREATE src/assets/icons/icon-192x192.png (1790 bytes)
CREATE src/assets/icons/icon-384x384.png (3557 bytes)
CREATE src/assets/icons/icon-512x512.png (5008 bytes)
CREATE src/assets/icons/icon-72x72.png (792 bytes)
CREATE src/assets/icons/icon-96x96.png (958 bytes)
CREATE src/manifest.json (1087 bytes)
UPDATE angular.json (3736 bytes)
UPDATE package.json (1390 bytes)
UPDATE src/app/app.module.ts (526 bytes)
UPDATE src/index.html (391 bytes)
```

![angular add pwa](assets/ng-add-pwa.png 'angular add pwa')

We can see that the command created a bunch of files and updated some. Here I'll try to quickly explain what happended.

- **ngsw-config.json**: is a configuration file for the service worker. This file will be used by ng-cli when it needs to create the service worker file. It is mainly used to configure the different caching strategies.
- **icon-xxx.png**: the app icon in different resolutions. These icons are used by the PWA manifest. The icons are used for example as the app icon and inside the splashscreen.
- **manifest.json**: the PWA manifest. It provides metadata about the web app. It contains for example the icon and the name of the app.
- The updated files allow to take into account the service worker and the manifest in the Angluar app. If we open **index.html**, we can note that it references the manifest in its head tag. These updates are necessary to consider the app as a PWA by a browser. The PWA features won't be enabled if we omit to reference them in the web app Even if the service worker and manifest files exist.

The angular-cli already filled the manifest and the service worker configuration with some data. It also created assets with the Angular logo. The next step is thus to replace the logo with our own and also update the manifest and the service worker configuration.

In the next step, we will update the manifest.

## Filling the manifest

The manifest file contains some metadata used when installing the PWA and when it needs to display a splash screen. In fact, a PWA that is launched from the home-screen will display a splash screen.

Here is the description of some fields. [A more exhaustive list can be found here](https://pwa-workshop.js.org/1-manifest/):

- `name` - displayed on the splash-screen below the app icon
- `short_name` - displayed below the shortcut on the desktop or on the home screen
- `description` - a general description of the application
- `background_color` - the background color of the splash-screen
- `theme_color` - the general theme color of the application, used in the status bars for example if they are displayed
- `display` - specifies the display mode. `standalone`: look and feel like a standalone application. This means that the application will have its own window, its own icon in the launcher, and so on. In this mode, the user agent will exclude UI elements for controlling navigation, but can include other UI elements such as a status bar.
- `icons` - list of application icons of different resolutions, used for shortcut and splashscreen. The recommended sizes to be supplied are at least 192x192px and 512x512px. The device will automatically pick the best icon depending on the case. It is also interesting to provide a SVG vector version of the icon that will fit a maximum of sizes.

```javascript
{
  "name": "angular-pwa-app",
  "short_name": "angular-pwa-app",
  "theme_color": "#1976d2",
  "background_color": "#fafafa",
  "display": "standalone",
  "scope": "/",
  "start_url": "/",
  "icons": [
    {
      "src": "assets/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

After updating the manifest and the app icon, we can take care of the service worker configuration.

## Configuring the service worker

```json
{
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "app",
      "installMode": "prefetch",
      "resources": {
        "files": ["/favicon.ico", "/index.html", "/*.css", "/*.js"]
      }
    },
    {
      "name": "assets",
      "installMode": "lazy",
      "updateMode": "prefetch",
      "resources": {
        "files": ["/assets/**"]
      }
    }
  ]
}
```

## Testing

Since the service worker performs caching, PWA features are not enabled during development. Thus, `ng serve` will skip the service worker but will add the manifest. All PWA features are enabled in **production** mode. So, we will need to either run `ng serve --prod` or `ng build --prod` to enable the service worker. The second command requires running a local server to test like [http-server](https://github.com/indexzero/http-server) or [http-server-pwa](https://www.npmjs.com/package/http-server-pwa). I remembered having problems opening the wep app from Android using [serve](https://www.npmjs.com/package/serve). I do not advise using it.

In order to test a PWA in the best conditions, the web app needs to run at least on a static HTTPS server with a valid certificate. If we do not satisfy some of these conditions, we may have partial PWA support. For example, we may have a service worker running but the install will be a simple shortcut and the app won't be added to the app drawer on Android.

Surprisingly, achieving the perfect conditions is more complicated locally than on a free web host, particularly because of HTTPS. There are tutorials out there that show how to efficiently test a PWA locally like [this one](https://www.remotesynthesis.com/blog/running-ssl-localhost), [this other one](https://deanhume.com/testing-service-workers-locally-with-self-signed-certificates/) and [maybe this one](https://www.npmjs.com/package/http-server-pwa). For my part, I used a [free web host](https://www.alwaysdata.com).

Hopefully many things can be tested locally without much effort using **Applications tab** on **Chrome dev tools**:

- Checking the manifest
- Debugging the service worker
- Manipulating the cache

You'll need and Android device to test the **Add to home-screen** and **Splash screen features**. I noted that on HTTPS, choosing **Add to home-screen** will add the app to the app drawer while on http it will be a shortcut on the home-screen.

My advice is to first test on http locally and then arrange to also on https.

Now's the time to conclude.

## Conclusion

In this article, we have seen how to add PWA features to an Angluar app. thanks to ng-cli support, it was very easy to add a manifest, a service worker and sample app icons. Next, we have updated the manifest and configured the service worker.

Finally, we explained that testing PWA features in the best conditions is somewhat complicated but once we get the hang of, it becomes not so much difficult.

The final note is that there is no reason to not add PWA to an Angular app.

Happy coding :)
