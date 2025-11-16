# Hướng Dẫn Toàn Diện về Môi Trường Phát Triển và Tái Cấu Trúc Mã trong React

### **Giới thiệu**

#

Trong thế giới phát triển React, việc xây dựng các ứng dụng không chỉ dừng lại ở việc viết mã hoạt động. Nó còn là việc tạo ra các hệ thống dễ bảo trì, có khả năng mở rộng và hiệu suất cao. Nền tảng của một dự án React thành công nằm ở hai trụ cột chính: một môi trường phát triển vững chắc và sự am hiểu sâu sắc về các mẫu quản lý trạng thái cốt lõi. Một môi trường được thiết lập tốt sẽ đảm bảo chất lượng mã và năng suất của nhóm, trong khi các chiến lược quản lý trạng thái hiệu quả sẽ ngăn ngừa lỗi và đơn giản hóa sự phức tạp khi ứng dụng phát triển.

Mục tiêu của tài liệu này là cung cấp cho các nhà phát triển một hướng dẫn thực hành để đạt được cả hai mục tiêu trên. Chúng tôi sẽ hướng dẫn bạn qua các phương pháp hay nhất để xây dựng các ứng dụng React mạnh mẽ, dễ bảo trì và hiệu quả.

Tài liệu này được chia thành hai phần chính. **Phần 1** tập trung vào việc thiết lập một môi trường phát triển chuyên nghiệp, bao gồm các công cụ hiện đại để khởi tạo dự án, đảm bảo chất lượng mã với linting và định dạng, và tích hợp TypeScript để tăng cường độ an toàn cho mã nguồn. **Phần 2** đi sâu vào các chiến lược tái cấu trúc trạng thái nâng cao, khám phá các nguyên tắc cấu trúc trạng thái, tầm quan trọng của tính bất biến, và mẫu "nâng trạng thái lên" (lifting state up) để quản lý trạng thái được chia sẻ.

\--------------------------------------------------------------------------------

## **Phần 1: Thiết Lập Môi Trường Phát Triển React Chuyên Nghiệp**

#

Việc thiết lập một môi trường phát triển được cấu hình tốt là một quyết định chiến lược vượt ra ngoài việc lựa chọn công cụ. Nó là việc xây dựng một nền tảng vững chắc cho chất lượng mã, sự cộng tác hiệu quả và năng suất của nhà phát triển. Một môi trường được tiêu chuẩn hóa với các công cụ tự động để linting và định dạng sẽ giảm thiểu các lỗi logic, đảm bảo tính nhất quán trong codebase và cho phép các nhóm tập trung vào việc giải quyết các vấn đề kinh doanh thay vì tranh luận về kiểu dáng mã. Khi được thiết lập đúng cách, môi trường phát triển sẽ trở thành một tài sản vô giá, thúc đẩy các phương pháp hay nhất và hợp lý hóa toàn bộ vòng đời phát triển. Hãy bắt đầu bằng cách khởi tạo một dự án React hiện đại.

### **1.1. Khởi Tạo Dự Án React Hiện Đại**

#

Các phương pháp để khởi tạo một dự án React mới đã phát triển đáng kể. Đáng chú ý, công cụ `Create React App` (CRA) đã từng rất phổ biến **không còn được khuyến nghị** nữa. Lý do là vì CRA không còn được bảo trì tích cực, sử dụng các dependency đã lỗi thời và không hỗ trợ các tính năng hiện đại mà các framework khác cung cấp.

Thay vào đó, các nhà phát triển hiện có hai con đường chính để lựa chọn, tùy thuộc vào yêu cầu của dự án:

- **Sử dụng Frameworks:** Đây là một quyết định chiến lược. Frameworks như **Next.js** cung cấp một giải pháp tích hợp, có định hướng cho các ứng dụng phức tạp, giải quyết các vấn đề như routing và data fetching ngay từ đầu. Đối với các ứng dụng full-stack, Next.js (sử dụng App Router) là một lựa chọn xuất sắc, tận dụng toàn bộ kiến trúc của React để xây dựng các ứng dụng có khả năng mở rộng và hiệu suất cao, với sự hỗ trợ cho Server Components, rendering phía máy chủ (SSR), và tạo trang tĩnh (SSG).
- **Xây Dựng từ Đầu:** Ngược lại, các công cụ xây dựng như **Vite**, **Parcel**, và **Rsbuild** cung cấp sự linh hoạt tối đa cho các ứng dụng trang đơn (SPA) nơi bạn muốn tự lắp ráp stack của mình. Điều này có nghĩa là bạn sẽ chịu trách nhiệm giải quyết các vấn đề ở cấp độ framework. Vite, đặc biệt, được biết đến với tốc độ và hệ sinh thái plugin phong phú, là một lựa chọn tuyệt vời để bắt đầu.

