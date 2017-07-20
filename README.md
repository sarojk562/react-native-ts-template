### reactNativeTSTemplate
Describes instructions for building a React Native application using TypeScript.

### Installation
In the root directory, running `npm install` would install all the dependencies.

### tsconfig.json
For setting up TypeScript with React Native, the `jsx` flag needs to be set to `react-native`. All the typescript code could be organized inside a single folder (`ts`), and the resultant javascript code can be organised inside another folder (`js`). These folders can be mentioned in the `rootDir` and `outDir` flags respectively.

### Run `tsc` in watch mode
`tsc -w` would run the typescript compiler in watch mode, monitoring the `rootDir` for any changes and instantly compiling to `outDir`.

### To build the app on an Android device
During the first run, all the packages need to be installed. So in the root folder, run:
1. `npm install`
2. `npm install -g react-native` (in case `react-native` does not exist)
3. `react-native link` would link the required assets to the project
4. `react-native run-android` (this command should install the app on your device, with the assumption that `adb` works on your system, and USB debugging has been enabled on your device)

### To enable hot-reload of the application
Connect the physical device and run `react-native run-android`. In another terminal window, run `react-native start`, in the root folder. This would create a development server that creates incremental builds and deploys them on the device.

Also, once the app has loaded on the device, long press the menu key, and an alert window would open with a bunch of option. Select `Enable Hot Reloading` in the menu. This should now automatically pick up any changes made to the `js` files (which at this point, would be compiled from ts/*.tsx files, if the typescript compiler is being run in the watch mode). If hot reloading doesn't work, reinstall the app using `react-native run-android` with `react-native start` still running. This should fix the issue. Now edit `ts/app.tsx` and see the changes live on your device.

### Further instructions
To make the build process easier, use `ts/app.tsx` as the `main` file that exports one single function. This function can then be imported in `index.android.js` or `index.ios.js`. Thus, the `index.*.js` files need not be touched. All new functionality could be added in multiple source files in `ts/` directory.


### To create icons for different platforms
To create icons for different platforms, place your icon under resources folder in root directory or replace the existing icon.png with your file. 
1. `npm install`
2. `brew install imagemagick`
3. `Cmd/Ctrl + Shift + P` ~> This will open a task runner. Type `task` and choose `Ship Icons` from the options shown.
4. Press enter as confirmation of your project name shown and then on asking permission for overwrite type `y` and hit enter.


### To change the name of the project along with app build folders for both ios and android
If you wish you to just change the project name and keep the rest of our app template as it is, do as follows.
1. Change the `name` property in `package.json` file in `{$ROOT_DIR}`.
2. Then upgrage the project to make the necessary file and string changes by `react-native upgrade`.
3. Type `y` for all the consequent questions to change the `newAppName`.
4. With the above commands new project folders will be created accordingly.
5. To remove previous project folders of android `rm -rf android/app/src/main/java/com/{oldAppName}`.
6. To remove previous project folders of ios `rm -rf ios/{oldAppName}*`.
7. Re-run the task of shipping icons. `Cmd/Ctrl + Shift + P` ~> This will open a task runner. Type `task` and choose `Ship Icons` from the options shown.
8. Change strings `{oldAppName}` with `{newAppName}` in 3 files.
    1. `index.android.js`, `index.ios.js` in `{$ROOT_DIR}`.
        `import {newAppName} from './js/app';`
        `AppRegistry.registerComponent('{newAppName}', () => {newAppName});`
    2. `app.tsx` in `{$ROOT_DIR}/ts`.
        `export default class {newAppName} extends React.Component`
9. Now everything is set just re-run the build. For instance `react-native run-ios`.
