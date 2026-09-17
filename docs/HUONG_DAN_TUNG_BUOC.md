# Hướng dẫn thực hành Day 5 Segmentation Lab

Tài liệu này là quy trình làm bài từ lúc nhận repository đến lúc nộp link fork. Hãy làm theo thứ tự, giữ lại các ZIP sau mỗi task và đánh dấu checklist. Bài được thực hành trên lớp trong 240 phút; link fork phải được nộp trên VLearn trong vòng 24 giờ sau buổi lab.

> **Nguyên tắc quan trọng:** làm đúng ảnh trong `data/`, dùng đúng class của từng task, chỉ gán nhãn phần nhìn thấy, luôn Save trước khi export và không sửa JSON/PNG bên trong ZIP bằng tay.

## 1. Kết quả cần nộp

Bài có 9 task, 14 ảnh và tổng tối đa 100 điểm. Mỗi task là một CVAT task riêng và phải dùng đúng format export.

| Task | Loại | Số ảnh | Class | Format CVAT | Điểm |
| --- | --- | ---: | ---: | --- | ---: |
| `easy_semantic` | Semantic | 3 | 5 | `Segmentation mask 1.1` | 20 |
| `medium_instance` | Instance | 3 | 6 | `COCO 1.0` | 32 |
| `hard_panoptic` | Panoptic | 2 | 12 | `COCO 1.0` | 30 |
| `cp1_holes` | Instance | 1 | 6 | `COCO 1.0` | 3 |
| `cp2_slice` | Instance | 1 | 6 | `COCO 1.0` | 3 |
| `cp5_occlusion` | Instance | 1 | 6 | `COCO 1.0` | 3 |
| `cp3_thin` | Semantic | 1 | 4 | `Segmentation mask 1.1` | 3 |
| `cp4_curb` | Semantic | 1 | 2 | `Segmentation mask 1.1` | 3 |
| `cp6_coverage` | Semantic | 1 | 7 | `Segmentation mask 1.1` | 3 |
| **Tổng** | | **14** | | | **100** |

Trong fork cá nhân cần có:

- `REPORT.md` ở thư mục gốc, đã điền bằng thông tin thật.
- Các ZIP export thật nằm trực tiếp trong `submissions/` và có tên đúng mã task.
- Link fork GitHub cá nhân được dán vào bài nộp trên VLearn.

Không bắt buộc dùng Python, Jupyter, Colab, SAM hoặc AI. Chỉ CVAT và GitHub cũng đủ để hoàn thành bài.

## 2. Đọc repository trước khi làm

Đừng lấy class từ trí nhớ hoặc dùng chung một file label cho mọi task. Các file có vai trò như sau:

| File/thư mục | Dùng để làm gì |
| --- | --- |
| `README.md` | Tổng quan, timebox, quy tắc nộp và giới hạn của các công cụ |
| `GUIDE.md` | Lộ trình 240 phút và tóm tắt từng tier |
| `guideline-mini-sheet.md` | Phiếu quy tắc gán nhãn cần mở trong lúc làm |
| `data/manifest.json` | Nguồn chuẩn về task, loại, đường dẫn và trọng số |
| `data/**/classes.json` | Class và metadata của đúng task; dùng để đối chiếu |
| `data/**/cvat-labels.json` | JSON dành cho CVAT **Labels → Raw** |
| `lab-guide.html` | Ảnh minh họa vị trí nút trong CVAT; ảnh screenshot không phải ảnh chấm |
| `REPORT.md` | File report phải điền và nộp ở gốc fork |
| `reports/REPORT_TEMPLATE.md` | Giải thích cách điền report, có ví dụ tham khảo |
| `scripts/inspect_submissions.py` | Kiểm hợp đồng ZIP, không chấm mask đúng/sai |
| `docs/SELF_SCORING.md` | Tự chấm sau khi người phụ trách phát reference |

`classes.json` và `cvat-labels.json` cùng nằm trong thư mục của task. Không dán nguyên `classes.json` vào ô Raw vì file này có thêm metadata (`type`, `trainid`, `colors`, ...), không phải danh sách label CVAT.

