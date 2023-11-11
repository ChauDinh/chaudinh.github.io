---
layout: post
usehighlight: true
tags: [JavaScript, Data Structures, Algorithms, Coding Interview]
title: Recursion (Vietnamese) - Đệ quy
---

Bài này là tập hợp một số notes của mình trong quá trình ôn lại giải thuật Đệ quy trong lập trình nên có thể không phù hợp với các bạn chưa quen với khái niệm này.

Tuy nhiên, một số ví dụ trong này chắc chắn mọi người đã gặp ở đâu đó hoặc có thể nghĩ ra ngay ý tưởng giải do các bài toán minh hoạ cho giải thuật Đệ quy rất phổ biến
và được nhắc rất nhiều trong các giáo trình, blogs về cấu trúc dữ liệu và thuật toán.

Nếu bạn cảm thấy quen thuộc với phần lớn bài tập trong này, thì bài viết này mình nghĩ sẽ phù hợp với bạn.

# Định nghĩa

Trong toán học, khái niệm đệ quy (recursion) được định nghĩa rằng nếu `A` là một khái niệm mà khi định nghĩa `A` ta có sử dụng ngay chính khái niệm `A`.

Ta có thể lấy ví dụ định nghĩa về số tự nhiên như sau:

- 0 là một số tự nhiên
- `n` là một số tự nhiên nếu `n-1` là một số tự nhiên

<img src="https://drive.google.com/uc?id=1SqnqTt8eRcwOiyPzHWb4OXBRpO2vgwGZ" width="500"/>

Như hình trên, nếu xem lại định nghĩa về khái niệm số tự nhiên, chúng ta thấy phần cơ sở của Đệ quy chính là 0 là một số tự nhiên. Phần đệ quy là việc định nghĩa số `n` theo `n-1`. Khi `n` tiến về 0,
đó chính là phần cơ sở. Từ đây, suy ngược lên lại, ta thu được tính chất của `n` là một số tự nhiên theo định nghĩa.

Một ví dụ khác về đệ quy là giai thừa của số tự nhiên `n`. Ta định nghĩa `n! = 1` khi `n = 0` và `n! = n * (n - 1)!` khi `n > 0`.
Trong định nghĩa này, phần cơ sở là `n! = 1` khi `n = 0` và phần đệ quy chính là `n! = n * (n - 1)!`.

Ví dụ về giai thừa trên cũng được minh hoạ bằng code ở rất nhiều giáo trình, tại đây mình dùng JavaScript:

```JavaScript
const factorial = (n) => {
  if (n === 0) return 1;
  return n * factorial(n - 1);
}
```

Tuy nhiên còn một vấn đề quan trọng trong chương trình đệ quy nữa mà ta chưa nói đến là điều kiện dừng của chương trình đệ quy. Từ hình minh hoạ và ví dụ code về giai thừa số tự nhiên, ta có
thể thấy điều kiện dừng chính là khi `n = 0` hoặc khi quá trình đệ quy gọi đến phần cơ sở, nếu không, chương trình sẽ gọi đệ quy bất tận.

# Điều kiện dừng

Giải thuật đệ quy thông thường được xử lý bằng hai bước là bước phân rã và bước thay thế ngược.

- Phân rã là bước giải quyết bài toán đồng dạng với bài toán gốc nhưng có kích thước nhỏ hơn. Ví dụ `n! = n * (n - 1)!`, ta sẽ quy về giải bài toán có kích thước nhỏ hơn 1.
- Thay thế ngược là quá trình đặt điều kiện dừng (stopping condition). Ví dụ `0! = 1`. Khi có được giá trị này, chương trình sẽ thay thế ngược lại các lời gọi đệ quy trước đó
  cho đến khi gặp được giá trị ta cần tính và in ra kết quả.

Sau khi tìm hiểu xong lý thuyết cơ bản của đệ quy, chúng ta cùng nhau xem đệ quy có thể giải quyết được những dạng toán nào và có những dạng đệ quy nào trong thực tế.

# Các dạng đệ quy

- Đệ quy tuyến tính (linear recursion)
- Đệ quy nhị phân (binary recursion)
- Đệ quy phi tuyến, đệ quy lồng (nested recursion)
- Đệ quy hỗ tương (mutual recursion)

Đây là 4 loại thường gặp, một số loại đặc biệt khác sẽ được đề cập khi ta gặp các bài toán cụ thể bên dưới.
