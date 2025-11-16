# Báo cáo Kỹ thuật Chuyên sâu về các Khái niệm Cốt lõi và Phương pháp Tối ưu trong React

## 1.0 Giới thiệu: Vượt qua những Điều cơ bản

# 

Tài liệu này cung cấp một phân tích kỹ thuật sâu sắc về các cơ chế cốt lõi của React, được thiết kế cho các kiến trúc sư và nhà phát triển cao cấp muốn xây dựng các ứng dụng phức tạp một cách tự tin và hiệu quả. Mục tiêu của chúng tôi không chỉ là giải thích "cái gì" mà còn làm sáng tỏ "tại sao" đằng sau các quyết định kiến trúc của React.

Báo cáo này được cấu trúc theo một mạch truyện rõ ràng: xây dựng các ứng dụng mạnh mẽ bằng cách hiểu rõ lõi "thuần túy" của React, quản lý "bộ nhớ" của nó (**state**) một cách chiến lược, và biết khi nào và làm thế nào để "phá vỡ quy tắc" một cách có chủ đích thông qua các "lối thoát" (escape hatches). Bằng cách nắm vững các khái niệm này, chúng ta có thể khai thác toàn bộ sức mạnh của React để xây dựng các ứng dụng có hiệu suất cao, dễ bảo trì và có khả năng mở rộng.

Chúng ta sẽ bắt đầu bằng cách xem xét mô hình lập trình nền tảng đã làm cho React trở nên khác biệt và hiệu quả.

## 2.0 Mô hình Lập trình Khai báo của React

# 

Từ góc độ kiến trúc, lợi thế chiến lược của React nằm ở việc nó áp dụng mô hình lập trình khai báo. Điều này trái ngược hoàn toàn với các phương pháp mệnh lệnh truyền thống, nơi chúng ta phải tự mình viết code để thao tác từng bước trên DOM. Với cách tiếp cận khai báo, chúng ta chỉ cần mô tả các trạng thái khác nhau mà giao diện người dùng (UI) có thể có; React sẽ đảm nhận phần việc phức tạp là cập nhật DOM một cách hiệu quả để phản ánh trạng thái hiện tại. Mô hình này không chỉ đơn giản hóa việc phát triển mà còn là nền tảng cho khả năng dự đoán và tối ưu hóa của React.

Mô hình này được hiện thực hóa thông qua cú pháp **JSX** và được làm cho đáng tin cậy và hiệu quả bởi vì React giả định các component là các **hàm thuần túy** (pure functions), cho phép nó render và so sánh **cây UI** một cách có thể dự đoán được.

### 2.1 Giao diện người dùng dưới dạng Cây (UI as a Tree)

# 

React mô hình hóa giao diện người dùng dưới dạng một cấu trúc cây, một khái niệm cơ bản để hiểu luồng dữ liệu và hiệu suất. Từ góc độ kiến trúc, có hai loại cây chính cần phân biệt:

1.  **Render Tree (Cây Render):** Đây là biểu diễn mối quan hệ cha-con giữa các component trong một lần render cụ thể. Các component gần gốc của cây được coi là "top-level components", trong khi những component không có con được gọi là "leaf components". Việc hiểu cấu trúc này là rất quan trọng để quản lý luồng dữ liệu và hiệu suất, vì các thay đổi ở các component cấp cao có thể kích hoạt render lại toàn bộ cây con bên dưới chúng.
2.  **Module Dependency Tree (Cây Phụ thuộc Module):** Cây này biểu diễn các câu lệnh `import` giữa các file JavaScript (module). Các công cụ xây dựng (bundlers) như Vite hay Rsbuild sử dụng cây này để xác định đoạn code nào là cần thiết cho ứng dụng, từ đó loại bỏ code không sử dụng và tối ưu hóa kích thước gói (bundle size) cuối cùng.

### 2.2 Component là các Hàm Thuần túy (Pure Functions)

# 

React được thiết kế dựa trên một giả định chiến lược: mọi component bạn viết đều là một **hàm thuần túy**. Trong khoa học máy tính, một hàm thuần túy có hai đặc điểm chính:

-   **Nó chỉ lo việc của mình (It minds its own business):** Nó không thay đổi bất kỳ đối tượng hoặc biến nào đã tồn tại trước khi nó được gọi.
-   **Đầu vào giống nhau, đầu ra giống nhau (Same inputs, same output):** Với cùng một tập hợp đầu vào (**props**), một hàm thuần túy sẽ luôn trả về cùng một kết quả JSX.

