# 🦆 Đua vịt trắc nghiệm

Game trắc nghiệm có đường đua vịt cho buổi thuyết trình. Người chơi vào sảnh chờ, người tổ chức bấm Bắt đầu, cả phòng cùng chơi trong 10 phút. Hết giờ tự công bố top 3.

## Các file
| File | Việc của nó |
|---|---|
| `index.html` | Giao diện và toàn bộ logic game (thường không cần sửa) |
| `questions.js` | **Câu hỏi**, thời gian, số phút của ván, tỉ lệ vật phẩm và hình phạt |
| `setup.html` | **Trang cài đặt**: kiểm tra Firebase và tạo link chủ trì, link người chơi |
| `config.js` | Cấu hình Firebase và tên phòng (không bắt buộc nếu dùng link từ setup.html) |
| `README.md` | Hướng dẫn này |

Các file `index.html`, `setup.html`, `questions.js`, `config.js` phải nằm cùng một thư mục.

## 1. Cài Firebase bằng trang setup.html (cách dễ nhất)
GitHub Pages chỉ chứa file tĩnh, nên cần Firebase (miễn phí) làm nơi lưu điểm và đồng hồ dùng chung. Không có nó thì máy bạn không thể báo cho máy người chơi biết đã bấm Bắt đầu.

1. Đưa các file lên GitHub Pages (mục 2).
2. Mở `https://<tên-github>.github.io/<tên-repo>/setup.html`.
3. Làm theo Bước 1 trên trang đó: tạo project Firebase, tạo Realtime Database, dán luật, copy khối `firebaseConfig`.
4. Dán khối đó vào Bước 2 rồi bấm **Kiểm tra kết nối**. Trang tự thử đọc và ghi thật, báo ✅ hoặc nói rõ sai ở đâu, và tự dò `databaseURL` nếu bạn dán thiếu.
5. Trang tạo sẵn **link chủ trì** (có nút BẮT ĐẦU) và **link người chơi**. Hai link này đã chứa sẵn cấu hình nên **không cần sửa `config.js`**.
6. Mỗi buổi thuyết trình đổi tên phòng ở Bước 2 và kiểm tra lại để lấy link mới, điểm cũ sẽ không bị lẫn.

Cách cũ vẫn dùng được: điền giá trị vào `config.js`. Link có cấu hình sẽ ưu tiên hơn `config.js`.

Lưu ý: luật Firebase cho phép ai có link đều ghi được điểm, phù hợp game vui trong buổi thuyết trình, không dùng cho thi cử nghiêm túc. Giá trị `apiKey` của Firebase web không phải mật khẩu nên đưa vào link được.

## 2. Đưa lên GitHub Pages
1. Tạo repo, tải cả 4 file `index.html`, `setup.html`, `questions.js`, `config.js` lên nhánh `main` (thư mục gốc).
2. **Settings → Pages → Deploy from a branch → main / (root)**.
3. Link game: `https://<tên-github>.github.io/<tên-repo>/`
4. Mỗi lần sửa file, chờ khoảng 1 phút rồi bấm Ctrl+F5 khi mở lại link. Nếu vẫn thấy bản cũ, mở bằng cửa sổ ẩn danh hoặc thêm `?v=2` vào cuối link.

## 3. Cách chơi trong buổi thuyết trình
- **Bạn (chủ trì)** mở **link chủ trì** do setup.html tạo (dạng `.../index.html?host=1#cfg=...`) trên máy chiếu. Màn hình này hiện đường đua cỡ lớn, số người đã vào, nút **▶ BẮT ĐẦU** và nút **🔄 Ván mới**.
- **Người nghe** mở **link người chơi** do setup.html tạo (không có `?host=1`), nhập tên, bấm **Xuống nước!** và vào **sảnh chờ**.
- Khi bạn bấm **▶ BẮT ĐẦU**, tất cả cùng vào câu đầu tiên, đồng hồ đếm ngược `GAME_MINUTES` phút (mặc định 10).
- Hết giờ, mọi màn hình tự hiện bục top 3 🥇🥈🥉 và bảng điểm còn lại.
- Game **không bao giờ tự cho người chơi vào** trước khi bạn bấm Bắt đầu. Ai vào khi ván đang chạy sẽ thấy nút "Vào chơi ngay" để chơi phần thời gian còn lại. Người vào sau khi hết giờ không chơi được.
- Giữa các lần thuyết trình bấm **🔄 Ván mới** để xóa người chơi và điểm, mọi người quay về sảnh chờ (phải nhập tên lại).
- Ai có link `?host=1` cũng bấm được 2 nút của host, nên chỉ gửi link thường cho người nghe.
- Thử nhanh: thêm `?min=1` vào link để ván chỉ dài 1 phút.

