# Install React App
```sh
$ create-react-app appName
```
# Install electron
```sh
$ yarn add -D electron electron-builder
```
# Install dependences for development
```sh
$ yarn add cross-env electron-is-dev concurrently wait-on
```
# Full script
```sh
$ create-react-app {my-app}; cd {my-app}; yarn add -D electron electron-builder; yarn add cross-env electron-is-dev concurrently wait-on 
```
# Config public/electron.js
``` JS
const { app, BrowserWindow } = require('electron')
const path = require('path')
const isDev = require('electron-is-dev')

function createWindow () {
  // Создаем окно браузера.
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // and load the index.html of the app.
  win.loadURL(
      isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, "../build/index.html")}`
      )

  // Отображаем средства разработчика.
  // win.webContents.openDevTools()
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Некоторые API могут использоваться только после возникновения этого события.
app.whenReady().then(createWindow)

// Quit when all windows are closed.
app.on('window-all-closed', () => {
  // Для приложений и строки меню в macOS является обычным делом оставаться
  // активными до тех пор, пока пользователь не выйдет окончательно используя Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
   // На MacOS обычно пересоздают окно в приложении,
   // после того, как на иконку в доке нажали и других открытых окон нету.
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow()
  }
})

// In this file you can include the rest of your app's specific main process
// code. Можно также поместить их в отдельные файлы и применить к ним require.
```
# Configure package.json 
This
```json
"private": true,
``` 
change to this
```json
"description": "App like app",
"author": "Misha",
"main": "public/electron.js",
"build": {
  "appId": "App.app"
},
"homepage": "./",
``` 
then change this
``` json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```
to this:
```json
"scripts": {
    "react-start": "react-scripts start",
    "react-build": "react-scripts build",
    "react-test": "react-scripts test",
    "react-eject": "react-scripts eject",
    "electron-build": "electron-builder",
    "build": "npm run react-build && npm run electron-build",
    "start": "concurrently \"cross-env BROWSER=none npm run react-start\" \"wait-on http://localhost:3000 && electron .\""
  },
```
# Run build
```sh
$ npm run build
```
# And one more thing
If you whant to change the electron app icon, read [this docs](https://medium.com/fantageek/changing-electron-app-icon-acf26906c5ad)
If you have some questions, go to [video](https://www.youtube.com/watch?v=Cdu2O6o2DCg)
# That's all, you did it! 😇