Sự ràng buộc này là một sự đánh đổi chiến lược: nó cho phép React thực hiện các tối ưu hóa hiệu suất mạnh mẽ và đảm bảo hành vi render có thể dự đoán được. Ví dụ, một component không thuần túy có thể sửa đổi một biến bên ngoài trong quá trình render:

    // Component không thuần túy (impure) - Thay đổi một biến bên ngoài
    let guest = 0;
    function Cup() {
      // Xấu: thay đổi một biến đã tồn tại!
      guest = guest + 1;
      return <h2>Tea cup for guest #{guest}</h2>;
    }
    

Hành vi này không thể đoán trước. Cách tiếp cận đúng đắn là truyền dữ liệu qua **props** để làm cho component trở nên thuần túy:

    // Component thuần túy (pure)
    function Cup({ guest }) {
      return <h2>Tea cup for guest #{guest}</h2>;
    }
    

Điều quan trọng cần nhớ: giai đoạn render phải luôn thuần túy. Các tác vụ có tác dụng phụ (side effects)—chẳng hạn như thay đổi DOM hoặc gọi API—phải được thực hiện bên ngoài giai đoạn render, trong các trình xử lý sự kiện hoặc **Effects**.

### 2.3 Cú pháp JSX

# 

JSX là một phần mở rộng cú pháp cho JavaScript, cho phép chúng ta viết mã đánh dấu (markup) giống HTML trực tiếp trong code. Nó là công cụ hiện thực hóa mô hình khai báo, cho phép logic render và nội dung cùng tồn tại ở một nơi. Mặc dù trông giống HTML, JSX có một vài quy tắc nghiêm ngặt:

-   **Trả về một phần tử gốc duy nhất:** Mọi component phải trả về một cây JSX duy nhất. Nếu cần trả về nhiều phần tử, hãy bọc chúng trong một thẻ cha chung như `<div>` hoặc một Fragment (`<>...</>`).
-   **Đóng tất cả các thẻ:** Không giống như HTML, các thẻ trong JSX phải được đóng một cách tường minh. Ví dụ, `<img ...>` phải được viết thành `<img ... />`.
-   **Sử dụng** `**camelCase**` **cho hầu hết các thuộc tính:** Các thuộc tính HTML như `class` trở thành `className` trong JSX để tránh xung đột với các từ khóa dành riêng của JavaScript.

Sau khi mô tả giao diện người dùng, bước tiếp theo là làm cho nó trở nên động. Điều này đưa chúng ta đến cốt lõi của tính tương tác trong React: quản lý **state**.

## 3.0 Quản lý Trạng thái (State Management): Nền tảng của Tính tương tác

# 

**State** là trung tâm của mọi ứng dụng React động; nó hoạt động như "bộ nhớ của component". Nó cho phép một component theo dõi thông tin thay đổi theo thời gian và kích hoạt các lần render lại để hiển thị những thay đổi đó. Phần này sẽ phân tích chi tiết về quản lý **state**, từ `useState` cơ bản đến các chiến lược cấu trúc **state** nâng cao cần thiết để xây dựng các ứng dụng mạnh mẽ và dễ bảo trì.

### 3.1 `useState`: Giải phẫu một Hook

# 

Hook `useState` là cơ chế cơ bản để thêm **state** vào các function component. Nó cung cấp hai yếu tố thiết yếu:

1.  Một **biến trạng thái** để lưu giữ dữ liệu giữa các lần render.
2.  Một **hàm set trạng thái** để cập nhật biến đó và kích hoạt một lần render mới.

Cú pháp của nó như sau:

    const [value, setValue] = useState(initialValue);
    

-   `value`: Đây là biến trạng thái. Nó chứa một "**snapshot**" của **state** tại thời điểm render cụ thể đó.
-   `setValue`: Đây là hàm set. Bạn gọi hàm này để yêu cầu một lần render mới với một giá trị **state** mới.
-   `initialValue`: Đây là đối số duy nhất, xác định giá trị ban đầu của biến trạng thái và chỉ được sử dụng trong lần render đầu tiên.

Mỗi instance của một component đều có **state** riêng tư và độc lập.

### 3.2 Vòng đời Cập nhật Giao diện: Render và Commit

# 

React sử dụng một quy trình ba bước có thể dự đoán được để cập nhật giao diện người dùng, có thể được ví như một "đầu bếp trong nhà bếp":

