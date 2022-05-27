## What
這是一個測試如何用 verdaccio 及 babel 來打包一個 Create-React-App 專案。
___
## verdaccio
verdaccio 提供了私人的 npm proxy 伺服器，你可以在本地端開發相關 libraies 並上傳至 verdaccio 供整個團隊使用。

1. 安裝：`npm i -g verdaccio`   
執行：`verdaccio`  

2. 緊接著你就可以在 `http://localhost:4837` 看見 verdaccio 的後台網站。  
接著進入你要開發的 library。  
執行:`npm set registry http://localhost:4873/`  
這會幫助你設定遠端 repo 的位址，就是 verdaccio proxy。  
或者你也可以選擇在根目錄建立 `.npmrc` 檔案，並編輯如下:

```js
// .npmrc
registry="http://localhost:4873/"
```

3. 當你開發完後，可以利用下列指令打包並上傳。  
`npm adduser --registry http://localhost:4873`  
會叫你輸入 username / password / email ，之後就可以用這個帳密登入後台

4. 執行: `npm publish --registry http://localhost:4873` 打包上傳。
___
## React / Babel
### 1. 資料夾結構
```js
├── App.css
├── App.js
├── App.test.js
├── index.css
├── index.js
├── library
│   ├── button.js
│   └── index.js
├── logo.svg
├── reportWebVitals.js
└── setupTests.js
```
在 `src` 資料夾底下增加 `library` 資料夾準備放我們的元件並統一用 `index.js` 輸出。
### 2.Babel
安裝相關 dependencies  
`npm i -s -d @babel/core @babel/cli @babel/preset-env`  
`npm i s @babel/polyfill`

設定 `package.json` build command
```js"
...
"build": "rm -rf dist && NODE_ENV=production babel src/lib --out-dir dist --copy-files"
```
另外還要新建一個 `.babelrc` 檔案來做 babel config 設定
```js
// .babelrc
preset: [
  {
    "presets": [
      [
        "@babel/env",
          {
            "targets": {
              "edge": "17",
              "firefox": "60",
              "chrome": "67",
              "safari": "11.1"
            },
            "useBuiltIns": "usage",
            "corejs": "3.6.5"
          }
      ],
      "@babel/preset-react"
    ]
  }
]
```

### 3.build
這時我們可以開始來打包並上傳啦！  
首先執行 `npm run build` 會發現根目錄多出了一個 `dist` 資料夾  
這時候只要再執行 `npm publish --registry="http://localhost:4873"`
就可以在 `verdaccio` 看到你上傳的 library 啦:heart_eyes:

## 參考資源
- [Publish React components as an npm package](https://levelup.gitconnected.com/publish-react-components-as-an-npm-package-7a671a2fb7f) by JB
