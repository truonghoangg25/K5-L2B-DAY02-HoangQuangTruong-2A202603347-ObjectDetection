# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Hoàng Quang Trường
- MSSV:2A202603347
- Email:26ai.truonghq@vinuni.edu.   
- Lớp:E403
- Link repo bài làm:https://github.com/truonghoangg25/K5-L2B-DAY02-HoangQuangTruong-2A202603347-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/drive/1ucD_YxW2SzAQg90uR-kai9On9S_sNqrG#scrollTo=Jz0zZwG4RM3g

## 2. Schema và mini guideline
Quy tắc chung:

Box bao quanh sát phần thân xe nhìn thấy được, hạn chế khoảng trống thừa và không cắt mất phần xe nhìn thấy.

Vật thể bị che khuất vẫn được gán nhãn nếu có thể nhận diện được loại xe; box chỉ bao quanh phần xe nhìn thấy.

Vật thể bị cắt bởi rìa ảnh vẫn được gán nhãn nếu nhận diện được; box kéo sát đến rìa ảnh.

Vật thể rất nhỏ hoặc mơ hồ không được tự đoán nhãn nếu không đủ bằng chứng.

| Ảnh        | Tình huống                                         | Quyết định cuối                          | Lý do theo schema |
|:-----------|:---------------------------------------------------|:-----------------------------------------|:------------------|
| `drive_14` | Xe cứu hộ có hình dáng gần giống xe van            | `van`                                    | Phân loại theo hình dạng thân xe (dạng van) và đặc điểm nhận dạng của phương tiện; không căn cứ vào chức năng cứu hộ. |
| `drive_37` | Xe tưới đường có hình dáng gần giống xe van        | `van`                                    | Theo schema, nhãn dựa trên cấu trúc và thiết kế xe hơn là chức năng sử dụng; xe này có hình dạng chủ yếu là xe van. |
| `drive_42` | Xe bus bị che mất quá nửa, chỉ còn thấy phần đuôi | `bus` nếu vẫn nhận diện được là xe buýt | Schema cho phép giữ nhãn xe bị che khuất khi vẫn xác định được loại xe; box chỉ bao quanh phần xe còn thấy. |

Quy tắc chung:
- Box bao quanh sát phần thân xe nhìn thấy được, hạn chế khoảng trống bên trong box và không cắt mất phần xe nhìn thấy.
- Vật thể bị che khuất vẫn được gán nhãn nếu có thể nhận diện được loại xe; box chỉ bao quanh phần xe nhìn thấy.
- Vật thể bị cắt bởi rìa ảnh vẫn được gán nhãn nếu nhận diện được; box kéo sát đến rìa ảnh tại vị trí xe bị cắt.
- Vật thể rất nhỏ hoặc mơ hồ không được tự đoán nhãn nếu không đủ bằng chứng; ghi nhận trường hợp để xem xét lại.
## 3. Annotation và self-QC