## 3. Chuẩn bị trước khi vẽ

### 3.1. Fork repository

1. Đăng nhập GitHub và mở repository đề bài của lớp.
2. Bấm **Fork** và chọn tài khoản cá nhân.
3. Làm bài trong fork, không làm trực tiếp trên repository gốc.
4. Kiểm tra fork có `data/`, `REPORT.md`, `submissions/` và các tài liệu cần thiết.
5. Có thể dùng **Download ZIP** hoặc clone fork về máy. Không cần clone nếu chỉ upload bằng giao diện GitHub.

### 3.2. Mở CVAT

1. Mở địa chỉ CVAT local do coach cung cấp. Khi chạy ngay trên máy thường là `http://localhost:8080`.
2. Dùng Chrome hoặc Edge.
3. Nếu không truy cập được, chụp màn hình lỗi, ghi địa chỉ và thời điểm đã thử rồi báo coach.
4. Không tự dựng một stack CVAT khác giữa giờ lab.
5. SAM là tùy chọn và có thể không có trên CVAT của lớp. Không thấy SAM thì dùng **Brush** hoặc **Polygon**.

### 3.3. Chuẩn bị bảng tra task

| Task | Thư mục ảnh | Class chính xác |
| --- | --- | --- |
| `easy_semantic` | `data/tiers/easy_semantic/images/` | `road`, `sidewalk`, `building`, `vegetation`, `sky` |
| `medium_instance` | `data/tiers/medium_instance/images/` | `person`, `bicycle`, `car`, `motorcycle`, `bus`, `truck` |
| `hard_panoptic` | `data/tiers/hard_panoptic/images/` | `road`, `sidewalk`, `building`, `vegetation`, `sky`, `person`, `car`, `bus`, `truck`, `motorcycle`, `bicycle`, `traffic light` |
| `cp1_holes` | `data/checkpoints/cp1_holes/images/` | Đọc `cp1_holes/classes.json` |
| `cp2_slice` | `data/checkpoints/cp2_slice/images/` | Đọc `cp2_slice/classes.json` |
| `cp5_occlusion` | `data/checkpoints/cp5_occlusion/images/` | Đọc `cp5_occlusion/classes.json` |
| `cp3_thin` | `data/checkpoints/cp3_thin/images/` | `pole`, `traffic sign`, `sky`, `road` |
| `cp4_curb` | `data/checkpoints/cp4_curb/images/` | `road`, `sidewalk` |
| `cp6_coverage` | `data/checkpoints/cp6_coverage/images/` | `road`, `sidewalk`, `building`, `vegetation`, `sky`, `car`, `person` |

Tên class phải giống từng ký tự. Ví dụ `traffic sign` có dấu cách; không đổi thành `traffic_sign`.

## 4. Hiểu semantic, instance và panoptic

### Semantic

Câu hỏi là: **pixel này thuộc loại vùng nào?** Các mảng cùng class không cần tách thành nhiều object. Dùng cho `easy_semantic`, `cp3_thin`, `cp4_curb`, `cp6_coverage`.

### Instance

Câu hỏi là: **pixel này thuộc vật nào?** Hai xe cùng class vẫn là hai object/mask riêng. Một vật bị che có thể có nhiều mảng nhìn thấy nhưng vẫn là một instance. Dùng cho `medium_instance`, `cp1_holes`, `cp2_slice`, `cp5_occlusion`.

### Panoptic

Panoptic cần cả vùng semantic và instance:

- **Stuff:** `road`, `sidewalk`, `building`, `vegetation`, `sky`; không đếm từng cá thể.
- **Thing:** `person`, `car`, `bus`, `truck`, `motorcycle`, `bicycle`, `traffic light`; mỗi vật là một instance riêng.
- Một mask `car` chung cho nhiều xe là sai panoptic.

## 5. Quy tắc hình học dùng cho mọi task