1.  **Kích hoạt một lần render (Triggering a render):** Đây là bước khởi đầu, có thể do lần render đầu tiên hoặc khi **state** thay đổi (ví dụ, do một sự kiện của người dùng).
2.  **Render component (Rendering the component):** Trong giai đoạn này, React gọi các function component của bạn để xác định những gì sẽ hiển thị trên màn hình. Giai đoạn này phải là một quá trình tính toán thuần túy.
3.  **Commit vào DOM (Committing to the DOM):** Sau khi tính toán xong, React sẽ áp dụng các thay đổi cần thiết vào DOM của trình duyệt. Giai đoạn này là lúc giao diện người dùng thực sự được cập nhật.

### 3.3 Trạng thái là một "Snapshot"

# 

Một khái niệm kiến trúc cốt lõi trong React là "**state** hoạt động giống như một **snapshot**". Khi một component render, nó sẽ nhận được một phiên bản cố định của **state** cho lần render đó. Bất kỳ trình xử lý sự kiện, hàm, hoặc biến nào được tạo ra trong lần render đó sẽ "nhìn thấy" giá trị **state** từ **snapshot** đó.

Điều này giải thích tại sao đoạn mã sau không hoạt động như mong đợi:

    function Counter() {
      const [number, setNumber] = useState(0);
    
      return (
        <button onClick={() => {
          setNumber(number + 1); // number là 0
          setNumber(number + 1); // number vẫn là 0
          setNumber(number + 1); // number vẫn là 0
        }}>+3</button>
      );
    }
    

Khi người dùng nhấp vào nút, trình xử lý sự kiện `onClick` được tạo ra trong một lần render nơi `number` là `0`. Do đó, mỗi lệnh gọi `setNumber(number + 1)` thực chất là `setNumber(1)`. React xếp hàng các bản cập nhật này, và trong lần render tiếp theo, giá trị của `number` sẽ là `1`, không phải `3`. Mặc dù việc giải quyết vấn đề này (ví dụ, sử dụng updater functions như `setNumber(n => n + 1)`) nằm ngoài phạm vi của phần này, việc hiểu rõ hành vi **snapshot** này là cực kỳ quan trọng.

### 3.4 Tính bất biến (Immutability): Cập nhật Objects và Arrays

# 

Một quy tắc nền tảng trong quản lý **state** của React là: **"đối xử với bất kỳ đối tượng JavaScript nào bạn đưa vào state như là chỉ đọc (read-only)."** Thay vì sửa đổi (mutate) trực tiếp một đối tượng hoặc mảng, bạn phải tạo một bản sao mới và sau đó sử dụng hàm set trạng thái. Từ góc độ kiến trúc, quy tắc bất biến này rất quan trọng để React có thể tối ưu hóa việc render một cách đáng tin cậy.

-   **Đối với Objects:** Sử dụng cú pháp spread `...` để tạo một **bản sao nông (shallow copy)** của một đối tượng và ghi đè các thuộc tính cần thiết. "Nông" có nghĩa là các đối tượng lồng nhau vẫn là cùng một tham chiếu, đó là lý do tại sao bạn phải tạo bản sao ở mỗi cấp độ khi cập nhật.
-   **Đối với Arrays:** Sử dụng các phương thức không làm thay đổi mảng gốc (non-mutating) như `map()` và `filter()`, kết hợp với cú pháp spread `...`.
    -   **Thêm một mục:** `setArtists([...artists, newArtist]);`
    -   **Xóa một mục:** `setArtists(artists.filter(a => a.id !== artistId));`
    -   **Thay thế các mục:** `setArtists(artists.map(a => a.id === artistId ? updatedArtist : a));`

Đối với các cấu trúc **state** lồng nhau phức tạp, các thư viện như **Immer** có thể đơn giản hóa quá trình cập nhật bằng cách cho phép bạn viết mã trông có vẻ "đột biến" trên một đối tượng `draft` một cách an toàn.

### 3.5 Các Nguyên tắc Cấu trúc Trạng thái

# 

Khi thiết kế **state** của một component, mục tiêu chính là giảm thiểu sự phức tạp và loại bỏ các "trạng thái không thể xảy ra". Dưới đây là năm nguyên tắc để định hướng thiết kế của bạn:

1.  **Nhóm các trạng thái liên quan (Group related state):** Nếu hai hoặc nhiều biến **state** luôn thay đổi cùng nhau, hãy hợp nhất chúng thành một đối tượng **state** duy nhất.
2.  **Tránh các mâu thuẫn trong trạng thái (Avoid contradictions in state):** Thay vì nhiều biến boolean có thể mâu thuẫn (ví dụ: `isSending` và `isSent`), hãy sử dụng một biến **state** duy nhất (`status: 'typing' | 'sending' | 'sent'`). Điều này giúp loại bỏ các "trạng thái không thể xảy ra", một mối quan tâm chính trong kiến trúc ứng dụng.
3.  **Tránh trạng thái dư thừa (Avoid redundant state):** Nếu một thông tin có thể được tính toán từ **props** hoặc **state** hiện có trong quá trình render (ví dụ: `fullName` từ `firstName` và `lastName`), đừng lưu trữ nó trong **state**. Điều này liên quan trực tiếp đến nguyên tắc "nguồn chân lý duy nhất" (single source of truth).
4.  **Tránh trùng lặp trong trạng thái (Avoid duplication in state):** Thay vì sao chép toàn bộ đối tượng vào **state** (ví dụ: `selectedItem`), hãy chỉ lưu trữ ID hoặc chỉ mục. Điều này đảm bảo bạn có một "nguồn chân lý duy nhất" và tránh các vấn đề về đồng bộ hóa.
5.  **Tránh trạng thái lồng nhau sâu (Avoid deeply nested state):** Các cấu trúc **state** phẳng ("bình thường hóa") dễ cập nhật hơn nhiều. Điều này giúp các mẫu bất biến từ phần 3.4 (`...` spread) trở nên đơn giản và ít bị lỗi hơn đáng kể.

Khi **state** cần được truy cập bởi nhiều component, chúng ta cần xem xét các mẫu để chia sẻ nó.

### 3.6 Nâng Trạng thái lên (Lifting State Up) và Context

# 

Có hai mẫu hình chính để chia sẻ **state** giữa các component, mỗi mẫu có các trường hợp sử dụng chiến lược riêng:

| Phương pháp | Mô tả | Trường hợp sử dụng tốt nhất |
| --- | --- | --- |
| **Nâng Trạng thái lên** | Di chuyển **state** đến cha chung gần nhất của các component. Nguyên tắc "nguồn chân lý duy nhất" đảm bảo rằng mỗi phần của **state** có một "chủ sở hữu" cụ thể trong ứng dụng. | Khi một vài component liên quan cần phối hợp **state** của chúng. |
| **Truyền dữ liệu với Context** | Cung cấp một cách để truyền dữ liệu qua cây component mà không cần phải truyền **props** xuống theo cách thủ công ở mọi cấp độ. Hữu ích để tránh "prop drilling". | Khi nhiều component ở các cấp độ lồng khác nhau cần truy cập vào cùng một dữ liệu (ví dụ: theme, người dùng đã đăng nhập). |

Những mẫu hình này cũng dẫn đến sự phân biệt giữa các component "controlled" (được điều khiển bởi **props** từ cha) và "uncontrolled" (quản lý **state** riêng của chúng).

### 3.7 Tách biệt Logic Trạng thái với `useReducer`

# 

Khi logic cập nhật **state** trở nên phức tạp, `useState` có thể trở nên cồng kềnh. `useReducer` là một giải pháp thay thế mạnh mẽ giúp tách biệt logic **state** ra khỏi component. Quá trình chuyển đổi bao gồm một sự thay đổi khái niệm:

-   **Với** `**useState**`**:** Bạn ra lệnh một cách mệnh lệnh: `setTasks([...])`.
-   **Với** `**useReducer**`**:** Bạn mô tả một sự kiện một cách khai báo: `dispatch({ type: 'added_task', ... })`.

Quá trình chuyển đổi bao gồm ba bước:

1.  **Chuyển từ việc set trạng thái sang gửi các hành động (dispatching actions):** Mô tả "người dùng vừa làm gì" thay vì nói cho React "phải làm gì".
2.  **Viết một hàm reducer:** Một reducer là một **hàm thuần túy** nhận **state** hiện tại và một đối tượng hành động, và trả về **state** tiếp theo: `(state, action) => nextState`.
3.  **Sử dụng reducer từ component của bạn:** Thay thế `useState` bằng `useReducer` và sử dụng hàm `dispatch` trong các trình xử lý sự kiện.

## 4.0 Xử lý Sự kiện và Tương tác Người dùng