- Số file nhãn train: 50 file
- Kết quả checker và lỗi đã xử lý: đạt cấu trúc 50/50 file nhãn hợp lệ; tên file ảnh và nhãn tương ứng.  
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Đã kiểm tra cấu trúc nhãn bằng tools/check_yolo_labels.py và rà soát các trường hợp phân lớp khó, đặc biệt là drive_002, drive_014, drive_037, drive_042.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Tập nhãn được đối chiếu với Gold để phát hiện nhãn sai lớp hoặc còn thiếu. Các box hiện tại không được xác định là sai được giữ lại; không thay toàn bộ nhãn của người làm bằng Gold.
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số              | Trước sửa | Sau sửa nếu có |
|---------------------|----------:|---------------:|
| TP                  |       283 |            650 |
| FP                  |       271 |            192 |
| FN                  |       370 |              3 |
| Precision           |    0.5108 |         0.7720 |
| Recall              |    0.4334 |         0.9954 |
| F1                  |    0.4689 |         0.8696 |
| mean_iou_of_matches |    0.7657 |         0.8734 |
| passed              |     false |           true |

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số   | Giá trị         |
|----------|----------------:|
| Precision | 0.6715 (67.15%) |
| Recall    | 0.4449 (44.49%) |
| mAP50     | 0.5160 (51.60%) |
| mAP50–95  | 0.3046 (30.46%) |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Chỉ số precision và recall tổng thể hiện có lần lượt là 0.6715 và 0.4449. Bản tóm tắt metrics hiện có không chứa bảng recall riêng từng class, vì vậy không thể xác định chính xác lớp có recall thấp nhất hoặc chọn ảnh minh họa cho lớp đó chỉ từ các số liệu tổng thể. Cần đối chiếu confusion matrix và kết quả theo class trong assets/yolo26_runs/vehicle_eval/ để kết luận.

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra độ chính xác của bounding box: box có quá rộng hoặc quá chặt không, có lệch khỏi vật thể không, có bao phủ đủ phần vật thể nhìn thấy không. mAP50 sử dụng ngưỡng IoU 0.5, còn mAP50–95 đánh giá trên nhiều ngưỡng IoU nghiêm ngặt hơn. Vì vậy, box có thể đạt ở ngưỡng 0.5 nhưng không đủ sát ở các ngưỡng cao hơn.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Các trường hợp phương tiện chuyên dụng có hình dáng gần giống van như xe cứu hộ drive_14 và xe tưới đường drive_37 có thể gây nhầm lẫn giữa van với các lớp khác. Mini guideline đã thống nhất phân loại hai trường hợp này theo cấu trúc và hình dáng xe thay vì chức năng sử dụng. Đây là cơ sở để kiểm tra khả năng nhầm class. Tuy nhiên, muốn kết luận mô hình thực sự mắc lỗi class nào cần đối chiếu dự đoán với nhãn ground truth của từng ảnh.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không đưa 10 ảnh eval vào quá trình cập nhật trọng số khi train để giữ dữ liệu đánh giá tách khỏi dữ liệu huấn luyện. Tuy nhiên, theo đúng notebook, data.yaml sử dụng 10 ảnh này làm val; chúng được dùng để theo dõi metric trong quá trình huấn luyện và hỗ trợ lựa chọn best.pt, sau đó tiếp tục được dùng để đánh giá model bằng best_model.val(). Vì vậy, đây là tập validation/evaluation của notebook, không phải một test set hoàn toàn độc lập chưa từng được dùng để lựa chọn checkpoint.

## 6. Khó khăn và cách xử lý

| Bước | Vấn đề | Cách xử lý | Kết quả / còn thiếu |
|:---|:---|:---|:---|
| Annotation | Xe chuyên dụng dễ nhầm class | Phân loại theo hình dáng | `drive_14`, `drive_37`: `van`; `drive_42`: `bus` nếu nhận diện được. |
| Gold QC | Box lệch, sai class hoặc bị thiếu | So khớp IoU; sửa class, thêm box thiếu, giữ box chưa đủ căn cứ xóa | F1: 0.4689 → 0.8696; mean IoU: 0.7657 → 0.8734. |
| `drive_002` | Xe ngoài cùng bên phải có thể bị gán sai | Kiểm tra thủ công, không sửa tự động theo Gold | Giữ nhãn `van`. |
| Colab | Cần lưu ảnh dự đoán và log train | Tải `day2_colab_outputs_final.zip` | Đã tải; cần giải nén vào `report/individual/assets/`. |
| Báo cáo | Thiếu log runtime và recall từng class | Đối chiếu notebook, metrics và repository | Chưa xác minh runtime/version và per-class recall. |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Hoàng Quang Trường
- MSSV: 2A202603347
- Email: 26ai.truonghq@vinuni.edu.vn
- Lớp: E403
- Link repo bài làm: https://github.com/truonghoangg25/K5-L2B-DAY02-HoangQuangTruong-2A202603347-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1ucD_YxW2SzAQg90uR-kai9On9S_sNqrG#scrollTo=Jz0zZwG4RM3g

## 2. Schema và mini guideline

Schema sử dụng 4 lớp: `car`, `truck`, `bus`, `van`. Việc phân loại dựa trên hình dáng và cấu trúc phương tiện quan sát được, không chỉ dựa vào chức năng sử dụng.

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
|---|---|---|---|
| `drive_14` | Xe cứu hộ có hình dáng gần giống xe van | `van` | Phân loại theo hình dạng thân xe và đặc điểm nhận dạng của phương tiện, không căn cứ riêng vào chức năng cứu hộ. |
| `drive_37` | Xe tưới đường có hình dáng gần giống xe van | `van` | Dựa trên cấu trúc và thiết kế xe quan sát được thay vì chức năng sử dụng. |
| `drive_42` | Xe bus bị che khuất quá nửa, chỉ còn thấy phần đuôi | `bus` nếu vẫn nhận diện được | Giữ nhãn khi vẫn đủ đặc điểm nhận diện loại xe; box chỉ bao quanh phần xe nhìn thấy. |