Để bắt đầu một dự án SPA mới với Vite và TypeScript, bạn có thể sử dụng lệnh sau:

    npm create vite@latest my-app -- --template react-ts

Việc lựa chọn giữa một framework đầy đủ tính năng và một công cụ xây dựng tối giản phụ thuộc vào quy mô và nhu cầu cụ thể của dự án của bạn.

### **1.2. Cấu Hình Chất Lượng Mã: Linting và Định Dạng**

#

Việc duy trì một codebase lành mạnh, nhất quán và dễ cộng tác là rất quan trọng, đặc biệt là trong các dự án nhóm. Linting và định dạng code tự động là hai công cụ không thể thiếu để đạt được mục tiêu này, giúp bạn ngăn chặn các lỗi tiềm ẩn trước khi chúng xảy ra.

#### **Linting với ESLint**

#

ESLint là một linter mã nguồn mở phổ biến cho JavaScript, giúp tìm và sửa các vấn đề trong mã của bạn một cách tự động. Nó phân tích mã của bạn để nhanh chóng tìm ra các lỗi logic, lỗi cú pháp và các vấn đề về kiểu dáng.

Để tích hợp hiệu quả vào một dự án React, việc kích hoạt các quy tắc từ `eslint-plugin-react-hooks` là cực kỳ quan trọng. Các quy tắc này **rất cần thiết và giúp phát hiện sớm các lỗi nghiêm trọng nhất** liên quan đến việc sử dụng Hooks. Ví dụ, nó sẽ cảnh báo nếu bạn quên thêm một dependency vào mảng phụ thuộc của `useEffect`, một lỗi phổ biến có thể gây ra các vòng lặp vô hạn hoặc các hiệu ứng không được cập nhật. Việc tuân thủ các quy tắc này đảm bảo rằng các thành phần của bạn hoạt động như mong đợi và tránh được các hành vi không thể đoán trước.

#### **Định Dạng Tự Động với Prettier**

#

Prettier là một công cụ định dạng code có định hướng, giúp đảm bảo một kiểu dáng mã nhất quán trong toàn bộ dự án. Nó loại bỏ các cuộc tranh luận về kiểu dáng bằng cách tự động định dạng lại mã của bạn mỗi khi bạn lưu tệp.

Để tích hợp Prettier vào Visual Studio Code, hãy làm theo các bước sau:

1.  **Cài đặt Extension:** Mở VS Code, nhấn `Ctrl/Cmd+P` để mở Quick Open, sau đó dán `ext install esbenp.prettier-vscode` và nhấn Enter.
2.  **Kích hoạt "Format on Save":**
    - Nhấn `CTRL/CMD + SHIFT + P`.
    - Gõ "settings" và chọn "Preferences: Open User Settings".
    - Trong thanh tìm kiếm, gõ "format on save".
    - Đảm bảo tùy chọn "Editor: Format On Save" đã được chọn.

Để tránh xung đột, điều quan trọng là phải vô hiệu hóa các quy tắc định dạng của ESLint để Prettier có thể đảm nhận hoàn toàn nhiệm vụ này. Điều này có thể được thực hiện bằng cách sử dụng `eslint-config-prettier`, đảm bảo rằng ESLint chỉ tập trung vào việc phát hiện các lỗi logic, trong khi Prettier xử lý tất cả các vấn đề về định dạng.

### **1.3. Tích Hợp TypeScript**

#

Việc thêm TypeScript vào một dự án React mang lại những lợi ích đáng kể, bao gồm kiểm tra kiểu tĩnh để phát hiện lỗi sớm, tự động hoàn thành mã tốt hơn và cải thiện trải nghiệm tổng thể cho nhà phát triển, giúp mã nguồn trở nên dễ đọc và bảo trì hơn.