1. Chỉ vẽ phần nhìn thấy trong ảnh. Không tự đoán đường biên ở phía sau vật bị che.
2. Hai vật cùng class đứng sát nhau vẫn phải tách thành hai instance ở task instance/panoptic.
3. Một vật bị vật khác che có thể có các mảng nhìn thấy rời nhau nhưng vẫn là **một** instance.
4. Kính, cửa sổ hoặc khe trên vật không tự động là lỗ cần khoét. Với `cp1_holes`, cửa sổ/khe phải nằm trong mask theo quy tắc task.
5. Ranh `road`–`sidewalk` dựa vào chức năng và bó vỉa, không chỉ dựa vào màu; đây là trọng tâm của `cp4_curb`.
6. Bounding box không thay thế cho segmentation mask.
7. Không tô nền, bóng hoặc vùng không đủ bằng chứng chỉ để tăng coverage.
8. Khi không chắc, ghi ảnh/vị trí, hai cách hiểu, bằng chứng và quyết định vào `REPORT.md`.

## 6. Tạo một task trong CVAT

Lặp lại các bước dưới đây cho từng mã task. Mỗi task phải có một CVAT task riêng, không trộn ảnh hoặc label giữa các task.

### Bước 6.1. Tạo task và tải ảnh

1. Chọn **Tasks → Create new task**.
2. Đặt **Name** đúng mã, ví dụ `easy_semantic`.
3. Chọn đúng các file JPG trong thư mục `images/` của mã đó.
4. Kiểm số ảnh: Easy 3, Medium 3, Hard 2, mỗi checkpoint 1.
5. Không chọn ảnh từ tier/checkpoint khác.

Nếu coach đã tạo task sẵn, mở đúng task đó thay vì tạo bản thứ hai.

### Bước 6.2. Thêm label bằng Raw

1. Mở `data/tiers/<task>/cvat-labels.json` hoặc `data/checkpoints/<task>/cvat-labels.json` của **đúng task**.
2. Sao chép toàn bộ mảng JSON từ dấu `[` đầu đến dấu `]` cuối.
3. Trong CVAT mở **Labels → Raw**.
4. Dán nội dung, bấm **Done** rồi kiểm tra danh sách label.

Không paste `classes.json` vào Raw. Nếu task đã có annotation, không dán đè Raw vì có thể làm mất liên kết giữa annotation và label.

### Bước 6.3. Hoặc thêm label bằng Constructor

1. Chọn **Constructor → Add label**.
2. Gõ từng tên trong `classes.json`.
3. Bấm **Continue/Add** sau mỗi tên cho đến khi đủ.
4. Kiểm tra không có label thừa từ task trước.

### Bước 6.4. Kiểm tra trước khi Submit

- Tên task đúng mã.
- Đúng số lượng ảnh.
- Tên và số lượng label khớp `classes.json`.
- Giữ nguyên dấu cách, chữ thường và ký tự trong tên class.
- Không có label của task khác.

## 7. Vẽ và Save trong CVAT

### 7.1. Mở Job

1. Mở task và chọn Job.
2. Xác nhận tên task và tên ảnh.
3. Xác định ba khu vực: công cụ ở bên trái, ảnh ở giữa, danh sách `Objects` ở bên phải.
4. Chọn đúng label trước khi bắt đầu vẽ.

### 7.2. Vẽ bằng Polygon

1. Chọn **Polygon** và chế độ **Shape**, không chọn **Track**.
2. Chọn label.
3. Phóng to ảnh nếu biên nhỏ.
4. Nhấp các điểm theo đường biên phần nhìn thấy.
5. Đóng polygon để tạo object.
6. Kiểm object mới trong danh sách `Objects`: đúng label, đúng số lượng và không ăn nền.

### 7.3. Vẽ bằng Brush/Mask

1. Chọn **Brush/Mask**, chế độ **Shape** và label đúng.
2. Tô bên trong vùng/vật cần gán nhãn.
3. Giảm kích thước brush ở ranh hẹp hoặc chi tiết mảnh.
4. Dùng Eraser để xóa phần tràn nền hoặc lấn sang class khác.
5. Kết thúc mask theo nút kết thúc của giao diện.
6. Kiểm lại mask trong `Objects` và trên ảnh.

