# Hiểu về Tương Tác và Trạng Thái trong React

### Giới thiệu

# 

Chào mừng bạn đến với React! Nếu bạn là người mới bắt đầu, tài liệu này sẽ là kim chỉ nam giúp bạn khám phá cách các ứng dụng React trở nên "sống động". Chúng ta sẽ cùng tìm hiểu cách ứng dụng phản hồi lại tương tác của người dùng và quản lý những dữ liệu thay đổi theo thời gian.

Mục tiêu chính của tài liệu này là giúp bạn hiểu rõ hai khái niệm nền tảng, xương sống của mọi ứng dụng React: `**props**` (thuộc tính) và `**state**` (trạng thái). Nắm vững hai khái niệm này là bước đầu tiên để xây dựng những giao diện người dùng mạnh mẽ và có tính tương tác cao.

\--------------------------------------------------------------------------------

## 1\. Cách React "Suy Nghĩ": Giao diện Khai báo

### 1.1. Lập trình Khai báo vs. Mệnh lệnh

# 

Trước khi đi sâu vào chi tiết, điều quan trọng là phải hiểu triết lý cốt lõi của React. Sự khác biệt chính nằm ở cách chúng ta yêu cầu giao diện người dùng (UI) thay đổi.

1.  **Lập trình Mệnh lệnh (Imperative):** Bạn ra lệnh cho máy tính _cách_ thực hiện một việc gì đó, từng bước một.
2.  **Lập trình Khai báo (Declarative):** Bạn mô tả _kết quả_ bạn muốn, và để máy tính tự tìm ra cách thực hiện.

Hãy tưởng tượng bạn đang bắt một chiếc taxi.

-   **Mệnh lệnh:** Bạn sẽ chỉ đường chi tiết cho tài xế: "Đi thẳng, đến ngã tư rẽ trái, đi qua hai đèn đỏ rồi rẽ phải..." Bạn đang kiểm soát từng bước đi.
-   **Khai báo:** Bạn chỉ cần nói với tài xế: "Hãy đưa tôi đến Sân bay Tân Sơn Nhất". Bạn mô tả đích đến, còn việc chọn con đường nào là của tài xế.

React tuân theo triết lý **khai báo**. Thay vì ra lệnh trực tiếp cho giao diện "bật nút này", "ẩn phần kia", bạn chỉ cần mô tả giao diện nên trông như thế nào ở các trạng thái khác nhau. React sẽ lo phần còn lại, tự động cập nhật giao diện để khớp với những gì bạn đã khai báo.

### 1.2. Component và `props`: Truyền dữ liệu xuống

# 

Giao diện người dùng trong React được xây dựng từ những khối nhỏ, độc lập và có thể tái sử dụng gọi là `**Component**`. Các component này giao tiếp với nhau để tạo nên một ứng dụng hoàn chỉnh.

Vậy làm thế nào để một component cha truyền dữ liệu cho component con của nó? Câu trả lời là `**props**`.

-   `props` (viết tắt của properties - thuộc tính) là cách để truyền thông tin từ component cha xuống component con.
-   Chúng hoạt động tương tự như các thuộc tính trong HTML (ví dụ: `src`, `alt` trong thẻ `<img>`).
-   Bạn có thể truyền bất kỳ giá trị JavaScript nào qua props, bao gồm đối tượng, mảng, và thậm chí cả hàm.

**Ví dụ:** Component `GreeterApp` (cha) truyền một prop `message` cho component `Greeting` (con).

    // Component cha
    function GreeterApp() {
      return (
        <div>
          <Greeting message="Chào mừng bạn đến với React!" />
          <Greeting message="Hãy cùng khám phá tương tác nhé!" />
        </div>
      );
    }
    
    // Component con
    function Greeting({ message }) { // Đọc prop 'message'
      return (
        <h1>{ message }</h1>
      );
    }
    

\--------------------------------------------------------------------------------

## 2\. Lắng Nghe Người Dùng: Xử Lý Sự Kiện

### 2.1. Thêm Trình Xử lý Sự kiện (Event Handler)

# 

Để một component có thể phản hồi lại tương tác (nhấp chuột, di chuột, nhập liệu), bạn cần thêm các **trình xử lý sự kiện** vào JSX.