Nếu bạn đang làm việc trên một dự án React hiện có, bạn có thể thêm các định nghĩa kiểu của React bằng lệnh sau:

    npm install --save-dev @types/react @types/react-dom

Một quy tắc quan trọng cần nhớ là mọi tệp chứa mã JSX phải sử dụng phần mở rộng `.tsx`.

Việc định nghĩa kiểu cho các props của thành phần là một trong những ứng dụng phổ biến nhất của TypeScript trong React. Bạn có thể sử dụng một `interface` hoặc `type` để mô tả hình dạng của đối tượng props, đảm bảo rằng các thành phần nhận được dữ liệu chính xác.

Dưới đây là một ví dụ đơn giản về việc định nghĩa kiểu cho props của một thành-phần `MyButton`:

    interface MyButtonProps {
      /** The text to display inside the button */
      title: string;
      /** Whether the button can be interacted with */
      disabled: boolean;
    }

    function MyButton({ title, disabled }: MyButtonProps) {
      return (
        <button disabled={disabled}>{title}</button>
      );
    }

\--------------------------------------------------------------------------------

## **Phần 2: Các Mẫu Tái Cấu Trúc Trạng Thái Nâng Cao**

#

Quản lý và cấu trúc trạng thái một cách hiệu quả là một trong những khía cạnh quan trọng và thách thức nhất của việc phát triển các ứng dụng React phức tạp. Cách bạn tổ chức trạng thái có thể ảnh hưởng sâu sắc đến khả năng bảo trì, hiệu suất và khả năng mở rộng của ứng dụng. Khi các thành phần trở nên phức tạp hơn, logic trạng thái có thể trở nên rối rắm, dẫn đến các lỗi khó gỡ và một codebase khó hiểu.

Trong phần này, chúng ta sẽ khám phá các khái niệm và mẫu cốt lõi để làm chủ việc quản lý trạng thái. Chúng ta sẽ bắt đầu với các nguyên tắc cơ bản để cấu trúc trạng thái một cách hiệu quả. Sau đó, chúng ta sẽ đi sâu vào việc xử lý bất biến (immutability), một khái niệm nền tảng trong React, và cuối cùng, chúng ta sẽ thảo luận về mẫu "nâng trạng thái lên" (lifting state up) để quản lý trạng thái được chia sẻ giữa các thành phần.

### **2.1. Các Nguyên Tắc Vàng để Cấu Trúc Trạng Thái**

#

Việc cấu trúc trạng thái tốt có thể tạo ra sự khác biệt giữa một thành phần dễ bảo trì và một thành phần là nguồn gốc thường xuyên của các lỗi. Bằng cách tuân thủ một vài nguyên tắc hướng dẫn, bạn có thể làm cho trạng thái của mình dễ cập nhật hơn và giảm thiểu khả năng xảy ra lỗi.

Dưới đây là năm nguyên tắc chính để cấu trúc trạng thái của bạn một cách hiệu quả:

