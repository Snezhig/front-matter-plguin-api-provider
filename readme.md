## Obsidian Front Matter Title Plugin Api Provider

This package provide some functions to get API from to resolve file titles
by [Front Matter Title](https://github.com/Snezhig/obsidian-front-matter-title) which is required to be installed


## Installation
`npm i front-matter-plugin-api-provider`


### Documentation

|                      Method                      |                                  Description                                  |
|:------------------------------------------------:|:-----------------------------------------------------------------------------:|
|        `isPluginEnabled(app: App): bool`         |        returns true if `obsidian-front-matter-title-plugin` is enabled        |
|       `getApiSafe(app: App): ApiInterface`       |       returns an object which catch internal errors and get Api itself        |
|      `getDeffer(app: App): DefferInterface`      | returns an object that provides functions to check plugin's state and get API |
| `DefferInterface.getApi(): ApiInterface or null` |                    returns API if plugin is ready or null                     |
|     `DefferInterface.isPluginReady(): bool`      |                        returns true if plugin is ready                        |
|  `DefferInterface.awaitPlugin(): Promise<void>`  |          returns promise which will be resolved when plugin is ready          |
|    `DefferInterface.isFeaturesReady(): bool`     |                  returns true if plugin's features are ready                  |
| `DefferInterface.awaitFeatures(): Promise<void>` |   returns promise which will be resolved when plugin and features are ready   |

### Get API

```typescript
import {getDeffer} from "front-matter-plugin-api-provider";

const path = 'Folder/ds1.md';
const deffer = getDeffer(this.app);
let api = null;
if (deffer.isPluginReady()) {
    api = deffer.getApi();
} else {
    await deffer.awaitPlugin();
    api = deffer.getApi();
    //if you want to wait features you can use the following chain
    if (!deffer.isFeaturesReady()) {
        await deffer.awaitFeatures();
    }
}
//Resolve title asynchronously
await api.resolve(path).then(console.log)

//Resolve title synchronously
console.log(api.resolveSync(path));
```

or you can use **Safe Wrapper**

```typescript
import {getApiSafe} from "front-matter-plugin-api-provider";

const path = 'Folder/ds1.md';
//Wrapper will check
const api = getApiSafe(this.app);

//Resolve title asynchronously.
//It will a promise which waits plugin or features, then resolves title.
api.resolve(path).then(console.log);

//Resolve title synchronously.  It will return null, if plugin is not ready yet
console.log(api.resolveSync(path));
```
