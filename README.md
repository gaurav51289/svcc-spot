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
      await imgcat(status.artwork, { log: true, height: 8 });
      this.log(`${chalk.cyanBright('Now Playing ... ')} ${status.track}`);
      this.log(`From ${chalk.yellowBright(status.album)}`);
      this.log(`By ${chalk.yellowBright(status.artist)}`);
      this.log(`Elapsed ${chalk.redBright(status.position)} of ${status.duration}`);
```

topic
```json
"topics": {
      "track": {
        "description": "work with tracks"
      }
    }
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
    "@types/bluebird": "^3.5.28",
    "@types/dotenv": "^6.1.1",
    "@types/inquirer": "^6.5.0",
    "@types/spotify-web-api-node": "^4.0.1"
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

search.ts
```typescript
import * as osa from 'spot-osa';
const imgcat = require('imgcat');
import { default as chalk } from 'chalk';
import { SpotApi } from '../spot-api';
import * as inq from 'inquirer';
inq.registerPrompt('autocomplete', require('inquirer-autocomplete-prompt'));
```

```typescript
 await inq.prompt([
      {
        type: "autocomplete",
        name: "track",
        message: "Search a track",
        source: async (answers: any, input: string) => {
          return new Promise<any>((resolve, reject) => {
        });
        }
      }
    ]).then(async (sel: any) => {
      await osa.play(sel.track.uri);
    });
```


```typescript
 await inq.prompt([
      {
        type: "autocomplete",
        name: "track",
        message: "Search a track",
        source: async (answers: any, input: string) => {
          return new Promise<any>((resolve, reject) => {
            if (!input) {
              resolve([]);
              return;
            }
            this.spotApi
              .search(input)
              .then((items: any[]) => {
                resolve(
                  items.map(item => {
                    return {
                      name: `${item.name} - ${item.album.name}`,
                      value: item
                    };
                  })
                );
              })
              .catch(err => {
                reject(err);
              });
          });
        }
      }
    ]).then(async (sel: any) => {
      await osa.play(sel.track.uri);
    });
    
    await osa.status().then(async (status: any) => {
      await imgcat(status.artwork, { log: true, height: 8 });
      this.log(`${chalk.cyanBright("Now Playing ... ")} ${status.track}`);
      this.log(`From ${chalk.yellowBright(status.album)}`);
      this.log(`By ${chalk.yellowBright(status.artist)}`);
      this.log(
        `Elapsed ${chalk.redBright(status.position)} of ${status.duration}`
      );
    });

```