### 7.4. Quy trình kiểm một ảnh

Sau khi vẽ một ảnh, đi theo đúng thứ tự:

1. **Đúng ảnh:** có đang làm đúng JPG của task không?
2. **Đúng class:** label có khớp vật/vùng không?
3. **Đủ:** có vùng rõ ràng hoặc vật nào bị bỏ sót không?
4. **Đúng instance:** có gộp hai vật hoặc tách một vật không?
5. **Đúng biên:** mask có ăn nền, bóng hoặc vùng bị che không?
6. **Save:** bấm **Save**.
7. Đổi sang ảnh khác rồi quay lại ảnh vừa làm để chắc mask vẫn còn.

`Save` lưu annotation trong CVAT; `Export` tạo ZIP để nộp. Cần thực hiện cả hai.

## 8. Làm ba tier chính

### 8.1. `easy_semantic` — 20 điểm

1. Tạo task với 3 ảnh trong `data/tiers/easy_semantic/images/`.
2. Thêm đúng 5 class: `road`, `sidewalk`, `building`, `vegetation`, `sky`.
3. Vẽ các vùng lớn trước, sau đó phóng to kiểm ranh nhỏ.
4. Phân biệt road và sidewalk theo chức năng/bó vỉa, không chỉ theo màu.
5. Không tô `sky` xuyên qua mái nhà.
6. Không để `vegetation` tràn lên `building`.
7. Kiểm các mảng rõ ràng còn bỏ sót trên cả 3 ảnh.
8. Save, đổi ảnh, quay lại kiểm tra.
9. Chọn **Job → Export job dataset → Segmentation mask 1.1**.
10. Đổi tên ZIP bên ngoài thành `easy_semantic.zip`.

### 8.2. `medium_instance` — 32 điểm

1. Tạo task với 3 ảnh trong `data/tiers/medium_instance/images/`.
2. Thêm `person`, `bicycle`, `car`, `motorcycle`, `bus`, `truck`.
3. Trước khi xem bất kỳ gợi ý tự động nào, tự vẽ object đầu tiên.
4. Ghi ngay tên ảnh, vị trí, class và quy tắc chọn biên để điền mục 2 của report.
5. Nếu dùng gợi ý sau đó, kiểm và sửa vùng tràn nền, thiếu vùng, sai class, gộp vật hoặc tách vật.
6. Đếm từng vật trong mỗi ảnh.
7. Tạo một mask riêng cho mỗi vật.
8. Với vật bị che, chỉ vẽ phần nhìn thấy; không nối mask xuyên qua vật che.
9. Kiểm không gộp hai vật sát nhau và không tách một vật bị che thành hai object.
10. Save, đổi ảnh, quay lại kiểm tra `Objects`.
11. Export **COCO 1.0** và đổi tên thành `medium_instance.zip`.

### 8.3. `hard_panoptic` — 30 điểm

1. Tạo task với 2 ảnh trong `data/tiers/hard_panoptic/images/`.
2. Thêm đủ 12 class: `road`, `sidewalk`, `building`, `vegetation`, `sky`, `person`, `car`, `bus`, `truck`, `motorcycle`, `bicycle`, `traffic light`.
3. Xem toàn ảnh trước để lập danh sách stuff và thing.
4. Vẽ stuff: road, sidewalk, building, vegetation, sky.
5. Vẽ từng thing thành instance riêng; ví dụ hai xe phải là hai object `car`.
6. Chỉ vẽ phần thing nhìn thấy, không đoán phần bị che.
7. Xem lại toàn ảnh để tìm vùng stuff bỏ sót.
8. Phóng to kiểm lỗ, tràn biên, chồng lấn, khoảng trống và object bị gộp.
9. Save và kiểm cả hai ảnh.
10. Export **COCO 1.0** và đổi tên thành `hard_panoptic.zip`.

COCO hợp lệ chỉ chứng minh cấu trúc export. Nó không tự chứng minh panoptic đúng, không chứng minh stuff đã phủ đủ và không chứng minh các thing đã tách đúng.

