# Ionic Tabs Starter for Stencil

We will be implementing the stencil component starter with an added drop down component from James Griffiths.

The test component library: https://www.npmjs.com/package/ulicina

The [Stencil docs on framework integration](https://stenciljs.com/docs/framework-integration/) in step four for Angular state:
*Copy the component collection(s) during the build*

Had to create my own .angular-cli.json file to add this to:
```
  "assets": [
  "assets",
  "favicon.ico",
  { "glob": "**/*", "input": "../node_modules/ulicina/dist/mycomponent", "output": "./mycomponent" }
  ],
```

Then import it in app.module.ts:
```
import '../../node_modules/ulicina/dist/mycomponent';
```

But this does nothing:
```
<p>Here it is --><my-dropdown name="Stencil key features"></my-dropdown><--</p>
```

There is a failed call in the network tab:
```
http://localhost:8100/build/mycomponent/mycomponent.6q0htit6.js
Not Found
Code 404
```

Indeed, it's not there.  But it is in the node modules.  That should then go into the vendor.js, right?
Reed Richards added to the [Forum post](https://forum.ionicframework.com/t/example-integration-stenciljs-web-component-in-ionic-app/106083/4) on this subject.  He has overwritten copy.config.js from app-scripts and modified the copySwToolbox like following
```
copySwToolbox: {
  src: ['{{ROOT}}/node_modules/sw-toolbox/sw-toolbox.js', '{{ROOT}}/node_modules/my-component/mycomponent**/*'],
  dest: '{{BUILD}}'
}
```

Might have to try the component wrapper a la the Adrián Brito Pacheco ionic-test-stencil-web-component project.
Here's [the integration commit from the Ionic side of his project](https://github.com/abritopach/ionic-test-stencil-web-component/commit/384e4ed75e4a2701f8d1d1742e1c81d4079243a3).



## In the beginning

The project was begun in this manner:
```
$ ionic start ionic-tabs tabs
✔ Creating directory ./ionic-tabs - done!
✔ Downloading and extracting tabs starter - done!
? Would you like to integrate your new app with Cordova to target native iOS and Android? Yes
✔ Personalizing ionic.config.json and package.json - done!
> ionic integrations enable cordova --quiet
✔ Downloading integration cordova - done!
✔ Copying integrations files to project - done!
[OK] Added cordova integration!
Installing dependencies may take several minutes.
  ✨   IONIC  DEVAPP  ✨
 Speed up development with the Ionic DevApp, our fast, on-device testing mobile app
  -  🔑   Test on iOS and Android without Native SDKs
  -  🚀   LiveReload for instant style and JS updates
 ️-->    Install DevApp: https://bit.ly/ionic-dev-app    <--
> npm i
✔ Running command - done!
> git init
  🔥   IONIC  PRO  🔥
 Supercharge your Ionic development with the Ionic Pro SDK
  -  ⚠️   Track runtime errors in real-time, back to your original TypeScript
  -  📲   Push remote updates and skip the app store queue
Learn more about Ionic Pro: https://ionicframework.com/products
? Install the free Ionic Pro SDK and connect your app? Yes
-----------------------------------
> npm i --save -E @ionic/pro
✔ Running command - done!
> ionic link
✔ Looking up your apps - done!
? Which app would you like to link Create a new app
? Please enter a name for your new app: ionic-tabs
> ionic config set app_id "1af0c9e5" --json
[OK] app_id set to "1af0c9e5" in ./ionic-tabs/ionic.config.json!
> ionic git remote
> git remote add ionic git@git.ionicjs.com:timotay/ionic-tabs.git
[OK] Added remote ionic.
[OK] Project linked with app 1af0c9e5!
> git add -A
> git commit -m "Initial commit" --no-gpg-sign
Next Steps:
* Go to your newly created project: cd ./ionic-tabs
* Get Ionic DevApp for easy device testing: https://bit.ly/ionic-dev-app
* Finish setting up Ionic Pro Error Monitoring: https://ionicframework.com/docs/pro/monitoring/#getting-started
* Finally, push your code to Ionic Pro to perform real-time updates, and more: git push ionic master
```