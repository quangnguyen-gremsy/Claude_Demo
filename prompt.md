# Prompt Showcase: YOLOv5 Inference Script

---

## BAD Prompt (Junior Approach)

```
Write a Python script to run YOLOv5 on an image
```

**Vấn đề:**
- AI không biết dùng torch.hub hay ultralytics?
- Output format là gì? Print ra terminal? Save file?
- Confidence threshold bao nhiêu?
- Xử lý lỗi file không tồn tại như thế nào?
- → AI tự quyết hết, output khó reuse

---

## GOOD Prompt (Senior Approach)

```
I need a Python script to benchmark YOLOv5 inference on a single image.
Before writing any code, show me a plan.

Context:
- Support 2 backends: PyTorch (torch.hub yolov5s) and ONNX (onnxruntime)
- Backend selected via CLI flag: --backend pytorch | onnx
- Input: image path from CLI argument, --backend flag, --runs (default 100)
- Output: save results to benchmark.json with this schema:
    {
      "image": "<filename>",
      "backend": "pytorch" | "onnx",
      "benchmark": {
        "runs": int,
        "mean_ms": float,
        "std_ms": float,
        "min_ms": float,
        "max_ms": float,
        "fps": float
      },
      "detections": [
        {"label": str, "confidence": float, "bbox": [x1, y1, x2, y2]}
      ],
      "total_objects": int
    }

Constraints:
- Confidence threshold: 0.4
- Benchmark: warm up 5 runs before measuring, then measure --runs iterations
- Single file, no classes needed
- Handle: file not found, unsupported backend, model load failure
- Python 3.9+, dependencies: torch, torchvision, onnxruntime

Before coding:
1. List the implementation steps
2. What assumptions are you making about ONNX input preprocessing?
3. What are 3 edge cases I should test?
```

**Tại sao tốt hơn:**
- AI biết chính xác cần làm gì → không waste token
- Output schema rõ ràng → code có thể plug vào pipeline khác ngay
- Yêu cầu plan trước → Human review trước khi AI code
- Edge cases được raise sớm → ít bug hơn
```