## 9. Làm sáu checkpoint

Mỗi checkpoint có class và quy tắc riêng. Trước khi tạo task, mở `classes.json` và `cvat-labels.json` trong đúng thư mục checkpoint.

### `cp1_holes` — lỗ/kính, 3 điểm

- Ảnh: `data/checkpoints/cp1_holes/images/`.
- Loại: instance; export **COCO 1.0**.
- Kính/cửa sổ/khe nằm trong mask vật theo quy tắc task; không khoét lỗ tùy tiện.
- Kiểm class, biên và số object rồi Save.
- Đặt tên `cp1_holes.zip`.

### `cp2_slice` — hai vật sát nhau, 3 điểm

- Ảnh: `data/checkpoints/cp2_slice/images/`.
- Loại: instance; export **COCO 1.0**.
- Hai xe cùng class dù sát hoặc chạm nhau vẫn là hai instance.
- Tìm đường ranh giữa hai xe, tạo hai object và kiểm trong `Objects`.
- Đặt tên `cp2_slice.zip`.

### `cp5_occlusion` — che khuất, 3 điểm

- Ảnh: `data/checkpoints/cp5_occlusion/images/`.
- Loại: instance; export **COCO 1.0**.
- Các mảng nhìn thấy của cùng một vật bị che vẫn là một instance.
- Không vẽ xuyên qua vật che; không tách thành hai object chỉ vì mask bị gián đoạn.
- Đặt tên `cp5_occlusion.zip`.

### `cp3_thin` — nét mảnh, 3 điểm

- Ảnh: `data/checkpoints/cp3_thin/images/`.
- Class: `pole`, `traffic sign`, `sky`, `road`.
- Loại: semantic; export **Segmentation mask 1.1**.
- Phóng to cột/biển, dùng brush khoảng 2–3 px khi cần.
- Kiểm không bỏ sót nét mảnh và không tô dày lan sang nền.
- Đặt tên `cp3_thin.zip`.

### `cp4_curb` — ranh bó vỉa, 3 điểm

- Ảnh: `data/checkpoints/cp4_curb/images/`.
- Class: `road`, `sidewalk`.
- Loại: semantic; export **Segmentation mask 1.1**.
- Chọn ranh theo bó vỉa/chức năng sử dụng, không chọn chỉ vì hai vùng có màu khác nhau.
- Đặt tên `cp4_curb.zip`.

### `cp6_coverage` — phủ vùng, 3 điểm

- Ảnh: `data/checkpoints/cp6_coverage/images/`.
- Class: `road`, `sidewalk`, `building`, `vegetation`, `sky`, `car`, `person`.
- Loại: semantic; export **Segmentation mask 1.1**.
- Quét toàn ảnh để tìm mọi vùng thuộc class còn bỏ sót, kể cả xe/người.
- Sửa khe trống rõ ràng nhưng không tô bừa vùng không chắc.
- Đặt tên `cp6_coverage.zip`.

Nếu chưa làm kịp checkpoint, không tạo ZIP rỗng. Ghi `chưa có` và lý do trong report.

## 10. Export đúng và lưu file an toàn

Sau mỗi task:

1. Save trong CVAT.
2. Export từ Job bằng format được quy định trong bảng task.
3. Tải ZIP về máy.
4. Đổi **tên file ZIP bên ngoài**, không đổi file bên trong:

```text
submissions/easy_semantic.zip
submissions/medium_instance.zip
submissions/hard_panoptic.zip
submissions/cp1_holes.zip
submissions/cp2_slice.zip
submissions/cp5_occlusion.zip
submissions/cp3_thin.zip
submissions/cp4_curb.zip
submissions/cp6_coverage.zip
```

Không làm các việc sau:

- Không đổi `COCO 1.0` thành format khác vì “trông tương đương”.
- Không sửa JSON, polygon, RLE, PNG hoặc annotation id trong ZIP.
- Không tạo ZIP rỗng cho task chưa làm.
- Không đưa ảnh gốc, file tạm notebook hoặc reference vào `submissions/`.

