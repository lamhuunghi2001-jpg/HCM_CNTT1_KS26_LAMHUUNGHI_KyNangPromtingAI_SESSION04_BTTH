# BÀI TẬP THỰC HÀNH TỔNG HỢP
## SESSION 04 – KỸ THUẬT PROMPTING NÂNG CAO (LOGIC & CHUỖI)

### 1. Giới thiệu bài toán

**Tên bài: Xây dựng quy trình kiểm tra và tổng hợp dữ liệu đăng ký học phần.**

Trong quá trình đăng ký môn học, nhà trường phải nhận và kiểm tra nhiều thông tin của sinh viên. Tuy nhiên, dữ liệu đôi khi có thể bị thiếu, sai định dạng hoặc bị trùng lặp.

Nếu kiểm tra bằng tay sẽ mất nhiều thời gian. Vì vậy, em chọn sử dụng ChatGPT để hỗ trợ kiểm tra và tổng hợp dữ liệu theo từng bước.

### 2. Hướng giải quyết

Em chia công việc thành 4 bước để dễ thực hiện và kiểm tra kết quả.

| Bước | Công việc | Kết quả cần có |
|---|---|---|
| 1 | Nhận và làm sạch dữ liệu | Dữ liệu đã chuẩn hóa |
| 2 | Kiểm tra dữ liệu | Danh sách hợp lệ và có lỗi |
| 3 | Tổng hợp | Số lượng đăng ký từng môn |
| 4 | Kiểm tra kết quả | Báo cáo hoàn chỉnh |

**Sơ đồ thực hiện:**

```text
       DỮ LIỆU ĐẦU VÀO
               |
               v
       BƯỚC 1: LÀM SẠCH
               |
               v
       BƯỚC 2: KIỂM TRA
               |
         +-----+-----+
         |           |
      Hợp lệ       Có lỗi
         |           |
         v           v
       TỔNG HỢP    GHI LẠI LỖI
         |           |
         +-----+-----+
               |
               v
       BƯỚC 4: ĐỐI CHIẾU
               |
               v
          BÁO CÁO CUỐI
```

### 3. Prompt hệ thống

Đây là câu lệnh chung được sử dụng để hướng dẫn ChatGPT trong suốt quá trình thực hiện.

```text
Bạn là trợ lý hỗ trợ kiểm tra dữ liệu đăng ký học phần.

Yêu cầu:
- Chỉ sử dụng dữ liệu được cung cấp.
- Không tự thêm thông tin còn thiếu.
- Giữ lại dữ liệu ban đầu để đối chiếu.
- Thực hiện đúng thứ tự các bước.
- Ghi rõ những dữ liệu bị lỗi hoặc trùng lặp.
- Sử dụng bảng hoặc JSON để trình bày kết quả.
- Nếu một bước bị lỗi, hãy thông báo và không
  chuyển dữ liệu chưa kiểm tra sang bước tiếp theo.
- Không làm mất dữ liệu khi phải xử lý lại.

Cuối cùng, kiểm tra lại số liệu trước khi xuất báo cáo.
Trình bày bằng tiếng Việt, ngắn gọn và dễ hiểu.
```

### 4. Các Prompt thực hiện

#### Bước 1: Làm sạch dữ liệu

**Mục đích:** Sửa cách trình bày dữ liệu cho thống nhất, không tự ý thay đổi thông tin.

**Prompt:**

```text
Hãy giúp tôi làm sạch danh sách đăng ký học phần.

Thực hiện:
1. Chuyển mã sinh viên thành chữ in hoa.
2. Xóa khoảng trắng thừa.
3. Giữ nguyên mã học phần.
4. Không tự điền thông tin còn thiếu.
5. Giữ lại tất cả các dòng dữ liệu ban đầu.

Trả kết quả thành bảng.
Đồng thời ghi rõ tổng số dòng dữ liệu.
```

#### Bước 2: Kiểm tra dữ liệu

**Mục đích:** Tìm những dòng bị thiếu thông tin hoặc đăng ký trùng.

**Prompt:**

```text
Dựa vào dữ liệu đã làm sạch ở bước 1,
hãy kiểm tra các thông tin sau:

1. Mã sinh viên có bị thiếu không?
2. Mã học phần có bị thiếu không?
3. Có sinh viên nào đăng ký trùng cùng một học phần không?
4. Chia dữ liệu thành 3 nhóm:
   - Hợp lệ.
   - Không hợp lệ.
   - Trùng lặp.

Không tự sửa dữ liệu sai.
Hãy ghi rõ lý do của từng trường hợp.
```

