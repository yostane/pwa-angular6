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

![angular logo](assets/ng-add-pwa.png 'angular logo')

## Filling the manifest

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

## Conclusion
