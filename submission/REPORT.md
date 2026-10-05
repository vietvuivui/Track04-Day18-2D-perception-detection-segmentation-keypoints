# Lab 18 — 2D Perception: Detection · Segmentation · Keypoints

**Học viên:** Nguyễn Văn Quốc Việt — 2A202602973

## Notebook đã chạy

- GitHub: [lab_2d_perception_student.ipynb](https://github.com/vietvuivui/Track04-Day18-2D-perception-detection-segmentation-keypoints/blob/main/lab_2d_perception_student.ipynb)
- Xem nhanh (nbviewer): <https://nbviewer.org/github/vietvuivui/Track04-Day18-2D-perception-detection-segmentation-keypoints/blob/main/lab_2d_perception_student.ipynb>

Notebook được chạy toàn bộ từ đầu đến cuối (Restart → Run All) trên Google Colab, GPU Tesla T4,
`torch 2.11.0+cu130`, `ultralytics 8.4.171`, `device = cuda`. Output được giữ nguyên trong file.

## Kết quả chính

| Mục | Kết quả |
|---|---|
| 1B · `box_iou`, `nms`, `batched_nms` | ✅ đạt bộ kiểm tra, không dùng phao |
| 1C · NMS trên `bus.jpg` | NMS tự viết giữ 5 box, khớp NMS của Ultralytics; đếm được 4 người, 1 xe buýt |
| 1C · Latency (T4, trung bình 30 lần) | postprocess: one-to-many + NMS ≈ 1.1–1.3 ms, one-to-one NMS-free ≈ 0.4 ms |
| 1D ⭐ · `average_precision` | ✅ đạt, vẽ được đường PR |
| 2B · `mask_iou`, `polygon_to_mask`, `mask_to_yolo_seg` | ✅ đạt; mask IoU YOLO26n-seg ↔ Mask R-CNN 0.842–0.939 |
| 2C · `autolabel/bus.txt` | ✅ hợp lệ, 5 object, IoU polygon ↔ mask SAM 0.967–0.983 |
| 3B · `oks` | ✅ đạt; lệch 8 px: mắt còn 0.53, hông còn 0.97 |
| 3C · `joint_angle` | ✅ đạt; ảnh gốc thân nghiêng 1° (đứng), ảnh xoay 83–94° (🚨 NGÃ?), chỉ còn 3 người |
| 4A · `FLIP_IDX` | ✅ `[0, 1, 2, 3, 7, 6, 5, 4, 10, 11, 8, 9]` |
| 4B · Fine-tune YOLO26n-pose (40 epoch, imgsz 640, GPU) | Box mAP50-95 0.930 · Pose mAP50 0.995 · Pose mAP50-95 0.457 · train 4.1 phút |
| 4B · Phân tích lỗi | OKS trung bình 0.745, 0/53 ảnh bỏ sót, 4/53 ảnh nghi đảo trái ↔ phải |
| Câu hỏi | 12/12 (Q1–Q12 trong notebook và `ket_qua.json`) |
| 4C ⭐ | Không làm |

## File nộp

- `lab_2d_perception_student.ipynb` — notebook đã chạy, còn nguyên output
- `submission/ket_qua.json` — checklist, bảng latency, kết quả 4B, câu trả lời Q1–Q12
- `submission/autolabel/bus.txt` — nhãn YOLO-seg tự động tạo bằng YOLO26 + SAM 2.1
