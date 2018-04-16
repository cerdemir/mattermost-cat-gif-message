# mattermost-cat-gif-message
Bash script to send cat gifs through mattermost incoming webhook

Prerequisites:  
jq  
curl  

Example:  
`./sendmessage.sh Gunaydin millet`

```bash
#!/bin/bash

# mattermost webhook url https://docs.mattermost.com/developer/webhooks-incoming.html
MATTERMOSTURL="https://mattermost.xxxxxxxx.dk/hooks/xxxxxxxxxxxxxxxxxx"

# [subredditurl]/random.json
# get random post as json
REDDITURL="https://reddit.com/r/CatGifs/random.json"

# fallback gif url
DEFAULTGIFURL="https://g.redditmedia.com/AY3cOniQ5X4OGD7h6_FHaYEZOK1BBDB3oDjNyb_YFM8.gif?s=24b8400bc3714e4d574132a7fb923bb8"

# fetch
GIFURL=`curl -L -s -A 'User Agent' $REDDITURL | jq -r .[0].data.children[0].data.preview.images[0].variants.gif.source.url`

# fallback
if [ "$GIFURL" = "null" ]; then
    GIFURL=$DEFAULTGIFURL
fi

# send request to mattermost
curl  -i -X POST -H 'Content-Type: application/json' -d "{\"text\": \"# $@ \n![kedicik](${GIFURL})\"}" $MATTERMOSTURL 
```
