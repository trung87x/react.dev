# Hiển thị Dữ liệu trong React: Từ Cây Giao diện đến Danh sách Động

#

Chào mừng bạn đến với React! Trong thế giới của React, chúng ta xây dựng giao diện người dùng (UI) bằng cách kết hợp các khối xây dựng độc lập và có thể tái sử dụng được gọi là "component". Các component này không tồn tại một cách riêng lẻ; chúng lồng vào nhau để tạo thành một cấu trúc cây, phản ánh cách giao diện của bạn được tổ chức. Mục tiêu của tài liệu này là giúp bạn hiểu rõ về cấu trúc cây này và cách áp dụng nó để hiển thị các danh sách dữ liệu một cách linh hoạt và hiệu quả.

\--------------------------------------------------------------------------------

## 1\. Giao diện của bạn dưới dạng một Cấu trúc Cây

### 1.1. Cây Render là gì?

#

Hãy bắt đầu với một mô hình tư duy nền tảng: mọi giao diện bạn xây dựng trong React đều là một cái cây. Khi bạn xây dựng một ứng dụng, React sẽ tạo ra một "cây render" (render tree) từ các component của bạn. Đây là một mô hình về mối quan hệ cha-con giữa các component. Như tài liệu chính thức của React đã nói, "React sử dụng cây để mô hình hóa các mối quan hệ giữa các component và module." (React uses trees to model the relationships between components and modules.)

Hãy xem xét một ví dụ đơn giản. Giả sử bạn có một component `App` hiển thị một component `Gallery`, và `Gallery` lại hiển thị nhiều component `Profile`.

    function App() {
      return (
        <Gallery />
      );
    }

    function Gallery() {
      return (
        <section>
          <h1>Amazing scientists</h1>
          <Profile />
          <Profile />
        </section>
      );
    }

Cấu trúc cây render tương ứng của các component sẽ trông như thế này:

- `App`
  - `Gallery`
    - `Profile`
    - `Profile`

Component ở gần đỉnh của cây, như `App`, được coi là component cấp cao (top-level). Các component không có con, như `Profile`, là các component lá (leaf components).

Bên cạnh cây render, React còn sử dụng một loại cây khác gọi là "cây phụ thuộc module" (module dependency tree). Trong khi cây render giúp chúng ta hiểu luồng dữ liệu và hiệu suất render, cây phụ thuộc module được các công cụ xây dựng (build tools) như Vite hoặc Webpack sử dụng để xác định các tệp mã nguồn cần được gộp lại (bundle). Hiểu được cây này rất quan trọng khi bạn cần gỡ lỗi về kích thước ứng dụng của mình.

### 1.2. Tại sao việc này lại hữu ích?

#

Việc coi giao diện người dùng như một cấu trúc cây mang lại nhiều lợi ích quan trọng, giúp bạn hiểu rõ hơn về cách ứng dụng của mình hoạt động.

- **Hiểu luồng dữ liệu:** Trong React, dữ liệu thường chảy từ trên xuống dưới, tức là từ các component cha (cấp cao) xuống các component con (cấp thấp). Việc hình dung cấu trúc cây giúp bạn dễ dàng theo dõi dòng chảy này và quản lý state của ứng dụng.
- **Hiểu hiệu suất render:** Các component gần gốc cây (top-level) có ảnh hưởng lớn đến hiệu suất của tất cả các component bên dưới chúng. Nếu một component cấp cao render lại, nó có thể kích hoạt việc render lại của toàn bộ cây con bên dưới. Hiểu rõ điều này giúp bạn tối ưu hóa hiệu suất ứng dụng hiệu quả hơn.

Bây giờ khi đã hiểu về cấu trúc cây của UI, hãy cùng xem cách áp dụng kiến thức này vào một trong những tác vụ phổ biến nhất: hiển thị một danh sách dữ liệu.

\--------------------------------------------------------------------------------

## 2\. Hiển thị Danh sách từ Mảng dữ liệu

#

