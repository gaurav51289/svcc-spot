# spot-cli
--------

Dep
```json
    "@oclif/plugin-plugins": "^1.7.8",
    "imgcat": "^2.3.0",
    "spot-osa": "file:/Users/gchodwadia/Desktop/svcc/npmlibs/spot-osa-1.0.0.tgz"
```
----------
play command

Imports
```javascript		
import * as osa from 'spot-osa';
const imgcat = require('imgcat');
import { default as chalk } from 'chalk';
```

URI: spotify:track:5HNCy40Ni5BZJFw1TKzRsC

Arg
```javascript


```


Print
```javascript
      imgcat(status.artwork, { log: true, heightc: 8 });
      this.log(`${chalk.cyanBright('Now Playing ... ')} ${status.track}`);
      this.log(`From ${chalk.yellowBright(status.album)}`);
      this.log(`By ${chalk.yellowBright(status.artist)}`);
      this.log(`Elapsed ${chalk.redBright(status.position)} of ${status.duration}`);
```

-------------------
-------------------


# plugin-search

Deps
```json
    "dotenv": "^8.1.0",
    "imgcat": "^2.3.0",
    "inquirer": "^7.0.0",
    "inquirer-autocomplete-prompt": "^1.0.1",
    "spot-osa": "file:/Users/gchodwadia/Desktop/svcc/npmlibs/spot-osa-1.0.0.tgz",
    "spotify-web-api-node": "^4.0.0"
```
```
    "postpack": "rm -f oclif.manifest.json && cp plugin-search-0.0.0.tgz /Users/gchodwadia/Desktop/svcc/npmlibs",
    "prepack": "rm -rf lib && rm -rf tsconfig.tsbuildinfo && tsc -b && oclif-dev manifest && oclif-dev readme",
```
-------------
```bash
$ touch src/spot-api.ts
```
```typescript
// @ts-ignore
import * as Spotify from 'spotify-web-api-node';
import * as dotenv from 'dotenv';

export class SpotApi {

  private spotify: Spotify;

  constructor() {
    dotenv.config();
    this.spotify = new Spotify({
      clientId: '491abdfe39e145c4b9d9df8480cbfe85',
      clientSecret: '24401904a7d74bb5aaa466c94aab97d0'
    });
  }


  public search = (query: string): Promise<any> => {
    return new Promise<any>((resolve, reject) => {
      this.spotify.clientCredentialsGrant().then((data) => {
        this.spotify.setAccessToken(data.body['access_token']);
        this.spotify.searchTracks(query,{ limit: 20 }).then((data: any) => {
          resolve(data.body.tracks.items);
        }).catch(err => {
          reject(err);
        });
      }).catch(err => {
        console.log(err);
        reject(err);
      });
    });
  }
}
```