Quy tắc chung:
- Box bám sát vật thể, hạn chế khoảng trống thừa.
- Vật thể bị che khuất vẫn được gán nhãn nếu có thể nhận diện lớp và xác định vị trí.
- Vật thể bị cắt bởi rìa ảnh: box kết thúc tại rìa ảnh, không suy đoán phần nằm ngoài khung hình.
- Vật thể rất nhỏ hoặc mơ hồ chỉ được gán nhãn khi có đủ bằng chứng để nhận diện.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file.
- Kết quả checker: đạt cấu trúc 50/50 file nhãn hợp lệ; tên file ảnh và nhãn tương ứng.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: **[CẦN BỔ SUNG: ghi tên 10 ảnh thực tế đã kiểm tra]**.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Trong quá trình kiểm tra, tập nhãn được đối chiếu với Gold để phát hiện nhãn sai lớp hoặc còn thiếu. Các box đã có nhưng không được xác định là sai được giữ lại; không thay toàn bộ nhãn của người làm bằng Gold.
- Commit khóa nhãn ban đầu: **[CẦN BỔ SUNG: mã commit ban đầu nếu có; kiểm tra bằng `git log --oneline`]**.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa |
|---|---:|---:|
| TP | 283 | 650 |
| FP | 271 | 192 |
| FN | 370 | 3 |
| Precision | 0.5108 | 0.7720 |
| Recall | 0.4334 | 0.9954 |
| F1 | 0.4689 | 0.8696 |
| mean_iou_of_matches | 0.7657 | 0.8734 |
| passed | false | true |

Kết quả sau rework đạt ngưỡng đánh giá: F1 từ 0.80 trở lên và mean IoU từ 0.70 trở lên.