Nếu CVAT không hiện format hoặc export lỗi, giữ annotation đã Save, chụp lỗi và báo coach. Không sửa ZIP bằng tay để vượt lỗi.

## 11. Tự kiểm trước khi nộp

### 11.1. Kiểm trong CVAT

Với từng task, kiểm theo danh sách:

- [ ] Đúng task, đúng ảnh và đúng số lượng ảnh.
- [ ] Đúng loại semantic/instance/panoptic.
- [ ] Tên label khớp `classes.json` của task đó.
- [ ] Đủ vùng/vật rõ ràng.
- [ ] Không gộp hai vật và không tách nhầm một vật.
- [ ] Mask chỉ phủ phần nhìn thấy, không ăn nền/bóng sai.
- [ ] Hard có cả stuff và từng thing.
- [ ] Đã Save, đổi ảnh rồi quay lại kiểm tra.
- [ ] Đã export đúng format.

### 11.2. Kiểm bằng script không cần ground truth

Nếu có Python 3.10+, mở terminal tại thư mục gốc repository và chạy:

```bash
python3 scripts/inspect_submissions.py --dir submissions
```

Trên Windows có thể dùng `py -3` thay cho `python3`.

Ý nghĩa kết quả:

- `OK`: ZIP đọc được và khớp hợp đồng cấu trúc ảnh/class/mask.
- `THIẾU`: chưa có ZIP; không phải lỗi nếu đã ghi trong report.
- `LỖI`: quay lại CVAT, sửa annotation, Save và export lại.
- `annotations` ở COCO: số mask bạn đã nộp, không phải số vật đúng.
- Cảnh báo panoptic: phải tự xem lại chồng lấn và phủ vùng trong CVAT.

Script không biết biên đúng, số object đúng, class đúng theo ground truth hay điểm. `OK` không có nghĩa là bài được 100 điểm.

Nếu không có Python, bỏ qua script và tự làm checklist trong CVAT; đây không phải điều kiện bắt buộc.

## 12. Điền `REPORT.md`

Mở `REPORT.md` ở gốc fork, thay các dấu `…` bằng thông tin thật. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Không tự điền điểm.

### Mục 1 — Bài đã nộp

Với từng task ghi:

- Tên ZIP đúng trong `submissions/`.
- Số ảnh đã vẽ và Save, ví dụ `2 / 3`.
- `chưa có` nếu chưa làm hoặc export lỗi.
- Nếu export lỗi, ghi dữ liệu đã Save đến đâu và đã báo coach thế nào.

Cột điểm trong report là điểm tối đa của task, không phải điểm tự chấm.

### Mục 2 — Một quyết định Medium trước gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem gợi ý tự động. Ghi:

- Tên ảnh và vị trí object.
- Class.
- Quy tắc khiến bạn dừng mask ở ranh đó.
- Nếu dùng gợi ý: vùng đúng/sai, đã sửa hay giữ và lý do.
- Nếu không dùng: ghi `không dùng` và vẫn giải thích một quyết định gán nhãn.

Ví dụ về mức chi tiết cần có: “Xe bên trái ở nửa dưới ảnh, class `car`; tôi chỉ vẽ phần thân xe nhìn thấy, dừng tại cột che vì không đoán biên phía sau.” Không chép ví dụ trong template thành câu trả lời của mình.

### Mục 3 — Một lỗi thật đã phát hiện và sửa

Ghi task/ảnh/vùng, loại lỗi, bằng chứng nhìn thấy, quy tắc đã dùng, hành động sửa và việc đã Save/export lại chưa. Ví dụ lỗi có thể là road-sidewalk sai ranh, hai xe bị gộp, vật bị che bị tách, mask ăn nền hoặc bỏ sót vùng.

Nếu đã xem GitHub Actions hoặc chạy scorer, chỉ ghi kết quả liên quan nếu có. Không tự ghi PASS, top 3, bonus hoặc điểm chính thức.

### Mục 4 — Ba ca chưa chắc hoặc đã cân nhắc

Mỗi dòng cần có:

