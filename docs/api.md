API & Data Contracts — BananaPeel
Coordinate System

All boxes/points are normalized to the original image size unless noted.

Endpoints
POST /segment

Runs segmentation on an image region/prompt and returns a mask.

Request

{
  "imageId": "string",
  "imageUrl": "string (optional)",
  "prompt": {
    "type": "box|points",
    "box": [0.10, 0.12, 0.45, 0.82],
    "points": [[0.33,0.52,1],[0.35,0.49,0]]
  },
  "options": {
    "return": "rle|png",
    "previewScale": 1.0
  }
}


Response (RLE example)

{
  "mask": { "format": "rle", "size": [H, W], "counts": "<coco-rle>" },
  "bbox": [x, y, w, h],
  "score": 0.97,
  "layerName": "segment_1"
}


Response (PNG alpha example)

{
  "mask": { "format": "png", "width": W, "height": H, "data": "data:image/png;base64,..." },
  "bbox": [x, y, w, h],
  "score": 0.95,
  "layerName": "segment_1"
}


Implementation note: a temporary stub response may use "format": "bbox" with just bbox for initial wiring.

POST /nl/plan

Converts a natural-language instruction into a list of canvas actions.

Request

{
  "imageId": "img_123",
  "utterance": "cut out the person and put them in front of the car",
  "sceneSummary": { "layers": [] },
  "mode": "plan_and_ground"
}


Response

{
  "actions": [
    {"op":"segment","target":{"query":"person"},"prompt":{"type":"box","box":[0.18,0.22,0.36,0.78]},"result_layer_name":"person_1"},
    {"op":"segment","target":{"query":"car"},"prompt":{"type":"box","box":[0.40,0.48,0.88,0.86]},"result_layer_name":"car_1"},
    {"op":"reorder","layer":"person_1","zAction":"bringInFrontOf","relativeTo":"car_1"}
  ]
}

Scene JSON (draft)
{
  "image": { "id": "img_123", "width": 1920, "height": 1080 },
  "layers": [
    { "id": "person_1", "type":"image-mask", "source":"img_123", "mask": { "format":"rle", "size":[H,W], "counts":"..." }, "transform": { "tx":120, "ty":-30, "scale":1.05, "rot":0 }, "zIndex": 3 }
  ],
  "history": []
}