-   Các trình xử lý sự kiện là những hàm do chính bạn định nghĩa.
-   Chúng thường được đặt bên trong component của bạn.
-   Bạn truyền hàm này vào một prop sự kiện đặc biệt, ví dụ như `onClick` cho một nút bấm.

    function AlertButton() {
      // 1. Định nghĩa một hàm xử lý sự kiện
      function handleClick() {
        alert('Bạn đã nhấp vào tôi!');
      }
    
      return (
        // 2. Truyền hàm đó cho prop onClick
        <button onClick={handleClick}>
          Nhấp vào tôi
        </button>
      );
    }
    

### 2.2. Sai Lầm Phổ Biến: Truyền Hàm vs. Gọi Hàm

# 

Một trong những lỗi phổ biến nhất đối với người mới bắt đầu là vô tình _gọi_ hàm xử lý sự kiện thay vì _truyền_ nó.

| Cú pháp Đúng (Truyền hàm) | Cú pháp Sai (Gọi hàm) |
| --- | --- |
| `onClick={handleClick}` | `onClick={handleClick()}` |
| **Giải thích:** React sẽ ghi nhớ hàm này và chỉ gọi nó khi người dùng nhấp vào nút. | **Giải thích:** Dấu `()` ở cuối sẽ khiến hàm này được thực thi _ngay lập tức_ mỗi khi component render, không cần chờ người dùng nhấp. |

### 2.3. Truyền Trình Xử lý Sự kiện qua `props`

# 

Một kỹ thuật rất mạnh mẽ trong React là component cha có thể định nghĩa hành vi và truyền nó xuống cho các component con. Thay vì truyền dữ liệu, bạn có thể truyền một hàm xử lý sự kiện dưới dạng prop.

Ví dụ dưới đây cho thấy component `Toolbar` (cha) định nghĩa các hành vi cụ thể như `handlePlayMovie`. Sau đó, nó truyền các hàm này cho các component `AlertButton` (con) chung chung. `AlertButton` không cần biết hàm đó làm gì, nó chỉ gọi hàm được truyền qua prop `onSmash` của nó.

    // Component cha định nghĩa hành vi
    function Toolbar() {
      const handlePlayMovie = () => {
        alert('Đang phát phim!');
      };
    
      const handleUploadImage = () => {
        alert('Đang tải ảnh lên!');
      };
    
      return (
        <div>
          <AlertButton onSmash={handlePlayMovie} message="Phát Phim" />
          <AlertButton onSmash={handleUploadImage} message="Tải Ảnh Lên" />
        </div>
      );
    }
    
    // Component con có thể tái sử dụng, nhận hành vi qua props
    function AlertButton({ onSmash, message }) {
      return (
        <button onClick={onSmash}>
          {message}
        </button>
      );
    }
    

\--------------------------------------------------------------------------------

## 3\. Bộ Nhớ Của Component: Giới Thiệu `state`

### 3.1. Tại sao chúng ta cần `state`?

# 

Các component cần "ghi nhớ" những thông tin thay đổi do tương tác, chẳng hạn như hình ảnh hiện tại trong một carousel, nội dung trong một ô nhập liệu, hoặc các sản phẩm trong giỏ hàng. Trong React, loại bộ nhớ dành riêng cho component này được gọi là `**state**` (trạng thái).

Các biến cục bộ thông thường không thể làm được điều này vì hai lý do:

1.  **Biến cục bộ không tồn tại giữa các lần render.** Khi React render lại component, nó sẽ thực thi hàm component từ đầu, và mọi thay đổi đối với biến cục bộ sẽ bị mất.
2.  **Việc thay đổi biến cục bộ không kích hoạt render lại.** React không nhận ra rằng nó cần phải render lại component với dữ liệu mới.

Để cập nhật giao diện, chúng ta cần hai thứ:

1.  **Giữ lại** dữ liệu giữa các lần render.
2.  **Kích hoạt** React render lại component với dữ liệu mới.

### 3.2. Gặp gỡ Hook đầu tiên: `useState`

# 

`useState` là một **Hook** của React cung cấp cả hai khả năng trên. _Hook_ là những hàm đặc biệt cho phép component của bạn "kết nối" vào các tính năng của React.

Để sử dụng `useState`, bạn cần `import` nó từ React. `import { useState } from 'react';`

Khi bạn gọi `useState`, bạn truyền vào giá trị ban đầu (initial value). Nó trả về một mảng chứa hai phần tử:

-   **Biến trạng thái (state variable):** (ví dụ: `score`) Giá trị hiện tại của trạng thái cho lần render này.
-   **Hàm cập nhật trạng thái (setter function):** (ví dụ: `setScore`) Một hàm cho phép bạn cập nhật trạng thái và kích hoạt một lần render mới.

Dưới đây là một component `Counter` sử dụng `useState` để theo dõi số lần nhấp chuột.

    import { useState } from 'react';
    
    function Counter() {
      // Khai báo một biến trạng thái tên là "score" với giá trị ban đầu là 0
      const [score, setScore] = useState(0);
    
      function increment() {
        // Gọi hàm cập nhật để yêu cầu render lại với giá trị mới
        setScore(score + 1);
      }
    
      return (
        <>
          <button onClick={() => increment()}>+1</button>
          <h1>Điểm: {score}</h1>
        </>
      );
    }
    

### 3.3. `State` là riêng tư và độc lập

# 

`State` là cục bộ và riêng tư đối với mỗi instance (bản sao) của component trên màn hình.

Nếu bạn render cùng một component hai lần, mỗi bản sao sẽ có `state` hoàn toàn độc lập và không ảnh hưởng đến nhau. Hãy thử nhấp vào các bộ đếm dưới đây; bạn sẽ thấy chúng được cập nhật riêng biệt.

    function App() {
      return (
        <div>
          <Counter />
          <Counter />
        </div>
      )
    }
    

\--------------------------------------------------------------------------------

## 4\. Vòng Đời Cập Nhật Giao Diện

### 4.1. Ba Bước Cập Nhật: Kích hoạt, Render, và Commit

# 

Quá trình hiển thị component của bạn lên màn hình diễn ra qua ba bước. Hãy tưởng tượng các component của bạn là những đầu bếp trong bếp, chuẩn bị những món ăn ngon từ nguyên liệu. Trong kịch bản này, React là người bồi bàn nhận yêu cầu từ khách hàng và mang đến cho họ đơn đặt hàng.

Quá trình yêu cầu và phục vụ giao diện này có ba bước:

1.  **Kích hoạt (Triggering):** Một yêu cầu render được đưa ra. Điều này có thể xảy ra khi ứng dụng khởi động lần đầu hoặc khi trạng thái thay đổi (ví dụ: do gọi hàm `set...`). _Giống như khách hàng đặt món ăn._
2.  **Render:** React gọi các component của bạn để xác định xem giao diện nên trông như thế nào. _Giống như đầu bếp chuẩn bị món ăn trong bếp._ Đây là quá trình "tính toán" và không làm thay đổi bất cứ thứ gì đã có.
3.  **Commit:** React áp dụng các thay đổi đã tính toán lên DOM (cấu trúc của trang web). _Giống như người bồi bàn đặt món ăn lên bàn._ React chỉ thay đổi những phần DOM thực sự cần thiết.

### 4.2. `State` như một "Ảnh Chụp Nhanh" (Snapshot)

# 

Một khái niệm cốt lõi cần hiểu là: **việc thiết lập** `**state**` **không thay đổi biến** `**state**` **bạn đang có trong lần render hiện tại**. Thay vào đó, nó kích hoạt một lần render mới với giá trị `state` mới.

React `state` hoạt động giống như một "ảnh chụp nhanh". Hãy xem xét ví dụ về nút "+3" này.

    import { useState } from 'react';
    
    function Counter() {
      const [number, setNumber] = useState(0);
    
      return (
        <>
          <h1>{number}</h1>
          <button onClick={() => {
            setNumber(number + 1);
            setNumber(number + 1);
            setNumber(number + 1);
          }}>+3</button>
        </>
      )
    }
    

Trong lần render đầu tiên, `number` là `0`. Khi bạn nhấp vào nút:

-   Trong trình xử lý sự kiện `onClick` của lần render đó, giá trị của `number` _luôn luôn_ là `0`, bất kể bạn gọi `setNumber` bao nhiêu lần.
-   Mỗi lệnh gọi `setNumber(0 + 1)` đều yêu cầu React chuẩn bị một lần render mới với `number` là `1`.

Bảng sau đây làm rõ điều gì xảy ra:

| Lệnh được gọi | `number` trong handler | React làm gì |
| --- | --- | --- |
| `setNumber(number + 1)` | `0` | Chuẩn bị thay đổi `number` thành `1` trong lần render tiếp theo. |
| `setNumber(number + 1)` | `0` | Chuẩn bị thay đổi `number` thành `1` trong lần render tiếp theo. |
| `setNumber(number + 1)` | `0` | Chuẩn bị thay đổi `number` thành `1` trong lần render tiếp theo. |