Hầu hết các ứng dụng đều cần hiển thị danh sách các mục, từ danh sách sản phẩm đến danh sách bài viết. Trong React, bạn có thể sử dụng các phương thức mảng chuẩn của JavaScript để biến đổi một mảng dữ liệu thành một mảng các component.

### 2.1. Biến đổi một mảng dữ liệu thành các Component

#

Giả sử bạn có một mảng dữ liệu đơn giản như sau:

    const people = [
      'Creola Katherine Johnson: mathematician',
      'Mario José Molina-Pasquel Henríquez: chemist',
      'Mohammad Abdus Salam: physicist',
      'Percy Lavon Julian: chemist',
      'Subrahmanyan Chandrasekhar: astrophysicist'
    ];

Để hiển thị danh sách này, chúng ta sẽ sử dụng phương thức `.map()` của JavaScript. Phương thức này sẽ lặp qua từng phần tử trong mảng `people` và trả về một mảng mới chứa các phần tử JSX `<li>`.

    // Trong component của bạn
    const listItems = people.map(person => <li>{person}</li>);

    return <ul>{listItems}</ul>;

Bằng cách đặt `{listItems}` bên trong một thẻ `<ul>`, bạn đã render thành công một danh sách các mục từ mảng dữ liệu của mình.

### 2.2. Lọc danh sách trước khi hiển thị

#

Đôi khi, bạn chỉ muốn hiển thị một phần của danh sách. Bạn có thể kết hợp phương thức `.filter()` với `.map()` để làm điều này. `.filter()` sẽ tạo ra một mảng mới chỉ chứa các mục thỏa mãn điều kiện bạn đặt ra.

Ví dụ, với một mảng dữ liệu có cấu trúc hơn, chúng ta có thể lọc ra chỉ những người là nhà hóa học (`chemist`):

    // Dữ liệu có cấu trúc
    const people = [{
      id: 0,
      name: 'Creola Katherine Johnson',
      profession: 'mathematician',
    }, {
      id: 1,
      name: 'Mario José Molina-Pasquel Henríquez',
      profession: 'chemist',
    }, {
      id: 2,
      name: 'Mohammad Abdus Salam',
      profession: 'physicist',
    }, {
      id: 3,
      name: 'Percy Lavon Julian',
      profession: 'chemist',
    }, {
      id: 4,
      name: 'Subrahmanyan Chandrasekhar',
      profession: 'astrophysicist',
    }];

    // 1. Lọc mảng
    const chemists = people.filter(person =>
      person.profession === 'chemist'
    );

    // 2. Map qua mảng đã lọc
    const listItems = chemists.map(person =>
      <li key={person.id}>
        <p>
          <b>{person.name}:</b>
          {' ' + person.profession}
        </p>
      </li>
    );

    return <ul>{listItems}</ul>;

Quy trình này cho phép bạn tạo ra các danh sách động, hiển thị dữ liệu dựa trên các điều kiện thay đổi.

Bạn đã hiển thị thành công một danh sách, nhưng có thể bạn sẽ thấy một cảnh báo trong console. Hãy cùng tìm hiểu về cảnh báo đó và cách khắc phục trong phần tiếp theo.

\--------------------------------------------------------------------------------

## 3\. Tầm quan trọng của thuộc tính `key`

#

Đây là một trong những khái niệm quan trọng và thường gây nhầm lẫn nhất cho người mới bắt đầu. Việc hiểu rõ `key` sẽ giúp bạn tránh được rất nhiều lỗi tiềm ẩn.

Khi bạn render một danh sách như trên, bạn sẽ gặp cảnh báo sau trong console: `Warning: Each child in a list should have a unique “key” prop.` (Cảnh báo: Mỗi phần tử con trong một danh sách nên có một prop "key" duy nhất.)

### 3.1. Vấn đề: React cần nhận dạng từng mục

#

