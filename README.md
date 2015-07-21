node-red-node-youtube
=====================


Node-RED node that manipulates Youtube Data API.

Install
-------

Run the following command in the root directory of your Node-RED install\n

        npm install node-red-node-youtube

Usage
-----

Contains 3 Nodes that may be used together:

 - Channels List - List a user's channels
 - Playlist List - List a Channel Playlist(s)
 - PlaylistItems List - List the Videos in a Playlist

You will need a Google API Key in order to make use of the Nodes, see (https://developers.google.com/youtube/registering_an_application#Create_API_Keys "Creating API keys").


Example Flow
------------

(https://raw.githubusercontent.com/jlong23/node-red-node-youtube/master/example_getUserPlaylistVideoTitles.jpg)
```JSON
[{"id":"6812e728.97ed18","type":"YouTube Channels","config":"","part":"contentDetails","categoryId":"","forUsername":"ParryGripp","channelIds":"","managedByMe":"","mine":"","mySubscribers":"","maxResults":"50","onBehalfOfContentOwner":"","pageToken":"","name":"ParryGripp Channel","x":340,"y":650,"z":"88661d46.7799e","wires":[["66d56592.992a9c"]]},{"id":"c4b66425.3b4998","type":"YouTube PlayLists","config":"","part":"snippet","channelId":"","playlistId":"","mine":"","maxResults":"50","onBehalfOfContentOwner":"","onBehalfOfContentOwnerChannel":"","pageToken":"","name":"Get Channel Playlists","x":332,"y":709,"z":"88661d46.7799e","wires":[["c4617852.3b9e88"]]},{"id":"66d56592.992a9c","type":"function","name":"Extract Each Channel","func":"var totalResults = msg.payload.pageInfo.totalResults;\nvar maxResults = msg.payload.pageInfo.resultsPerPage;\nvar resultsCount = maxResults < totalResults ? maxResults : totalResults;\n\nvar msgArray = new Array();\nfor ( var x = 0; x < resultsCount; x ++ )\n{\n\tvar newMsg = { channelId: msg.payload.items[x].id };\n\tmsgArray.push( newMsg );\n}\n\nreturn [ msgArray ];\n","outputs":1,"x":605,"y":649,"z":"88661d46.7799e","wires":[["c4b66425.3b4998"]]},{"id":"a3a40160.5c5c","type":"inject","name":"DO IT!","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":143,"y":650,"z":"88661d46.7799e","wires":[["6812e728.97ed18"]]},{"id":"c4617852.3b9e88","type":"function","name":"Extract Each Playlist","func":"var totalResults = msg.payload.pageInfo.totalResults;\nvar maxResults = msg.payload.pageInfo.resultsPerPage;\nvar resultsCount = maxResults < totalResults ? maxResults : totalResults;\n\nvar msgArray = new Array();\nfor ( var x = 0; x < resultsCount; x ++ )\n{\n\tvar newMsg = { playlistId: msg.payload.items[x].id };\n\tmsgArray.push( newMsg );\n}\n\nreturn [ msgArray ];","outputs":1,"x":601,"y":708,"z":"88661d46.7799e","wires":[["fa98eb65.056718"]]},{"id":"fa98eb65.056718","type":"YouTube PlayListItems","config":"","part":"snippet","playlistId":"","playlistItemIds":"","maxResults":"50","onBehalfOfContentOwner":"","pageToken":"","videoId":"","fields":"","name":"Get Playlist Items","x":318,"y":776,"z":"88661d46.7799e","wires":[["7356a04c.8ca96"]]},{"id":"7356a04c.8ca96","type":"function","name":"Extract Each Video","func":"var totalResults = msg.payload.pageInfo.totalResults;\nvar maxResults = msg.payload.pageInfo.resultsPerPage;\nvar resultsCount = maxResults < totalResults ? maxResults : totalResults;\n\nvar msgArray = new Array();\nfor ( var x = 0; x < resultsCount; x ++ )\n{\n\tvar newMsg = { \n\t    playlistItemId: msg.payload.items[x].id,\n\t\tvideoId: msg.payload.items[x].snippet.resourceId.videoId,\n\t\ttitle: msg.payload.items[x].snippet.title,\n\t};\n\tmsgArray.push( newMsg );\n}\n\nreturn [ msgArray ];","outputs":1,"x":591,"y":771,"z":"88661d46.7799e","wires":[["5054136b.afabec"]]},{"id":"57e503f5.a81afc","type":"debug","name":"","active":true,"console":"false","complete":"true","x":1124,"y":734,"z":"88661d46.7799e","wires":[]},{"id":"5054136b.afabec","type":"template","name":"Format for youtube-dl batch download","field":"payload","template":"https://www.youtube.com/watch?v={{videoId}}","x":883,"y":770,"z":"88661d46.7799e","wires":[["57e503f5.a81afc","8f2cc845.70d338"]]},{"id":"8f2cc845.70d338","type":"file","name":"","filename":"/ytdl.txt","appendNewline":true,"overwriteFile":"false","x":1125,"y":807,"z":"88661d46.7799e","wires":[]}]
```