Đây là lý do tại sao sau một cú nhấp chuột, `number` chỉ trở thành `1` thay vì `3`. `state` dường như không cập nhật "ngay lập tức" bên trong các trình xử lý sự kiện vì nó hoạt động dựa trên ảnh chụp nhanh của lần render đó.

\--------------------------------------------------------------------------------

## 5\. Quy Tắc Vàng: Coi `state` là Bất Biến (Immutable)

# 

Bạn không nên thay đổi trực tiếp các object và array mà bạn giữ trong React state. Thay vào đó, khi bạn muốn cập nhật một object hoặc array, bạn cần **tạo một cái mới** (hoặc tạo một bản sao của cái hiện có), rồi cập nhật `state` để sử dụng bản sao đó.

### 5.1. Tại sao không nên thay đổi trực tiếp `state`?

# 

Thay đổi trực tiếp dữ liệu trong `state` được gọi là **mutation**.

    // 🔴 Sai: Thay đổi trực tiếp một object trong state
    person.firstName = 'Alice';
    

Bạn nên coi mọi object và array trong `state` của React là **chỉ đọc (read-only)**. Việc thay đổi trực tiếp `state` có thể dẫn đến các lỗi khó gỡ, các vấn đề về hiệu suất, và khiến component không render lại như mong đợi. Điều này xảy ra vì React so sánh các 'ảnh chụp nhanh' của state; nếu bạn chỉ thay đổi nội dung bên trong của cùng một object, React sẽ thấy object cũ và mới là một và sẽ bỏ qua việc render lại.

### 5.2. Cập nhật Object trong `state`

# 

Để cập nhật một object, hãy luôn tạo một object mới. Cú pháp spread `...` rất hữu ích để sao chép tất cả các thuộc tính từ object cũ sang object mới.

-   **Trước (Sai):** Thay đổi trực tiếp thuộc tính. React sẽ không nhận ra sự thay đổi này và có thể sẽ không render lại.
-   **Sau (Đúng):** Tạo một object mới với các thuộc tính được sao chép và ghi đè thuộc tính cần thay đổi.

### 5.3. Cập nhật Array trong `state`

# 

Tương tự như object, bạn nên tránh các phương thức làm thay đổi array gốc như `push()`, `pop()`, hoặc `splice()`. Thay vào đó, hãy sử dụng các phương thức tạo ra một array mới.

-   **Thêm một phần tử:** Sử dụng cú pháp spread `[...arr, newItem]`.
-   **Xóa một phần tử:** Sử dụng phương thức `filter()`, nó sẽ trả về một array mới không chứa phần tử đã bị lọc.
-   **Thay đổi một phần tử:** Sử dụng phương thức `map()`. `map()` tạo ra một array mới, nơi bạn có thể trả về một item đã được thay đổi hoặc giữ nguyên item cũ.

\--------------------------------------------------------------------------------

## 6\. Tổng Kết: Dòng Chảy Dữ Liệu Hoàn Chỉnh

# 

Bây giờ bạn đã hiểu cách React xử lý tương tác và cập nhật giao diện. Toàn bộ quy trình có thể được tóm tắt thành một chu trình rõ ràng, mạch lạc:

1.  Người dùng thực hiện một hành động (ví dụ: nhấp chuột vào một nút).
2.  React gọi hàm **trình xử lý sự kiện** tương ứng mà bạn đã định nghĩa (ví dụ: `handleClick`).
3.  Trình xử lý sự kiện của bạn gọi **hàm cập nhật** `**state**` (ví dụ: `setSomething(...)`) với một giá trị _mới_. Việc truyền một giá trị mới này sẽ yêu cầu React lên lịch một lần render lại.
4.  React xử lý yêu cầu và **render lại** component. Trong lần render này, `useState` sẽ trả về "ảnh chụp nhanh" của `state` mới.
5.  React tính toán các thay đổi và **cập nhật DOM** để phản ánh giao diện người dùng mới.
6.  Chu trình hoàn tất và chờ đợi tương tác tiếp theo từ người dùng.

Việc nắm vững luồng dữ liệu một chiều này là chìa khóa để xây dựng các ứng dụng React mạnh mẽ, dễ dự đoán và dễ dàng gỡ lỗi. Chúc mừng bạn đã hoàn thành những bước đầu tiên quan trọng nhất!