Cảnh báo này xuất hiện vì React cần một cách để xác định duy nhất từng mục trong danh sách khi danh sách thay đổi (ví dụ: thêm, xóa, hoặc sắp xếp lại các mục). Nếu không có một định danh ổn định, React sẽ phải dựa vào thứ tự của các mục, điều này có thể dẫn đến các lỗi khó lường và làm giảm hiệu suất.

Hãy tưởng tượng các tệp trên máy tính của bạn không có tên. Thay vào đó, bạn phải gọi chúng là "tệp đầu tiên", "tệp thứ hai". Điều này sẽ trở nên rất rối rắm khi bạn xóa một tệp, vì "tệp thứ hai" sẽ trở thành "tệp đầu tiên". Tương tự, `key` trong React hoạt động giống như tên tệp, cung cấp một định danh ổn định cho mỗi mục, bất kể vị trí của nó.

### 3.2. Giải pháp: Thêm thuộc tính `key`

#

Giải pháp rất đơn giản: thêm một thuộc tính `key` vào mỗi phần tử trong mảng JSX bạn tạo ra. `key` phải là một chuỗi hoặc một số duy nhất trong số các phần tử anh em của nó.

Nguồn tốt nhất cho `key` là một ID duy nhất từ dữ liệu của bạn, chẳng hạn như ID từ cơ sở dữ liệu.

    const listItems = people.map(person =>
      <li key={person.id}>
        {person.name}
      </li>
    );

Với `key`, React có thể xác định chính xác mục nào đã thay đổi, được thêm vào hay bị xóa đi, giúp quá trình cập nhật giao diện người dùng trở nên hiệu quả và đáng tin cậy.

### 3.3. Các quy tắc vàng cho `key`

#

Khi sử dụng `key`, hãy ghi nhớ những quy tắc quan trọng sau:

- **Keys phải là duy nhất trong số các phần tử anh em (siblings):** Một `key` chỉ cần là duy nhất trong một danh sách cụ thể. Bạn hoàn toàn có thể sử dụng lại cùng một `key` trong các mảng khác nhau.
- **Keys phải ổn định và không thay đổi:** Đừng tạo `key` một cách ngẫu hứng trong khi render, ví dụ như dùng `Math.random()`. Nếu `key` thay đổi giữa các lần render, React sẽ phá hủy component cũ và tạo một component mới, làm mất state và giảm hiệu suất.
- **Tránh sử dụng chỉ số mảng (index) làm key:** Đây là một lỗi phổ biến nhưng rất nguy hiểm. React sử dụng `key` để nhận dạng một thể hiện (instance) của component qua các lần render. Khi danh sách được sắp xếp lại, một mục sẽ thay đổi chỉ số của nó, nhưng dữ liệu và state của nó nên được giữ nguyên. Nếu bạn dùng index làm `key`, React sẽ bị "đánh lừa". Ví dụ, component ở `index: 1` sẽ nhận state cũ của component từng ở `index: 1` trước đó, ngay cả khi nó là một mục dữ liệu hoàn toàn khác. Điều này dẫn đến hiển thị state sai và các lỗi rất khó gỡ. Hãy chỉ sử dụng index làm `key` nếu danh sách là tĩnh, không bao giờ thay đổi thứ tự, thêm hoặc xóa.

\--------------------------------------------------------------------------------

## 4\. Tổng kết

#

Qua tài liệu này, bạn đã khám phá những khái niệm cốt lõi về cách React cấu trúc và hiển thị dữ liệu. Dưới đây là những điểm chính cần ghi nhớ:

- React tổ chức giao diện người dùng thành một **cây render**, giúp hiểu rõ cấu trúc và luồng dữ liệu từ component cha xuống component con.
- Sử dụng các phương thức mảng của JavaScript như `.map()` và `.filter()` để hiển thị các danh sách component một cách linh hoạt từ dữ liệu.
- Luôn cung cấp một thuộc tính `**key**` ổn định và duy nhất cho mỗi mục trong danh sách để giúp React quản lý state và cập nhật UI một cách hiệu quả.
