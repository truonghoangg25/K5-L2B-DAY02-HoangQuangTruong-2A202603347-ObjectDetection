# Mini Guideline — Bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## 1. Định nghĩa lớp

| Lớp     | Định nghĩa của bạn                                      | Ví dụ / điểm phân biệt                                                          |
|---------|----------------------------------------------------------|----------------------------------------------------------------------------------|
| `car`   | Xe con dùng để chở người.                                | Sedan, SUV, taxi và pickup. Pickup được xếp vào `car`, không phải `truck`.       |
| `truck` | Xe tải chuyên chở hàng hóa.                              | Xe tải thùng, xe ben và xe đầu kéo; pickup được xếp vào `car`.                   |
| `bus`   | Xe buýt hoặc xe khách dùng để chở nhiều hành khách.      | Bao gồm minibus chở khách; không bao gồm van/minivan.                            |
| `van`   | Xe dạng van/minivan dùng để chở người hoặc hàng hóa.     | Van chở hàng, passenger van, minivan; không bao gồm bus.                         |

## 2. Quy tắc chung

- **Box bám sát vật thể nghĩa là: Bao quanh sát phần thân xe nhìn thấy được, hạn chế khoảng trống bên trong box và không cắt mất phần xe nhìn thấy.**
- **Vật thể bị che khuất: Vẫn gán nhãn nếu có thể nhận diện được loại xe; box chỉ bao quanh phần xe nhìn thấy.**
- **Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn nếu nhận diện được; box kéo sát đến rìa ảnh tại vị trí xe bị cắt.**
- **Vật thể rất nhỏ / mơ hồ: Không tự đoán nhãn nếu không thể xác định rõ loại xe; ghi nhận trường hợp để xem xét lại.** 

## 3. Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh         | Tình huống                                      | Quyết định cuối                          | Lý do theo schema                                                                                                  |
|-------------|-------------------------------------------------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `drive_14`  | Xe cứu hộ có hình dáng giống xe van             | `van`                                    | Dựa trên hình dáng thân xe dạng van và đặc điểm nhận diện của phương tiện; phân loại theo cấu trúc xe thay vì chức năng cứu hộ. |
| `drive_37`  | Xe tưới đường có hình dáng giống xe van         | `van`                                    | Dựa trên hình dáng thân xe dạng van; phân loại theo đặc điểm phương tiện thay vì chức năng tưới đường.             |
| `drive_42`  | Xe bus bị che mất quá nửa, chỉ nhìn thấy đuôi xe | `bus` nếu vẫn nhận diện được là xe buýt. | Schema cho phép gán nhãn xe bị che khuất nếu vẫn nhận diện được loại xe; box chỉ bao quanh phần xe nhìn thấy.     |