1. Ảnh/vị trí cụ thể.
2. Hai cách hiểu có thể.
3. Dấu hiệu nhìn thấy hoặc quy tắc dùng.
4. Quyết định của bạn hoặc câu hỏi cụ thể cho coach.

Ca đã quyết định được cũng hợp lệ; không cần bịa ba lỗi.

## 13. Tự đánh giá sau khi có reference

Ground truth cho `easy_semantic`, `medium_instance` và `hard_panoptic` được phát trong 60 phút cuối theo hướng dẫn lớp. Trước thời điểm đó, scorer không thể tính điểm vì repository học viên không chứa đáp án. Không commit, push, upload lên Colab public, gửi lên VLearn hoặc chia sẻ lại các file reference.

### 13.1. GitHub Actions

1. Trên fork mở tab **Actions** và bật workflow nếu GitHub hỏi.
2. Push `REPORT.md` và các ZIP vào fork.
3. Mở **Actions → Day 5 self-check → lần chạy mới nhất → Summary**.
4. Trước khi release reference xuất hiện, Summary chỉ kiểm cấu trúc và ghi chưa có ground truth; không phải điểm 0.
5. Sau khi reference chính thức được phát, chọn **Run workflow** hoặc push ZIP mới.
6. Summary có thể hiện tự đánh giá ba tier tối đa **82 điểm**: Easy 20 + Medium 32 + Hard 30.
7. Nếu muốn sửa: sửa trong CVAT → Save → export lại đúng tên → upload/push → xem Summary mới.

Action lấy reference từ release chính thức của repository lớp và không đưa reference vào fork. Đây là phản hồi cá nhân, không phải điểm chính thức, PASS, bonus hay bảng xếp hạng.

### 13.2. Chạy scorer trên máy

Chỉ chạy khi đã có reference hợp lệ theo hướng dẫn của coach. Tại thư mục gốc:

```bash
python3 --version
python3 -m pip install -r requirements.txt
python3 scoring/score.py --list
python3 scoring/score.py easy_semantic submissions/easy_semantic.zip --group tiers
python3 scoring/score.py medium_instance submissions/medium_instance.zip --group tiers
python3 scoring/score.py hard_panoptic submissions/hard_panoptic.zip --group tiers
python3 scoring/scorecard.py --group tiers --dir submissions --out reports/tiers
```

Reference local có cấu trúc do lớp cung cấp, tương ứng với ảnh trong task. Nếu gặp `Protected reference missing`, chưa có reference đúng chỗ; đó không phải điểm 0. Nếu ảnh submission không khớp task, kiểm lại ZIP và task, không đổi tên ảnh để ép chạy.

Đọc metric như sau:

- Semantic: `per-class IoU` và `coverage`; xem lại class có IoU thấp.
- Instance: `mean matched IoU`, `P@0.5`, `R@0.5`, `FP`, `FN`; kiểm mask ăn nền, vật thừa hoặc bỏ sót.
- Panoptic: `PQ`, `SQ`, `RQ`; phân biệt lỗi biên với lỗi thiếu/thừa instance hoặc stuff.

`SCORECARD.md` của ba tier tối đa 82, không bao gồm 18 điểm checkpoint. Cờ `REVIEW SIGNALS` chỉ là tín hiệu cần người xem lại, không phải kết luận gian lận. Nếu sửa sau khi xem reference, phải ghi trung thực việc đó; không coi kết quả sau reference là bằng chứng độc lập trước khi phát đáp án.

## 14. Đưa bài lên fork và nộp VLearn

### Cách upload bằng giao diện GitHub

1. Mở `REPORT.md` trên fork, bấm biểu tượng bút chì, điền report rồi **Commit changes**.
2. Mở thư mục `submissions/`.
3. Chọn **Add file → Upload files**.
4. Upload các ZIP đúng tên, rồi **Commit changes**.
5. Mở lại fork ở chế độ người xem bình thường.
6. Kiểm `REPORT.md` đã điền, ZIP ở đúng thư mục và không có ground truth/file tạm.
7. Sao chép URL của **fork cá nhân**, không phải repository đề bài.
8. Dán URL fork vào bài Day 5 trên VLearn trước hạn 24 giờ.