#### Bước 3: Tổng hợp dữ liệu

**Mục đích:** Thống kê những đăng ký hợp lệ.

**Prompt:**

```text
Dựa vào danh sách hợp lệ ở bước 2,
hãy thực hiện:

1. Đếm tổng số lượt đăng ký hợp lệ.
2. Đếm số sinh viên có đăng ký hợp lệ.
3. Thống kê số lượt đăng ký của từng học phần.
4. Không tính dữ liệu thiếu và dữ liệu trùng.

Trình bày kết quả thành bảng.
Không tự tạo thêm số liệu.
```

#### Bước 4: Kiểm tra và xuất báo cáo

**Mục đích:** Kiểm tra lại các con số trước khi hoàn thành.

**Prompt:**

```text
Hãy kiểm tra kết quả của các bước trước.

Thực hiện:
1. Kiểm tra tổng số dòng ban đầu.
2. Đối chiếu số dòng hợp lệ, không hợp lệ và trùng lặp.
3. Kiểm tra tổng lượt đăng ký từng môn.
4. Nếu có số liệu không khớp, hãy chỉ ra vị trí sai.
5. Nếu tất cả đều đúng, xuất báo cáo cuối cùng.

Trình bày ngắn gọn bằng tiếng Việt.
Không tự sửa số liệu để làm cho kết quả khớp.
```

### 5. Áp dụng Chain-of-Thought

Với những bài toán có nhiều bước, em không yêu cầu ChatGPT đưa ra kết quả ngay mà chia yêu cầu thành từng phần để kiểm tra.

**Prompt sử dụng:**

```text
Hãy xử lý bài toán theo thứ tự:

Bước 1: Đếm số dòng dữ liệu.
Bước 2: Kiểm tra thông tin bị thiếu.
Bước 3: Tìm dữ liệu trùng lặp.
Bước 4: Tính số lượt đăng ký hợp lệ.
Bước 5: Kiểm tra lại kết quả.

Hãy trình bày kết quả và lý do ngắn gọn
cho từng bước, không bỏ qua bước kiểm tra.
```

Nhờ cách làm này, em có thể dễ dàng biết ChatGPT đang xử lý đến đâu và phát hiện sai sót ở từng bước.

### 6. Xử lý khi cuộc trò chuyện bị ngắt

Khi làm việc với dữ liệu dài, ChatGPT có thể dừng trả lời giữa chừng hoặc không còn nhớ đầy đủ những yêu cầu ban đầu.

Em sử dụng 3 cách đã học để xử lý tình huống này.

**6.1. Mớm câu để tiếp tục**

Không chỉ nhắn "Tiếp tục", em sẽ gửi lại một phần cuối của nội dung trước đó.

Prompt:

```text
Hãy tiếp tục từ đúng vị trí đang bị ngắt.

Nội dung cuối cùng trước khi dừng:
[DÁN PHẦN CUỐI CỦA CÂU TRẢ LỜI TRƯỚC]

Giữ nguyên cách trình bày và các yêu cầu ban đầu.
Không làm lại những phần đã hoàn thành.
```

**6.2. Tóm tắt nội dung đã làm**

Khi cuộc trò chuyện quá dài, em yêu cầu ChatGPT tóm tắt lại công việc.

Prompt:

```text
Hãy tóm tắt quá trình làm việc hiện tại gồm:

1. Công việc đang thực hiện.
2. Những bước đã hoàn thành.
3. Kết quả đã có.
4. Những lỗi đã phát hiện.
5. Công việc cần làm tiếp theo.

Viết ngắn gọn để có thể dùng lại
khi chuyển sang cuộc trò chuyện mới.
```

**6.3. Nhắc lại vai trò và yêu cầu**

Khi ChatGPT trả lời không đúng hướng, em nhắc lại nhiệm vụ ban đầu.

Prompt:

```text
Bạn tiếp tục hỗ trợ kiểm tra dữ liệu đăng ký học phần.

Hãy nhớ các yêu cầu:
- Không tự thêm dữ liệu.
- Không bỏ qua thông tin bị thiếu.
- Kiểm tra dữ liệu trùng lặp.
- Giữ đúng kết quả của các bước trước.
- Trình bày ngắn gọn bằng tiếng Việt.
```