Các thao tác rework đã thực hiện:
- Đối chiếu box của nhãn hiện tại với Gold bằng IoU.
- Với các box khớp nhau nhưng khác class, sửa class theo kết quả đối chiếu, giữ nguyên tọa độ box hiện tại.
- Bổ sung các box Gold chưa có box tương ứng trong nhãn hiện tại.
- Giữ lại các box hiện tại không khớp Gold thay vì tự động xóa, vì cần tránh loại bỏ đối tượng có thể vẫn hợp lệ.
- Riêng `drive_002`, không áp dụng tự động sửa theo Gold đối với phương tiện ở phía ngoài cùng bên phải vì đã kiểm tra và xác định đó là `van`.
- Kết quả đánh giá cuối được lưu tại `outputs/annotation_evaluation_final.json`. Đây là kết quả sau khi dùng Gold để kiểm tra và rework, không phải phép đo độc lập về chất lượng gán nhãn ban đầu.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: **[CẦN BỔ SUNG từ thông tin runtime trong Colab; không tự suy đoán]**.
- Các tham số thay đổi so với notebook: Đã chạy notebook theo cấu hình hiện có; **[CẦN KIỂM TRA notebook nếu có thay đổi tham số]**.
- Số epoch huấn luyện: 50.
- Đường dẫn best checkpoint trong phiên chạy: `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.6715 (67.15%) |
| Recall | 0.4449 (44.49%) |
| mAP50 | 0.5160 (51.60%) |
| mAP50–95 | 0.3046 (30.46%) |

Kết quả dự đoán được lưu trong `assets/eval_predictions/`. Kết quả huấn luyện và checkpoint được lưu trong `assets/yolo26_runs/`.

Ví dụ: trên ảnh `drive_058.jpg`, mô hình dự đoán các phương tiện thuộc nhóm car, bus và van. Ảnh dự đoán được lưu trong thư mục kết quả eval để đối chiếu trực quan.

### Trả lời bốn câu hỏi cuối notebook

1. **Lớp nào recall thấp nhất? Ảnh nào minh họa?**

   **[CẦN BỔ SUNG]** Cần xem bảng recall theo từng class và ảnh minh họa trong kết quả đánh giá. Không thể xác định chính xác lớp có recall thấp nhất chỉ từ số lượng dự đoán trên ảnh.

2. **Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**

   Cần kiểm tra độ chính xác của tọa độ bounding box: box có quá rộng hoặc quá chặt không, có lệch khỏi vật thể không, có bao phủ đủ phần vật thể nhìn thấy không. mAP50 dùng ngưỡng IoU 0.5, trong khi mAP50–95 đánh giá ở nhiều ngưỡng IoU nghiêm ngặt hơn, nên box chưa đủ chính xác có thể làm mAP50–95 thấp.

3. **Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**

   Các trường hợp phương tiện chuyên dụng như xe cứu hộ `drive_14` và xe tưới đường `drive_37` có thể gây nhầm lẫn giữa `van` và các lớp khác. Đây là các trường hợp đã được ghi lại trong mini guideline. Muốn kết luận mô hình thực sự mắc lỗi class nào cần kiểm tra dự đoán và nhãn ground truth tương ứng.

4. **Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

   Không dùng ảnh eval để train nhằm giữ tập đánh giá độc lập, tránh rò rỉ dữ liệu và đánh giá quá lạc quan. Tập validation được dùng để theo dõi hiệu quả mô hình trong quá trình huấn luyện, hỗ trợ đánh giá khả năng tổng quát hóa và lựa chọn checkpoint phù hợp. Tập eval được dùng để đánh giá cuối cùng trên dữ liệu chưa tham gia huấn luyện.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation | Một số phương tiện chuyên dụng khó phân biệt class | Ghi lại các trường hợp mơ hồ trong mini guideline | Đã thống nhất cách phân loại cho `drive_14`, `drive_37`, `drive_42` |
| Gold QC | Có box hiện tại không khớp trực tiếp với Gold | Đối chiếu bằng IoU, sửa class hoặc bổ sung box khi phù hợp | F1 tăng từ 0.4689 lên 0.8696 |
| Colab | Cần lưu kết quả huấn luyện và dự đoán để đưa vào báo cáo | Chạy toàn bộ notebook và đóng gói kết quả thành ZIP | Đã tải được `day2_colab_outputs_final.zip`; cần chép hai thư mục kết quả vào `assets/` của repo |
| Báo cáo | Một số thông tin chi tiết chưa được ghi lại | Kiểm tra lại log, runtime và kết quả notebook | Còn cần bổ sung thông tin runtime, self-QC và recall theo class |

## 7. Tự đánh giá theo rubric

- **Schema và guideline:** Đã có định nghĩa 4 class, quy tắc gán nhãn và ghi lại 3 trường hợp mơ hồ.
- **Annotation:** Đã có 50 file nhãn train và checker xác nhận cấu trúc 50/50 hợp lệ. Cần bổ sung danh sách 10 ảnh self-QC thực tế và commit khóa nhãn ban đầu.
- **Gold QC:** Đã thực hiện đối chiếu và rework; kết quả cuối đạt F1 0.8696 và mean IoU 0.8734.
- **YOLO:** Đã chạy notebook 50 epoch, có checkpoint và kết quả đánh giá trên 10 ảnh eval. Cần bổ sung thông tin runtime và phân tích recall theo class.
- **Minh chứng:** Đã tải ZIP kết quả Colab; cần kiểm tra việc đặt hai thư mục `assets/eval_predictions/` và `assets/yolo26_runs/` cạnh báo cáo.

Không tự nhận hoàn thành các mục chưa kiểm chứng.

## 8. Công cụ AI và tài liệu tham khảo

- ChatGPT: hỗ trợ giải thích quy trình, hướng dẫn thao tác Git/WSL/Colab, hỗ trợ kiểm tra và trình bày báo cáo. Các kết quả số liệu được lấy từ output thực tế của bài làm.
- Google Colab: chạy notebook huấn luyện YOLO và đánh giá trên tập eval.
- Git và GitHub: quản lý phiên bản và lưu trữ bài làm.
- Ultralytics YOLO: huấn luyện và dự đoán object detection theo notebook của bài lab.
- Tài liệu trong repository bài lab: `README.md`, `GUIDE.md`, `RULES.md`, `RUBRIC.md`, `SUBMISSION.md` và notebook `notebooks/day2_annotation_to_yolo.ipynb`.