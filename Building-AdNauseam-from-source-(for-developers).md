##### Setup (OS X)

```
1. git clone https://github.com/dhowe/AdNauseam.git
2. git clone https://github.com/dhowe/uAssets.git
3. Install `jq` if you don't have it (https://stedolan.github.io/jq/)
4. cd AdNauseam
```

##### On Chromium / Opera (Mac OS X):

1. Build the extension in Terminal
```$ tools/make-chromium.sh```
or 
```$ tools/make-opera.sh```

2. Open the browser and go to settings, with URL ```chrome://extensions/``` (also works on Opera)
3. Enable Developer mode in the settings page and load extension from ```/bin/build/adnauseam.chromium``` or ```/bin/build/adnauseam.opera```

##### On Firefox (Mac OS X):

1. Open Firefox and go to ```about:config```, then set ```xpinstall.signatures.required``` to false
2. Make sure you have [jpm](https://www.npmjs.com/package/jpm) installed
3. In terminal:` $ tools/run-ff.sh`
4. (Optional) To use a Firefox profile other than default, pass the profile path to run-ff.sh:
````$ tools/run-ff.sh path/to/profile```` 