1.  **Nhóm trạng thái liên quan:** Nếu bạn thấy mình luôn cập nhật hai hoặc nhiều biến trạng thái cùng một lúc, hãy xem xét việc hợp nhất chúng thành một biến trạng thái duy nhất. Ví dụ, thay vì `const [x, setX] = useState(0);` và `const [y, setY] = useState(0);`, hãy nhóm chúng lại thành `const [position, setPosition] = useState({ x: 0, y: 0 });`. Điều này giúp đảm bảo tính nhất quán và giảm số lượng lệnh gọi cập nhật trạng thái.
2.  **Tránh mâu thuẫn trong trạng thái:** Thiết kế cấu trúc trạng thái của bạn để ngăn ngừa các trạng thái "bất khả thi". Ví dụ, việc sử dụng hai biến boolean như `isSending` và `isSent` có thể dẫn đến một trạng thái không thể xảy ra, nơi cả hai đều là `true`. Một cách tiếp cận tốt hơn là sử dụng một biến trạng thái duy nhất như `status` với các giá trị có thể là `'typing'`, `'sending'`, hoặc `'sent'`, điều này giúp loại bỏ hoàn toàn khả năng xảy ra các trạng thái mâu thuẫn.
3.  **Tránh trạng thái dư thừa:** Nếu một phần thông tin có thể được tính toán từ các props hoặc các biến trạng thái hiện có trong quá trình render, bạn không nên lưu trữ nó trong trạng thái. Ví dụ, `fullName` có thể được tính toán từ `firstName` và `lastName` mỗi khi render. Việc lưu trữ thông tin có thể tính toán được trong trạng thái sẽ tạo ra sự dư thừa và yêu cầu bạn phải giữ cho chúng đồng bộ một cách thủ công, điều này rất dễ gây ra lỗi.
4.  **Tránh trùng lặp trong trạng thái:** Khi cùng một dữ liệu được sao chép giữa nhiều biến trạng thái, việc giữ chúng đồng bộ sẽ trở nên khó khăn. Thay vì lưu trữ toàn bộ đối tượng (ví dụ: `selectedItem`), hãy chỉ lưu trữ ID hoặc chỉ mục của nó (`selectedId`) trong trạng thái. Sau đó, bạn có thể tìm thấy đối tượng đầy đủ từ một danh sách mỗi khi render. Điều này ngăn ngừa các lỗi đồng bộ hóa, chẳng hạn như khi mục gốc trong danh sách được chỉnh sửa nhưng bản sao được lưu trong trạng thái `selectedItem` không được cập nhật.
5.  **Tránh trạng thái lồng sâu:** Trạng thái có cấu trúc phân cấp sâu rất khó cập nhật một cách bất biến. Khi có thể, hãy ưu tiên cấu trúc trạng thái theo cách phẳng hơn, hoặc "chuẩn hóa" nó bằng cách lưu trữ các mục trong một đối tượng dưới dạng một bảng tra cứu theo ID, thay vì một cây lồng nhau sâu. Điều này giúp đơn giản hóa đáng kể logic cập nhật.

Như một câu nói được điều chỉnh lại của Albert Einstein đã tóm tắt rất hay:

**“Make your state as simple as it can be—but no simpler.”**

### **2.2. Xử Lý Bất Biến: Cập Nhật Đối Tượng và Mảng**

#

Một khái niệm cốt lõi trong React là trạng thái được coi như một "snapshot". Khi bạn gọi hàm `set`, React không thay đổi biến trạng thái trong lần render hiện tại. Thay vào đó, nó kích hoạt một lần render mới với giá trị trạng thái mới. Điều này có nghĩa là trong các trình xử lý sự kiện của một lần render cụ thể, giá trị trạng thái được "cố định" và không thay đổi, ngay cả sau khi gọi hàm `set`.

Ví dụ, việc gọi `setNumber(number + 1)` ba lần liên tiếp trong một trình xử lý sự kiện sẽ chỉ tăng `number` lên 1, vì `number` trong cả ba lần gọi đều là giá trị từ snapshot của lần render đó.

Từ khái niệm này, chúng ta đi đến nguyên tắc bất biến (immutability): trạng thái nên được coi là chỉ đọc. Thay vì thay đổi trực tiếp (mutating) các đối tượng hoặc mảng trong trạng thái, bạn nên tạo một bản sao mới và sau đó cập nhật trạng thái để sử dụng bản sao đó.

#### **Tại sao tính bất biến lại quan trọng?**

#

Việc tuân thủ tính bất biến không chỉ là một quy tắc. Nó mang lại những lợi ích hữu hình và ngăn ngừa các lỗi khó lường:

- **Gỡ lỗi:** Nếu bạn không thay đổi trạng thái, bạn có thể dễ dàng kiểm tra các snapshot trạng thái trước đó. `console.log` sẽ hiển thị chính xác trạng thái tại các thời điểm khác nhau mà không bị ảnh hưởng bởi các thay đổi sau này.
- **Tối ưu hóa:** Tính bất biến cho phép React thực hiện các kiểm tra tham chiếu nhanh chóng (`prevObj === obj`). Nếu tham chiếu không thay đổi, React có thể bỏ qua việc render lại toàn bộ cây thành phần con, ví dụ như khi sử dụng `React.memo`, giúp cải thiện đáng kể hiệu suất.
- **Các tính năng mới:** Nhiều tính năng mới của React, chẳng hạn như các tính năng đồng thời (concurrent features), dựa trên khả năng làm việc với các snapshot trạng thái khác nhau.
- **Các thay đổi về yêu cầu:** Việc triển khai các tính năng như undo/redo hoặc xem lại lịch sử thay đổi trở nên dễ dàng hơn nhiều khi bạn có một lịch sử các snapshot trạng thái bất biến.