## 4. Cách đổi câu hỏi
Mở `questions.js`, mỗi câu hỏi là một dòng:

    { q: "Nội dung câu hỏi?", a: ["Đáp án A", "Đáp án B", "Đáp án C", "Đáp án D"], c: 2 },

- `q`: câu hỏi. `a`: các đáp án. `c`: vị trí đáp án đúng, đếm từ 0 (0 = A, 1 = B, 2 = C, 3 = D).
- Thêm câu: chép nguyên một dòng rồi dán xuống dưới, giữ dấu phẩy cuối dòng. Xóa câu: xóa cả dòng.
- Nếu nội dung có dấu nháy kép `"` bên trong, hãy dùng nháy đơn `'` để tránh lỗi.
- Sửa ngay trên GitHub: mở file → biểu tượng cây bút ✏️ → sửa → **Commit changes**.

## 5. Luật chơi
- Trả lời đúng +100 điểm, trả lời nhanh được thưởng thêm tối đa +50.
- Đường đua chỉ hiện `TOP_DUCKS` (mặc định 5) vịt dẫn đầu, không có vạch đích, người dẫn đầu ở xa nhất và có 👑. Người chơi ngoài top 5 vẫn thấy hạng của mình (ví dụ "Hạng 12/40").

**Vật phẩm may mắn khi trả lời ĐÚNG** (xác suất `POWERUP_CHANCE`, mặc định 50%):
- ⚡ Nhân đôi điểm câu kế tiếp.
- 💡 Gợi ý 50/50: loại bớt 2 đáp án sai ở câu kế tiếp.
- 💰 Thưởng nóng: cộng ngay `BONUS_POINTS` điểm.
- 🏴‍☠️ Cướp `STEAL_PERCENT` (10%) điểm của người bạn chọn.
- 🎯 Bắn hạ ngôi sao: tự động cướp điểm của người dẫn đầu.
- 🛡️ Khiên: chặn 1 lần bị cướp điểm hoặc bị phạt.

**Hình phạt may rủi khi trả lời SAI hoặc hết giờ** (xác suất `PENALTY_CHANCE`, mặc định 40%):
- 💧 Rớt xuống nước: mất `PENALTY_PERCENT` (10%) điểm của mình.
- 🧧 Phát lì xì: tặng `GIFT_PERCENT` (8%) điểm cho một người ngẫu nhiên.
- 🐌 Vịt chậm: điểm câu kế tiếp bị chia đôi.
- ⏳ Thời gian cấp bách: câu kế tiếp chỉ còn 7 giây.
- 🧊 Đóng băng: câu kế tiếp phải chờ 5 giây mới chọn được đáp án.

Mọi con số trên chỉnh ở cuối file `questions.js`. Nếu người chơi trả lời hết câu hỏi sớm, họ chờ đến hết giờ. Muốn họ chơi lặp lại câu hỏi cho đến hết giờ, đặt `REPEAT_QUESTIONS = true`.

## Chạy thử trên máy
Game dùng ES module nên không mở trực tiếp file bằng cách nhấp đúp được. Chạy server tĩnh trong thư mục chứa file:

    npx serve .        # hoặc: python3 -m http.server

Khi chưa cấu hình Firebase, game chỉ hiện khung cảnh báo đỏ và khóa nút, không chạy được.

## Xử lý sự cố
- **Thấy khung đỏ "CHƯA KẾT NỐI FIREBASE"**: `config.js` vẫn còn giá trị mẫu. Chưa có sảnh chờ nhiều người. Điền Firebase theo mục 1. Khi chưa kết nối, nút vào chơi và nút của host bị khóa, không có chế độ chơi thử.
- **Khung đỏ "Không kết nối được Firebase"**: kiểm tra `databaseURL` trong `config.js` và chắc chắn đã bấm **Publish** ở tab Rules.
- **Thấy dữ liệu cũ từ lần thử trước**: dùng cùng một Firebase và cùng `ROOM` sẽ thấy lại người chơi cũ. Bấm **🔄 Ván mới** ở màn hình host hoặc đổi `ROOM`.
- **Sửa file mà web không đổi**: chờ 1 đến 2 phút (xem tab Actions có dấu tích xanh), rồi Ctrl+Shift+R hoặc mở ẩn danh.
