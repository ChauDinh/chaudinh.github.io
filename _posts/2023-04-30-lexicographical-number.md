---
layout: post
usehighlight: true
tags: [JavaScript, TypeScript, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: (Vietnamese) Sinh ra các sô tự nhiên sắp xêp theo thứ tự từ điển
---

Cho một số nguyên n, trả về tất cả các số trong phạm vi [1, n] được sắp xếp theo thứ tự từ điển.

Bạn hãy cố gắng giải quyết bài toán với độ phức tạp `O(n)` về thời gian và sử dụng `O(1)` bộ nhớ dùng thêm.

Luư ý: đây không phải là sắp xêp theo thứ tự tăng dân thông thường, theo thư tự từ điển thì số 10 sẽ được xếp truớc số 2.

Để giải quyết bài toán này với độ phức tạp O(n) về thời gian và sử dụng O(1) bộ nhớ dùng thêm, chúng ta có thể sử dụng phương pháp sinh các số trong phạm vi `[1, n]` theo thứ tự từ điển.

Các số được sắp xếp theo thứ tự từ điển theo quy tắc sau:

- So sánh từng chữ số của hai số để xác định số nào có giá trị lớn hơn ở chữ số đầu tiên. Nếu một số có giá trị lớn hơn, thì số đó được xếp sau.

- Nếu hai số có giá trị bằng nhau ở chữ số đầu tiên, chúng ta sẽ tiếp tục so sánh ở chữ số thứ hai. Chúng ta tiếp tục quy trình này cho đến khi chúng ta có thể xác định được số nào sẽ đứng trước.

- Nếu chúng ta đã so sánh hết tất cả các chữ số của hai số mà không thể xác định được số nào sẽ đứng trước, chúng ta sẽ xếp hai số theo thứ tự từ điển bất kỳ.

- Để thực hiện việc sinh các số theo thứ tự từ điển, chúng ta sẽ sử dụng một biến lưu giữ số hiện tại. Ban đầu, số này sẽ được gán bằng 1. Chúng ta sẽ in ra số hiện tại, sau đó tăng giá trị của số lên 1. Chúng ta tiếp tục quy trình này cho đến khi số hiện tại vượt quá giới hạn n.

```TypeScript
function lexicographicNumbers(n: number): number[] {
  const result: number[] = [];
  let currentNumber = 1;

  for (let i = 1; i <= n; i++) {
    result.push(currentNumber);

    if (currentNumber * 10 <= n) {
      currentNumber *= 10;
    } else {
      if (currentNumber >= n) {
        currentNumber = Math.floor(currentNumber / 10);
      }

      currentNumber++;

      while (currentNumber % 10 === 0) {
        currentNumber = Math.floor(currentNumber / 10);
      }
    }
  }

  return result;
}
```
Hàm `lexicographicNumbers` trả về một mảng chứa các số từ 1 đến n được sắp xếp theo thứ tự từ điển. Chúng ta sử dụng một mảng `result` để lưu trữ kết quả và một biến `currentNumber` để lưu trữ số hiện tại.

Chúng ta sử dụng vòng lặp `for` để duyệt qua các số từ 1 đến n. Trong mỗi vòng lặp, chúng ta thêm `currentNumber` vào mảng kết quả `result`.

Sau đó, chúng ta sử dụng các câu lệnh điều kiện để kiểm tra xem liệu chúng ta có thể tăng giá trị của `currentNumber` lên một mức độ mới hay không. Nếu có thể, chúng ta tăng `currentNumber` lên bằng cách nhân nó với 10. Nếu không, chúng ta cần tăng `currentNumber` lên một giá trị mới. Chúng ta sử dụng một vòng lặp `while` để loại bỏ các chữ số 0 thừa nếu có.

Cuối cùng, chúng ta trả về mảng result chứa các số được sắp xếp theo thứ tự từ điển.

Độ phức tạp thời gian của thuật toán này là `O(n)`, vì chúng ta chỉ cần thực hiện vòng lặp từ 1 đến n, và vòng lặp này chỉ thực hiện một số thao tác cơ bản như gán giá trị và in ra giá trị. Đồng thời, thuật toán này sử dụng `O(1)` bộ nhớ dùng thêm, vì chúng ta chỉ sử dụng một số biến cục bộ để lưu trữ thông tin.