# 

React xử lý các tương tác của người dùng thông qua các trình xử lý sự kiện. Một cách rõ ràng, các trình xử lý sự kiện là phần "không thuần túy" được chỉ định của một component. Trong khi giai đoạn render phải thuần túy, các trình xử lý sự kiện là nơi bạn _nên_ gây ra các tác dụng phụ, chẳng hạn như thay đổi **state**.

### 4.1 Định nghĩa và Truyền các Trình xử lý Sự kiện

# 

Các trình xử lý sự kiện thường được định nghĩa bên trong một component và được truyền dưới dạng **props** đến các thẻ JSX, chẳng hạn như `onClick={handleClick}`.

Điều cực kỳ quan trọng là phải phân biệt giữa việc **truyền một hàm** và **gọi một hàm**:

-   `onClick={handleClick}`: **Đúng.** Điều này truyền một tham chiếu đến hàm, để React có thể gọi nó sau này.
-   `onClick={handleClick()}`: **Sai.** Điều này gọi hàm _ngay lập tức_ trong quá trình render, là một lỗi phổ biến.

Một mẫu hình kiến trúc phổ biến là truyền các hàm xử lý sự kiện từ component cha xuống component con, cho phép các component con có thể tái sử dụng báo hiệu lại cho cha.

### 4.2 Luồng Lan truyền Sự kiện (Event Propagation)

# 

Khi một sự kiện xảy ra, nó sẽ "nổi bọt" (bubble) hoặc "lan truyền" (propagate) lên cây component. Mỗi trình xử lý sự kiện nhận một **đối tượng sự kiện** (`e`) làm đối số duy nhất, cung cấp các phương thức để kiểm soát luồng này:

-   `e.stopPropagation()`: Ngăn chặn sự kiện lan truyền lên các component cha.
-   `e.preventDefault()`: Ngăn chặn hành vi mặc định của trình duyệt đối với một số sự kiện nhất định (ví dụ: gửi biểu mẫu).

Mặc dù React xử lý hầu hết các tương tác UI, đôi khi chúng ta cần tương tác với các hệ thống không phải React, đòi hỏi phải có "lối thoát".

## 5.0 "Lối thoát" (Escape Hatches): Tương tác với Hệ thống bên ngoài

# 

"Lối thoát" là các API cho phép bạn "bước ra ngoài" mô hình khai báo của React. Đây là một sự phá vỡ có chủ đích và đôi khi có rủi ro khỏi mô hình cốt lõi, cần thiết khi các component React phải điều khiển và đồng bộ hóa với các hệ thống không do React quản lý, chẳng hạn như DOM của trình duyệt, các widget của bên thứ ba, hoặc mạng.

### 5.1 Refs: Tham chiếu Giá trị và Thao tác DOM

# 

Hook `useRef` phục vụ một mục đích kép như một "lối thoát" mạnh mẽ:

1.  **Tham chiếu giá trị mà không kích hoạt re-render:** Một **ref** hoạt động như một "túi bí mật" mà React không theo dõi. Nó rất hữu ích để lưu trữ các giá trị không ảnh hưởng đến JSX, chẳng hạn như ID của timeout.
2.  **Thao tác trực tiếp với DOM:** Bằng cách gắn một **ref** vào một phần tử JSX (`<div ref={myRef}>`), React sẽ đặt nút DOM thực tế vào `myRef.current`. Sau đó, bạn có thể truy cập nó để gọi các API của trình duyệt như `.focus()` hoặc `.scrollIntoView()`.

Nên sử dụng **ref** cho các hành động không phá hủy. Tránh sửa đổi các nút DOM do React quản lý, vì điều này có thể xung đột với các bản cập nhật của React.

### 5.2 Effects: Đồng bộ hóa với Hệ thống bên ngoài

# 

Hook `useEffect` là một "lối thoát" để chạy code _sau khi_ render và commit vào DOM, cho phép các component đồng bộ hóa với một hệ thống bên ngoài. Điều quan trọng là phải phân biệt giữa logic sự kiện (chạy để đáp ứng với một _tương tác cụ thể_) và logic **Effect** (chạy để đáp ứng với việc _component được hiển thị_).

Một `useEffect` có cấu trúc như sau:

