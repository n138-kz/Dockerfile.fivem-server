# [FiveM_bot__credentials](./)

## License

[n138-kz (c) 2025 / MIT](../../../LICENSE)

## Discord API

https://discord.com/developers/applications/1305300608408096890

### Application ID(Client ID)

```text
1305300608408096890
```

### Invite URL

https://discord.com/oauth2/authorize?client_id=1305300608408096890

### Public Key

```text
```

### Token

```text
```

### Client Secret

```text
```

### Guild/Server ID

```text
1160802738175758367
```

### Announcements To Channel ID

```text
1312352257119096873
```

### Status Embed


#### txAdmin discord config

```json:Embed JSON
{
  "title": "{{serverName}}",
  "url": "{{serverBrowserUrl}}",
  "description": "fivem evaluation server",
  "fields": [
    {
      "name": "> STATUS",
      "value": "```\n{{statusString}}\n```",
      "inline": true
    },
    {
      "name": "> PLAYERS",
      "value": "```\n{{serverClients}}/{{serverMaxClients}}\n```",
      "inline": true
    },
    {
      "name": "> NEXT RESTART",
      "value": "```\n{{nextScheduledRestart}}\n```",
      "inline": false
    },
    {
      "name": "> UPTIME",
      "value": "```\n{{uptime}}\n```",
      "inline": false
    }
  ],
  "image": {
    "url": "https://forum-cfx-re.akamaized.net/original/5X/e/e/c/b/eecb4664ee03d39e34fcd82a075a18c24add91ed.png"
  },
  "thumbnail": {
    "url": "https://forum-cfx-re.akamaized.net/original/5X/9/b/d/7/9bd744dc2b21804e18c3bb331e8902c930624e44.png"
  }
}
```

```json:Config JSON
{
  "onlineString": "🟢 Online",
  "onlineColor": "#0BA70B",
  "partialString": "🟡 Partial",
  "partialColor": "#FFF100",
  "offlineString": "🔴 Offline",
  "offlineColor": "#A70B28",
  "buttons": [
    {
      "emoji": "1062338355909640233",
      "label": "Connect",
      "url": "{{serverJoinUrl}}"
    },
    {
      "emoji": "1022921146041122878",
      "label": "Connect txAdmin",
      "url": "http://172.18.1.2:40120"
    },
    {
      "emoji": "1062339910654246964",
      "label": "txAdmin Discord",
      "url": "https://discord.gg/txAdmin"
    }
  ]
}
```
