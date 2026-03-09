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

---

## PROMPT 2 — Implement (sau khi review PLAN.md)

```
Plan looks good. Now implement Step 1 and Step 2 only:
- Step 1: CLI parsing + input validation
- Step 2: Image preprocessing (shared for both backends)

Do not implement inference yet.
After each step, show me the code and stop.
```

---

## PROMPT 3 — Tiếp tục từng bước

```
Good. Now implement Step 3 (PyTorch backend) only.
Keep the code compatible with the preprocessing from Step 2.
Stop after Step 3, do not implement ONNX yet.
```

---

## PROMPT 4 — Hoàn thiện

```
Now implement Step 4 (ONNX backend) and Step 5 (benchmark loop).
Reuse the preprocessing from Step 2.
Reminder: warm up 5 runs before measuring.
```

---

> Workflow: Plan First → Review → Step-by-step → Commit mỗi bước
> "Not reviewing AI code is the same as merging a PR you didn't read"