-   **Hàm setup:** Chứa code để bắt đầu quá trình đồng bộ hóa.
-   **Mảng dependencies:** Một mảng các giá trị "reactive" (**props**, **state**, và các biến được khai báo trong component) mà **Effect** đọc. **Effect** sẽ chỉ chạy lại nếu một trong những giá trị này thay đổi. Một mảng rỗng `[]` có nghĩa là **Effect** chỉ chạy một lần khi component "mount".
-   **Hàm cleanup:** Một hàm tùy chọn được trả về từ hàm setup, chỉ định cách dừng đồng bộ hóa (ví dụ: `removeEventListener`, `clearTimeout`).

### 5.3 Các Mẫu nâng cao với Effects

# 

-   **Tách biệt Logic Phản ứng và Không phản ứng:** Đôi khi, một **Effect** cần đọc một giá trị reactive nhưng không nên chạy lại khi giá trị đó thay đổi. Để giải quyết vấn đề này, logic không phản ứng nên được tách ra khỏi **Effect** (sử dụng `useEffectEvent` trong các phiên bản React mới hơn) để giữ mảng dependencies ở mức tối thiểu.
-   **Tái sử dụng Logic với Custom Hooks:** Khi logic `useEffect` trở nên lặp đi lặp lại, nó nên được trích xuất vào một Custom Hook—một hàm JavaScript có tên bắt đầu bằng `use`. Mỗi lần một Custom Hook được gọi, nó sẽ nhận được **state** và **Effects** độc lập.

### 5.4 Khi nào KHÔNG nên sử dụng Effect

# 

Việc lạm dụng **Effects** là một anti-pattern phổ biến dẫn đến code dễ vỡ và khó gỡ lỗi. Dưới đây là các tình huống mà một **Effect** là không cần thiết:

-   **Để chuyển đổi dữ liệu cho việc render:** Thay vào đó, hãy tính toán trực tiếp trong quá trình render.
-   **Để lưu vào bộ nhớ đệm các tính toán tốn kém:** Thay vào đó, hãy sử dụng `useMemo`.
-   **Để xử lý các sự kiện của người dùng:** Thay vào đó, hãy đặt logic trực tiếp vào các trình xử lý sự kiện tương ứng.
-   **Để reset trạng thái khi một prop thay đổi:** Thay vào đó, hãy sử dụng thuộc tính `key` để buộc React tạo lại component và **state** của nó.

## 6.0 Tối ưu hóa Hiệu suất

# 

Việc hiểu cách React quản lý **state** và các lần render là chìa khóa để tối ưu hóa hiệu suất. Hầu hết các tối ưu hóa đều xoay quanh việc tránh các công việc không cần thiết.

### 6.1 Bảo toàn và Thiết lập lại Trạng thái (Preserving and Resetting State)

# 

React xác định các component giữa các lần render dựa trên **vị trí của chúng trong cây UI**. Trạng thái được gắn với một "địa chỉ" trong cây. React sẽ phá hủy (và do đó reset) **state** của một component khi nó bị xóa khỏi cây, hoặc khi một component có kiểu khác được render ở cùng một vị trí.

Bạn có thể sử dụng hành vi này một cách có chủ đích bằng thuộc tính **key**. Việc cung cấp một **key** khác cho một component sẽ báo hiệu cho React rằng đó là một component hoàn toàn khác, buộc nó phải thiết lập lại **state** của component đó và toàn bộ cây con của nó, ngay cả khi nó được render ở cùng một vị trí.

### 6.2 Memoization với React Compiler

# 

Trước đây, tối ưu hóa hiệu suất thường đòi hỏi phải **memoization** thủ công bằng `useMemo` và `useCallback` để tránh tính toán lại hoặc tạo lại các hàm một cách không cần thiết. Cách tiếp cận này làm tăng gánh nặng tinh thần cho nhà phát triển.

**React Compiler** là sự đền đáp cuối cùng cho việc tuân thủ các nguyên tắc của React (tính thuần túy, bất biến, UI khai báo). Nó là một công cụ build-time tự động phân tích code của bạn và áp dụng **memoization** một cách thông minh, loại bỏ nhu cầu tối ưu hóa thủ công. Bằng cách tự động hóa quá trình này, compiler cho phép các nhà phát triển tập trung vào logic tính năng thay vì tối ưu hóa vi mô. Nó hoạt động với code React tuân thủ "Rules of React" và có thể được áp dụng dần dần bằng cách sử dụng các chỉ thị như `"use memo"`, đơn giản hóa đáng kể việc tối ưu hóa hiệu suất trong các ứng dụng React hiện đại.