### Cách dùng Git trên máy

Commit và push cũng được, miễn kết quả cuối cùng có cùng cấu trúc:

```text
REPORT.md
submissions/easy_semantic.zip
submissions/medium_instance.zip
submissions/hard_panoptic.zip
submissions/cp1_holes.zip
submissions/cp2_slice.zip
submissions/cp5_occlusion.zip
submissions/cp3_thin.zip
submissions/cp4_curb.zip
submissions/cp6_coverage.zip
```

Chỉ upload task đã hoàn thành. Task chưa làm phải được mô tả trong report, không tạo file rỗng.

## 15. Gói bài tùy chọn

Nếu cần một gói lưu/chuyển có manifest và SHA-256, chạy từ gốc repo:

```bash
python3 scripts/package_submission.py --learner-id D5_012
```

Thay `D5_012` bằng mã thật. Script lấy `REPORT.md` và các ZIP hợp lệ trong `submissions/`, tạo thêm một gói `day5-D5_012.zip` ở ngoài thư mục `submissions/`. Gói này không thay thế report và các ZIP riêng trên fork.

## 16. Timebox 240 phút

| Thời gian | Việc chính | Bằng chứng cần giữ |
| ---: | --- | --- |
| 0–15 | Fork, đọc quy tắc, mở CVAT, tạo Easy | Task đúng 3 ảnh và 5 class |
| 15–45 | Easy semantic, QC, Save/export | `easy_semantic.zip` |
| 45–110 | Medium instance; tự vẽ object đầu trước gợi ý | `medium_instance.zip`, ghi chú report |
| 110–120 | Nghỉ | Giữ an toàn ZIP đã xuất |
| 120–175 | Hard panoptic, QC, Save/export | `hard_panoptic.zip` |
| 175–185 | Nghỉ; nhận reference nếu lớp phát | Không đưa reference vào fork |
| 185–215 | Sáu checkpoint | ZIP của trạm đã làm |
| 215–235 | Kiểm ZIP, sửa nếu cần, điền report | `REPORT.md` hoàn chỉnh |
| 235–240 | Push, kiểm fork, ghi lỗi cần báo | Link fork sẵn sàng nộp |

Nếu chậm, ưu tiên theo thứ tự: **Save dữ liệu → export ZIP thật → kiểm tên/vị trí → điền report trung thực → làm task tiếp theo**. Không hy sinh Save/QC để tạo ZIP nhanh.

## 17. Checklist cuối cùng

- [ ] Tôi đang làm trên fork cá nhân.
- [ ] Tôi đã dùng đúng ảnh và đúng `classes.json` cho từng task.
- [ ] Tôi phân biệt đúng semantic, instance và panoptic.
- [ ] Tôi đã tự vẽ object Medium đầu tiên trước khi xem gợi ý.
- [ ] Tôi đã Save, đổi ảnh và quay lại kiểm tra.
- [ ] Tôi đã export đúng format của từng task.
- [ ] ZIP bên ngoài có tên đúng mã task.
- [ ] Tôi không sửa JSON/PNG bên trong ZIP.
- [ ] Tôi đã chạy `inspect_submissions.py` hoặc tự kiểm ZIP trong CVAT.
- [ ] Tôi đã điền đủ bốn mục trong `REPORT.md`.
- [ ] Tôi đã ghi rõ task chưa làm hoặc export lỗi.
- [ ] Tôi không đưa ground truth vào fork public.
- [ ] `REPORT.md` và ZIP nằm đúng vị trí trên fork.
- [ ] Tôi đã kiểm fork ở chế độ người xem.
- [ ] Tôi đã nộp link fork cá nhân trên VLearn trong vòng 24 giờ.

Khi bị kẹt, hãy báo coach bằng task, ảnh, bước đang làm, thông báo lỗi và ảnh chụp màn hình. Cách mô tả này giúp coach xử lý nhanh hơn so với chỉ báo “CVAT bị lỗi”.
