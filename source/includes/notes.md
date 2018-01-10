# Whiteboard API
- Pub/Sub
    - init
    - subscribe
    - broadcast
- Canvas Lifecycle
    - Init
      - Dimensions / Aspect Ratio
      - Capabilities
    - Reset
      
- Drawing
    - Line/Arrow
    - Circle
    - Rectangle
    - Freehand
    - Undo Last
- Cleanup

> Room related

```json

```
> Whiteboard specific

```json

{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "canvas_reset",
    "message": {
        "width": 1920,
        "height": 1080,
        "capabilities": [
            "clear_all"
        ]
    }
}

{
    "id":"<uuid>",
    "client": "<uuid>",
    "timestamp": 1234567890,
    "type": "canvas_clear"
}

{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "line",
        "draw": {
          "endpoint": "arrow",
          "x1":14, "y1":18,
          "x2":18, "y2":14,
          "strokeWidth":5,
          "strokeColor":"hsla(0, 0%, 0%, 1)",
          "fillColor":"hsla(0, 0%, 100%, 1)"
        }
    }
}

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
          "fill": "red"
        }
    }
}


{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_add",
    "message": {
        "type": "path",
        "draw": {
          "points":"[[4,5],[4,5],[4,5],[4,5],[5,5],[6,5],[6,5],[7,5],[7,5],[8,5],[8,5],[9,5],[9,5],[10,5],[11,5],[12,5],[13,5],[13,5],[14,5],[14,5],[14,5]",
          "pointSize":5,
          "pointColor":"hsla(0, 0%, 0%, 1)"
        }
    }
}

{
    "id":"<uuid>",
    "origin": "<uuid>",
    "timestamp": 1234567890,
    "type": "annotation_undo_last",
    "message": {
        "forOrigin": "<uuid>"
    }
}


```