#### **Cập nhật Đối tượng**

#

Khi bạn muốn cập nhật một trường trong một đối tượng trạng thái, bạn nên tạo một đối tượng mới. Cú pháp spread `...` là công cụ hoàn hảo cho việc này, cho phép bạn sao chép tất cả các trường từ đối tượng cũ và sau đó ghi đè các trường cần thay đổi.

    function Form() {
      const [person, setPerson] = useState({
        name: 'Niki de Saint Phalle',
        artwork: {
          title: 'Blue Nana',
          city: 'Hamburg',
        }
      });

      function handleNameChange(e) {
        setPerson({
          ...person, // Sao chép các trường cũ
          name: e.target.value // Ghi đè trường name
        });
      }
    }

Để cập nhật một đối tượng lồng nhau, bạn cần phải sao chép ở mỗi cấp. Ví dụ, để cập nhật `artwork.city`, bạn sẽ cần sao chép cả đối tượng `artwork` và đối tượng `person`:

    setPerson({
      ...person, // Sao chép các trường cấp cao nhất
      artwork: {
        ...person.artwork, // Sao chép các trường của đối tượng artwork cũ
        city: e.target.value // Ghi đè trường city
      }
    });

#### **Cập nhật Mảng**

#

Tương tự như đối tượng, mảng trong trạng thái cũng phải được coi là bất biến. Thay vì sử dụng các phương thức làm thay đổi mảng như `push()` hoặc `splice()`, hãy sử dụng các phương thức không đột biến để tạo một mảng mới.

- **Thêm một mục:** Để thêm một mục, hãy tạo một mảng mới bằng cách trải (spread) các mục hiện có và thêm mục mới vào đầu hoặc cuối. Đây là phương pháp bất biến tương đương với `push()` hoặc `unshift()`.
- **Xóa một mục:** Sử dụng phương thức `filter()` để tạo một mảng mới không chứa mục cần xóa. Phương thức này không thay đổi mảng ban đầu.
- **Thay thế một mục:** Sử dụng phương thức `map()` để tạo một mảng mới. Trong hàm callback của `map`, bạn có thể trả về một phiên bản mới của mục tại một chỉ mục cụ thể, hoặc giữ nguyên mục cũ.

#### **Cập nhật Đối tượng bên trong Mảng**

#

Thách thức lớn nhất là khi bạn cần cập nhật một đối tượng nằm trong một mảng. Điều này đòi hỏi phải kết hợp cả hai kỹ thuật: bạn cần tạo một mảng mới (sử dụng `map`) và trong quá trình đó, tạo một bản sao mới của đối tượng bạn muốn thay đổi (sử dụng cú pháp spread `...`).

    function handleToggle(artworkId, nextSeen) {
      setList(list.map(artwork => {
        if (artwork.id === artworkId) {
          // Tạo một đối tượng *mới* với các thay đổi
          return { ...artwork, seen: nextSeen };
        } else {
          // Không có thay đổi, trả về đối tượng cũ
          return artwork;
        }
      }));
    }

#### **Đơn giản hóa với Immer**

#

Việc cập nhật các cấu trúc dữ liệu lồng nhau có thể trở nên dài dòng và dễ gây lỗi. Thư viện **Immer** là một giải pháp được công nhận cho vấn đề này. Nó cho phép bạn viết mã trông giống như mã đột biến trực tiếp, nhưng lại xử lý việc tạo các bản sao bất biến một cách an toàn ở phía sau. Đây là một sự đánh đổi: bạn thêm một dependency vào dự án, nhưng đổi lại, bạn nhận được trải nghiệm phát triển tốt hơn và giảm đáng kể mã lặp lại.

### **2.3. Nâng Trạng Thái Lên (Lifting State Up)**

#

Vấn đề cốt lõi mà mẫu "nâng trạng thái lên" giải quyết là nhu cầu chia sẻ và đồng bộ hóa trạng thái giữa các thành phần anh em (sibling components). Khi nhiều thành phần cần phản ánh cùng một trạng thái thay đổi, việc để mỗi thành phần quản lý trạng thái riêng của nó sẽ dẫn đến sự không nhất quán và khó quản lý.

