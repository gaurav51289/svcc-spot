# spot-cli
--------

Dep
```json
"@oclif/plugin-plugins": "^1.7.8",
"chalk": "^2.4.2",
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

Print
```javascript
      imgcat(status.artwork, { log: true, length: 8 });
      this.log(`${chalk.cyanBright('Now Playing ... ')} ${status.track}`);
      this.log(`From ${chalk.yellowBright(status.album)}`);
      this.log(`By ${chalk.yellowBright(status.artist)}`);
      this.log(`Elapsed ${chalk.redBright(status.position)} of ${status.duration}`);
```




# plugin-search