### 7. Kết quả chạy thử

#### 7.1. Dữ liệu đầu vào

Em sử dụng một danh sách mẫu gồm 6 dòng dữ liệu:

| STT | Mã sinh viên | Mã học phần | Tên học phần |
|---|---|---|---|
| 1 | sv001 | IT101 | Lập trình C |
| 2 | SV002 | IT102 | Cơ sở dữ liệu |
| 3 | SV001 | IT101 | Lập trình C |
| 4 | SV003 | Trống | Mạng máy tính |
| 5 | SV004 | IT103 | Toán rời rạc |
| 6 | SV005 | IT102 | Cơ sở dữ liệu |

#### 7.2. Kết quả kiểm tra

Sau khi thực hiện các bước, kết quả như sau:

| STT | Mã sinh viên | Kết quả |
|---|---|---|
| 1 | SV001 | Hợp lệ |
| 2 | SV002 | Hợp lệ |
| 3 | SV001 | Trùng lặp |
| 4 | SV003 | Thiếu mã học phần |
| 5 | SV004 | Hợp lệ |
| 6 | SV005 | Hợp lệ |

#### 7.3. Kết quả tổng hợp

| Mã học phần | Tên học phần | Số lượt đăng ký |
|---|---|---:|
| IT101 | Lập trình C | 1 |
| IT102 | Cơ sở dữ liệu | 2 |
| IT103 | Toán rời rạc | 1 |
| **Tổng** | | **4** |

#### 7.4. Báo cáo cuối cùng

```text
BÁO CÁO ĐĂNG KÝ HỌC PHẦN

Tổng số dòng dữ liệu: 6
Số dòng hợp lệ: 4
Số dòng trùng lặp: 1
Số dòng không hợp lệ: 1

Tổng số lượt đăng ký hợp lệ: 4

Kết quả:
- IT101: 1 lượt
- IT102: 2 lượt
- IT103: 1 lượt

Kiểm tra:
4 + 1 + 1 = 6

Kết luận:
Dữ liệu đã được kiểm tra và tổng hợp.
Có 4 dòng hợp lệ được sử dụng để thống kê.
1 dòng trùng lặp và 1 dòng thiếu thông tin
được ghi lại để xử lý riêng.
```

### 8. Cách xử lý khi có lỗi

Trong quá trình thực hiện, em đưa ra một số trường hợp có thể xảy ra và cách giải quyết:

| Trường hợp | Cách giải quyết |
|---|---|
| Thiếu mã sinh viên | Đánh dấu không hợp lệ |
| Thiếu mã học phần | Ghi lại để kiểm tra |
| Đăng ký trùng | Giữ một dòng, ghi nhận dòng trùng |
| Dữ liệu bị rỗng | Yêu cầu nhập lại |
| ChatGPT trả sai định dạng | Yêu cầu trình bày lại |
| ChatGPT bị ngắt | Dùng cách mớm câu hoặc tóm tắt để tiếp tục |

Để tránh phải làm lại từ đầu, em sẽ lưu kết quả sau mỗi bước. Nếu gặp lỗi, em kiểm tra lại bước đó trước khi tiếp tục.

### 9. Nhận xét

Sau khi thực hiện bài tập, em nhận thấy:

- Chia công việc thành nhiều bước giúp dễ theo dõi hơn.
- Kiểm tra từng bước giúp phát hiện lỗi sớm.
- Dữ liệu được trình bày thành bảng nên dễ đối chiếu.
- Khi cuộc trò chuyện bị ngắt, có thể sử dụng phần tóm tắt để tiếp tục công việc.
- Việc sử dụng nhiều prompt giúp kiểm soát kết quả tốt hơn, nhưng cũng có thể mất thêm thời gian.

### 10. Kết luận

Qua bài thực hành, em đã hiểu rõ hơn về Chain-of-Thought, Prompt Chaining và cách xử lý khi cuộc trò chuyện bị ngắt.

Em đã áp dụng các kiến thức này vào bài toán kiểm tra dữ liệu đăng ký học phần. Thay vì yêu cầu ChatGPT làm tất cả trong một lần, em chia công việc thành nhiều bước và kiểm tra kết quả sau mỗi bước.

Theo em, cách làm này dễ thực hiện, dễ kiểm tra và có thể áp dụng vào những công việc khác trong quá trình học tập.