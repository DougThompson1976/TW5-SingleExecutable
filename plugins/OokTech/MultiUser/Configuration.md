# Configuration

Configuration for the plugin is set in the `settings.json` file in the
`settings` sub-folder of the folder where the `tiddlywiki.info` file is
located.

Everything is optional, if there are any missing pieces default values will be
used. If the json isn't formatted correctly than default values will be used.

## Example settings.json file

```
{
  "filePathRoot": "/home/inmysocks/TiddlyWiki/Wikis",
  "editionsPath": "/home/inmysocks/TiddlyWiki/Editions",
  "suppressBrowser": false,
  "fileURLPrefix": "file"
  "scripts": {
    "NewWiki": "tiddlywiki #wikiName --init #editionName"
  },
  "wikis": {
    "OneWiki": "/home/inmysocks/TiddlyWiki/Wikis/OneWiki",
    "TwoWiki": "/home/inmysocks/TiddlyWiki/Wikis/TwoWiki",
    "OokTech": {
      "TestWiki": "/home/inmysocks/TiddlyWiki/Wikis/TestWiki"
    }
  },
  "ws-server": {
    "port": 8080,
    "host": "127.0.0.1",
    "autoIncrementPort": false,
    "servePlugin": true
  },
  "heartbeat": {
    "interval": 1000
  },
  "mimeMap": {
    ".ico": "image/x-icon",
    ".html": "text/html",
    ".js": "text/javascript",
    ".json": "application/json",
    ".css": "text/css",
    ".png": "image/png",
    ".jpg": "image/jpeg",
    ".jpeg": "image/jpeg",
    ".wav": "audio/wav",
    ".mp3": "audio/mpeg",
    ".svg": "image/svg+xml",
    ".pdf": "application/pdf",
    ".doc": "application/msword",
    ".gif": "image/gif"
  }
}
```

''Note:'' All can be either absolute or relative. Relative paths are relative
to the folder with tiddlywiki.js in it if you are using the normal version or
the folder with the executable file if you are using the single executable
version.

## What each part is

- `filePathRoot` is the root folder where external files are served. If you
  want to use an external image from your computer in your wiki than you need
  to set this to a parent folder of where the pictures are. If none is given
  than local files aren't served.
- `fileURLPrefix` is the prefix used to distinguish file links from wikis. This
  has the normal restrictions on names as any URL, so avoid special characters.
  This defaults to `file` and only have an affect if you have also set
  `filePathRoot`.
  Note: If you set this to an empty string it will use the default value of
  `file` unless you set the `acceptance` value described below. This will break
  things and no tech support will be provided.
- `mimeMap` lists the file extensions and their associated mime-types that the
  server is allowed to serve. This only has an effect if `filePathRoot` is set.
- `editionsPath` is the folder that holds any custom editions you want to be
  able to use when making wikis using the control panel.
- `suppressBrowser` is only used if you are using the single executable
  version. If it is set to `true` than the browser isn't opened automatically
  when the server is started.
- `scripts` a list of scripts that you can call from inside the wiki using the
  `runScript` websocket message.
- `wikis` a list of child wikis to serve. The path to the wikis is determined
  by the name given. In the example above the wiki located at
  `/home/inmysocks/TiddlyWiki/Wikis/OneWiki` would be served on
  `localhost:8080/OneWiki` and the wiki located at
  `/home/inmysocks/TiddlyWiki/Wikis/TestWiki` would be served on
  `localhost:8080/OokTech/TestWiki`. You may have as many levels and wikis as
  you want.
- `ws-server` settings for the `wsserver` command. It takes the same arguments
  as the normal `server` command with the exception of `autoIncrementPort` and
  `servePlugin`, if `autoIncrementPort` is not set to false than the server
  will try using the given port (`8080` by default) and if it is in use it will
  try the next port up and continue until it finds an open port to use. It will
  do the same for the websockets port. If this is set to false than if the
  given port is in use an error is given and the process fails. The default
  websocket port is one higher than the http port used. If `servePlugin` is not
  false than any child wiki served will include the MultiUser plugin. So you
  can serve wikis that don't normally have the plugin and edit them as though
  they did.
- `heartbeat` settings for the heartbeat that makes sure the browser and server
  are still connected. You can almost certainly ignore this setting. The only
  setting is `interval`, the heartbeat message is sent every `interval`
  milliseconds (1000 milliseconds = 1 second). On slower hardware a longer
  heartbeat may be needed to prevent error messages when there is no error.
- `acceptance` this is a setting for accepting that you will get no help if you
  do something that requires it to be set. These are things that are either
  insecure or have a very good chance of breaking your wiki. You will get no
  tech support if you do any of them. If you want to do it anyway than you need
  to give this the value `I Will Not Get Tech Support For This`.

''Note:'' Only changes to the `scripts` and `wikis` will be available without
restarting the server. You still need to save the settings using the
`Update Settings` button after making changes in the `Manual Settings` tab
under the `Node Settings` tab in the $:/ControlPanel. If you change a wiki name
or path you also need to click on the `Update Routes` button after you click on
the `Update Settings` button for your changes to take effect.

Any other changes require a full server restart.