Nguyên tắc chỉ đạo ở đây là tạo ra một **"nguồn chân lý duy nhất" (single source of truth)**. Thay vì sao chép trạng thái được chia sẻ giữa các thành phần, hãy di chuyển nó lên thành phần cha chung gần nhất của chúng.

Quy trình nâng trạng thái lên có thể được tóm tắt trong ba bước rõ ràng:

1.  **Xóa** trạng thái khỏi các thành-phần con. Chúng sẽ không còn quản lý trạng thái cục bộ nữa.
2.  **Truyền** dữ liệu được mã hóa cứng từ thành phần cha chung để kiểm tra xem các thành phần con có hiển thị đúng với các giá trị prop khác nhau hay không.
3.  **Thêm** trạng thái vào thành phần cha chung và truyền nó xuống các thành phần con cùng với các trình xử lý sự kiện. Thành phần cha bây giờ sẽ "sở hữu" trạng thái và chịu trách nhiệm cập nhật nó.

Ví dụ thực tế từ hướng dẫn Tic-Tac-Toe minh họa hoàn hảo quá trình này. Trong trò chơi, quá trình này được áp dụng như sau:

1.  Đầu tiên, trạng thái `value` cục bộ đã được xóa khỏi thành phần `Square`.
2.  Sau đó, các giá trị được mã hóa cứng đã được truyền từ `Board` để xác minh rằng mỗi `Square` có thể hiển thị 'X', 'O' hoặc `null`.
3.  Cuối cùng, thành phần `Board` đã được cung cấp một mảng `squares` trong trạng thái của nó, mảng này sau đó được truyền xuống mỗi `Square` cùng với một trình xử lý sự kiện `onSquareClick`. Điều này cho phép `Board` quản lý trạng thái của toàn bộ bàn cờ và xác định người chiến thắng.

Khi chúng ta nâng trạng thái lên, chúng ta thường chuyển đổi một thành phần từ không được kiểm soát sang được kiểm soát:

- **Thành phần không được kiểm soát (Uncontrolled Component):** Một thành phần quản lý trạng thái riêng của nó trong nội bộ (ví dụ: `Square` ban đầu).
- **Thành phần được kiểm soát (Controlled Component):** Một thành phần nhận giá trị và các lệnh gọi lại để thay đổi giá trị đó thông qua props (ví dụ: `Square` cuối cùng, được `Board` kiểm soát).

\--------------------------------------------------------------------------------

### **Kết luận**

#

Tài liệu hướng dẫn này đã trình bày các khái niệm và thực hành thiết yếu để xây dựng các ứng dụng React chuyên nghiệp, dễ bảo trì và hiệu suất cao. Chúng ta đã khám phá hai lĩnh vực quan trọng: thiết lập một môi trường phát triển mạnh mẽ và làm chủ các mẫu tái cấu trúc trạng thái cốt lõi. Bằng cách áp dụng các nguyên tắc này, các nhà phát triển có thể nâng cao đáng kể chất lượng mã và năng suất của mình.

Chúng tôi đã tái khẳng định ba ý chính:

- **Một môi trường phát triển được thiết lập tốt** với linting và định dạng tự động là nền tảng cho bất kỳ dự án React thành công nào. Nó đảm bảo tính nhất quán, phát hiện lỗi sớm và thúc đẩy sự cộng tác liền mạch.
- **Việc xử lý trạng thái một cách bất biến** là rất quan trọng để có được hành vi ứng dụng có thể dự đoán được và tối ưu hóa hiệu suất. Bằng cách coi trạng thái là chỉ đọc và tạo các bản sao mới thay vì thay đổi trực tiếp, chúng ta tránh được các lỗi khó lường và mở đường cho các tính năng nâng cao.
- **"Nâng trạng thái lên"** là một mẫu cơ bản để quản lý trạng thái được chia sẻ. Bằng cách xác định một "nguồn chân lý duy nhất" trong một thành phần cha chung, chúng ta có thể tạo ra các thành phần phối hợp hoạt động hài hòa với nhau.

Việc áp dụng nhất quán những thực hành này sẽ không chỉ cải thiện chất lượng kỹ thuật của các dự án của bạn mà còn dẫn đến việc viết mã React sạch hơn, rõ ràng hơn và cuối cùng là dễ bảo trì hơn trong dài hạn.
