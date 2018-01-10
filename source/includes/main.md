# Introduction

This document describes the API specification and wire protocols for the Whiteboard feature of the LiveHelp product.
It describes bindings for Native and Web platforms of the LiveHelp product! 

The source for this API documentation can be found [here](https://github.com/stardotweb/node-slate).

# Authentication

> To authorize, use this code:

```objectivec
#import <UIKit/UIKit.h>
#import "Dependency.h"

@protocol WorldDataSource
@optional
- (NSString*)worldName;
@required
- (BOOL)allowsToLive;
@end

@property (nonatomic, readonly) NSString *title;
- (IBAction) show;
@end
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>


# Broadcasts

## Typical Broadcast Message Structure
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "<message_type>",
    "message": {}
}
```
Broadcast Body | Description
-------------- | -----------
id | The unique message identifier
origin | The identifier of the client originating the broadcast. For system-initiated broadcasts, this field would be `null`
timestamp | The timestamp of the initiation of the message
type | The message type or name
message | The optional message body


# Whiteboard Broadcasts

## Initialize Canvas

```json
{
    "id":"<uuid>",
    "origin": null,
    "timestamp": 1234567890,
    "type": "canvas_init",
    "message": {
        "width": 1920,
        "height": 1080,
        "assignedColor": "red",
        "capabilities": [
            "tools_freehand",
            "tools_circle",
            "tools_rectangle",
            "tools_line",
            "tools_arrow",
            "clear",
            "clear_others",
            "clear_all"
        ]
    }
}
```
Initialize the canvas on the client. The message body suggests the following - 

Message Body | Default | Description
------------ | ------- | -----------
width + height | - | The aspect ratio to render the canvas with
assignedColor | - | The unique-within-a-session color that this client has been assigned for this session. Used to easily distinguish/identify collaborators
capabilities | [] | The list of capabilities that this client session can perform. 

<aside class="info">
The `capabilities` list is used to limit features on a per-client-per-session basis. This is determined by the system based on various factors like client/platform limitations, subscription model, etc.
</aside>


## Clear Canvas
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "canvas_clear",
    "message": {
      "forOrigin": "<uuid>"
    }
}
```
Clear annotations from the canvas - from a particular origin/client or for all 

Message Body | Default | Description
------------ | ------- | -----------
forOrigin | - | The client for which all annotations should be cleared. 

<aside class="info">
In this case, the message body is optional. If no `forOrigin` is provided, all annotations for all origins are to be cleared
</aside>

## Add Annotation - Line
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "line",
        "draw": {
          "x1": 14, "y1": 18,
          "x2": 18, "y2": 14,
          "strokeWidth": 5,
          "strokeColor": "red",
          "fillColor": "red"
        }
    }
}
```
> In order to add a line as an arrow, one or both of `markerStart` and `markerEnd` can be additionally provided - 

```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "line",
        "draw": {
          "markerStart": "none",
          "markerEnd": "arrow",
          "x1": 14, "y1": 18,
          "x2": 18, "y2": 14,
          "strokeWidth": 5,
          "strokeColor": "red",
          "fillColor": "red"
        }
    }
}
```
Add a line/arrow annotation to the canvas

Message Body | Default | Description
------------ | ------- | -----------
type | - | The type of annotation added to the canvas
draw | {x1, y1, } | Information specific to drawing of the annotation. 
draw.x1 + draw.y1 | - | Start




## Initialize Canvas
## Initialize Canvas



```javascript
import {Auth} from "livehelp";

let Api = Auth.authorize("<client_uuid>");

let whiteboard = Api.whiteboard.create();

```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```bash
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve



