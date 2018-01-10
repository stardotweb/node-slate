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
draw | {} | Information specific to drawing of the annotation. 


`draw` Structure | Default | Description
---------------- | ------- | -----------
x1 + y1 | - | Starting coordinates for the line
x2 + y2 | - | End coordinates for the line
strokeWidth | 5 | Width of the line
strokeColor | - | Color of the line
markerStart | "none" | Optional - one of `none`, `arrow`, `circle`
markerEnd | "none" | Optional - one of `none`, `arrow`, `circle`

## Add Annotation - Rectangle
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "rect",
        "draw": {
          "x":14,"y":18,
          "width":65,"height":32,
          "strokeWidth":5,
          "strokeColor":"hsla(0, 0%, 0%, 1)"
        }
    }
}
```
Add a rectangle/square annotation to the canvas

Message Body | Default | Description
------------ | ------- | -----------
type | - | The type of annotation added to the canvas
draw | {} | Information specific to drawing of the annotation. 


`draw` Structure | Default | Description
---------------- | ------- | -----------
x + y | - | Starting coordinates for the shape
width + height | - | Dimensions of the shape
strokeWidth | 5 | Width of the shape
strokeColor | - | Color of the shape

## Add Annotation - Ellipse
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "ellipse",
        "draw": {
          "cx":14, "cy":18,
          "rx":100, "ry":200,
          "strokeWidth":5,
          "strokeColor":"hsla(0, 0%, 0%, 1)",
          "fillColor":"hsla(0, 0%, 100%, 1)"
        }
    }
}
```
Add a circle/ellipse annotation to the canvas

Message Body | Default | Description
------------ | ------- | -----------
type | - | The type of annotation added to the canvas
draw | {} | Information specific to drawing of the annotation. 


`draw` Structure | Default | Description
---------------- | ------- | -----------
cx + cy | - | Center coordinates for the shape
rx + ry | - | Dimensions of the shape
strokeWidth | 5 | Width of the shape
strokeColor | - | Stroke color of the shape
fillColor | - | Fill color of the shape

## Add Annotation - Freehand
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "path",
        "draw": {
          "points":"[[4,5],[5,5],[6,5],[6,5],[7,5],[8,5],[9,5]]",
          "pointSize":5,
          "pointColor":"hsla(0, 0%, 0%, 1)"
        }
    }
}
```
Add a freehand drawing annotation to the canvas

Message Body | Default | Description
------------ | ------- | -----------
type | - | The type of annotation added to the canvas
draw | {} | Information specific to drawing of the annotation. 


`draw` Structure | Default | Description
---------------- | ------- | -----------
points | [] | Coordinates representing the freehand shape drawn
pointSize | 5 | Width of the freehand drawing shape
pointColor | - | Stroke color of the freehand drawing shape


## Add Annotation - Text
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "text",
        "draw": {
          "x":14, "y":18,
          "content": "Hello World!",
          "fontSize":"18px",
          "fill": "red"
        }
    }
}
```
Add a text annotation to the canvas

Message Body | Default | Description
------------ | ------- | -----------
type | - | The type of annotation added to the canvas
draw | {} | Information specific to drawing of the annotation. 


`draw` Structure | Default | Description
---------------- | ------- | -----------
x + y | - | Top-left coordinates for the text annotation added
content | "" | The text content of the annotation
fill | - | Color of the text
fontSize | - | Size of the font for the text


## Undo Last Annotation
```json
{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_undo_last"
}
```
Remove the last annotation that was added by a collaborator

<aside class="info">
The metadata in the message (not body) can be used to determine which annotation to remove - <br>
specifically, the `origin` and `timestamp` properties of the broadcast message
</aside>