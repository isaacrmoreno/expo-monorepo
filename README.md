<h1 align="center">💻 How to Create a Monorepo with Expo 💚</h1>

#### Prerequisites:

- Expo CLI via `npm install -g expo-cli` 

<hr>

1. First Lets create your basic file structure.

```
mkdir mono-repo-demo
cd mono-repo-demo
mkdir apps packages
npm init -y
```
- If your not sure what `npm init -y` is, you can learn more about it [here.](https://docs.npmjs.com/cli/v8/commands/npm-init)
2. Next, navigate into your apps directory and initialize your expo project.
```
cd apps
expo init
```

3. Name your app project, then select one of the following workflow templates: 

```
    ----- Managed workflow -----
❯   blank               a minimal app as clean as an empty canvas
    blank (TypeScript)  same as blank but with TypeScript configuration
    tabs (TypeScript)   several example screens and tabs using react-navigation and TypeScript
    ----- Bare workflow -----
    minimal             bare and minimal, just the essentials to get you started
```
* If your not sure which workflow to pick, you can read more about them [here.](https://docs.expo.dev/introduction/managed-vs-bare/)

4. After your dependencies have installed, navigate back to your root directory and open up your preferred code editor.
```
cd ..
code .
```
- If the `code .` command is not working check out [this](https://stackoverflow.com/questions/50182417/code-command-is-not-working) resource. 

<hr>

- If you selected the `minimal` bare workflow, you can skip ahead [here.](#bare-workflow) 
### Managed Workflow

- If you selected the `blank`, `blank (typescript)`, or `tabs (typescript)` managed workflow then... 


5. Update your root package.json file by... 

- Adding a private value

```
"private": true,
```

- And adding your workspaces

```
"workspaces": [
  "apps/*",
  "packages/*"
  ],
```

<details>
<summary>Your root package.json should look something like this:</summary>

```
{
  "name": "mono-repo-demo",
  "private": true,
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
</details>

6. Next lets navigate to your apps directory and make two new files.

```
cd apps/your-app-name
touch metro.config.js index.js
```

7. Add the following to your metro.config.js file:
```
// Learn more https://docs.expo.io/guides/customizing-metro
const { getDefaultConfig } = require('expo/metro-config');
const path = require('path');

// Find the workspace root, this can be replaced with `find-yarn-workspace-root`
const workspaceRoot = path.resolve(__dirname, '../..');
const projectRoot = __dirname;

const config = getDefaultConfig(projectRoot);

// 1. Watch all files within the monorepo
config.watchFolders = [workspaceRoot];
// 2. Let Metro know where to resolve packages, and in what order
config.resolver.nodeModulesPaths = [
  path.resolve(projectRoot, 'node_modules'),
  path.resolve(workspaceRoot, 'node_modules'),
];

module.exports = config;
```

8. Add the following to your `index.js` file:
```
import { registerRootComponent } from 'expo';

import App from './App';

// registerRootComponent calls AppRegistry.registerComponent('main', () => App);
// It also ensures that whether you load the app in Expo Go or in a native build,
// the environment is set up appropriately
registerRootComponent(App);
```

9. Update your app package.json file's `"name"` property to reflect the following  
```
"name": "@mono-repo-demo/your-app-name",
```

10. Update your app package.json file's `"main"` entry point from `"main": "node_modules/expo/AppEntry.js",` to `"main": "index.js",`

<hr>

### Bare Workflow

- If you selected the `minimal` bare workflow please only complete steps 5 & 9 - Cheers 🍻 

11. Navigate to the initialized expo directory and start your project. 

```
cd apps/your-app-name
yarn start
```

12. press `i` on your keyboard or take a photo of QR code to open the Expo Go app and you should be off to the races! 🏇

#### Further Exploration
* Yarn - [Workspaces](https://classic.yarnpkg.com/en/docs/workspaces) 
* Expo - [Working with Monorepos](https://docs.expo.dev/guides/monorepos/)
