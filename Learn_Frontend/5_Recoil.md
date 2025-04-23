# 🌟 Recoil: Thư Viện Quản Lý State Đột Phá Cho React

## 📖 Recoil là gì?

🚀 **Recoil** là một thư viện quản lý trạng thái (**state management**) mã nguồn mở, được phát triển bởi đội ngũ **Facebook** – những người tạo ra React. Ra mắt vào năm 2020, Recoil mang đến một cách tiếp cận mới mẻ để xử lý trạng thái trong ứng dụng React, đặc biệt khi việc chia sẻ dữ liệu giữa các component trở nên phức tạp.

🔍 Khác với các thư viện như **Redux** hay **MobX**, Recoil được thiết kế để **đơn giản**, **gần gũi với cú pháp React**, và hỗ trợ tốt cho các ứng dụng cần xử lý trạng thái phức tạp hoặc bất đồng bộ. Dù vẫn đang trong giai đoạn thử nghiệm (**experimental**), Recoil đã thu hút sự chú ý lớn với hơn **70k stars trên GitHub** và được sử dụng trong nhiều dự án thực tế.

## ✨ Đặc điểm nổi bật của Recoil

🌈 Recoil mang đến một số tính năng độc đáo, giúp nó nổi bật trong hệ sinh thái quản lý trạng thái:
## ✨ 1. Atom - Đơn vị trạng thái nhỏ nhất

### 🧩 Atom là gì?

**Atom** là đơn vị cơ bản nhất trong Recoil để lưu trữ trạng thái. Mỗi **atom** đại diện cho một mảnh dữ liệu độc lập, chẳng hạn như một số, chuỗi, mảng, hoặc đối tượng. Các component có thể **subscribe** (đăng ký) vào một atom, và khi giá trị của atom thay đổi, Recoil đảm bảo chỉ các component đang sử dụng atom đó được **re-render**, giúp tối ưu hiệu suất.

-   **Cấu trúc của Atom**:

    -   **Key**: Một chuỗi duy nhất để xác định atom trong ứng dụng.

    -   **Default**: Giá trị mặc định của atom khi khởi tạo.

    -   **Tính chất**: Atom là **global state** (trạng thái toàn cục), có thể được truy cập từ bất kỳ component nào nếu component đó đăng ký.

-   **So sánh**:

    -   Trong **Redux**, trạng thái được lưu trong một **store** duy nhất, và bạn cần **action/reducer** để thay đổi.

    -   Trong **Recoil**, mỗi atom là một "mini-store" độc lập, không cần reducer, giúp mã gọn gàng hơn.


### 🚀 Ví dụ về Atom

Giả sử bạn muốn lưu trữ số lượt đếm trong một ứng dụng:

```jsx
// counterState.js
import { atom } from 'recoil';

export const counterState = atom({
  key: 'counterState', // Tên duy nhất
  default: 0, // Giá trị ban đầu
});
```

Component sử dụng atom này:

```jsx
// Counter.js
import { useRecoilState } from 'recoil';
import { counterState } from './counterState';

function Counter() {
  const [count, setCount] = useRecoilState(counterState);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Tăng</button>
      <button onClick={() => setCount(count - 1)}>Giảm</button>
    </div>
  );
}
```

### 🔍 Tại sao Atom quan trọng?

-   **Hiệu suất**: Chỉ component đăng ký atom được re-render khi atom thay đổi, tránh render không cần thiết.

-   **Đơn giản**: Không cần thiết lập store phức tạp như Redux, chỉ cần định nghĩa atom và sử dụng.

-   **Tính module**: Mỗi atom quản lý một phần trạng thái riêng, giúp dễ dàng tổ chức mã trong dự án lớn.


### ⚠️ Lưu ý khi dùng Atom

-   Đảm bảo **key** là duy nhất trong toàn ứng dụng để tránh xung đột.

-   Nếu atom chứa dữ liệu phức tạp (như mảng lớn), hãy chia nhỏ thành nhiều atom để tối ưu hiệu suất.


## 📊 2. Selector - Trạng thái dẫn xuất thông minh

### 🛠️ Selector là gì?

**Selector** là một hàm đặc biệt trong Recoil, dùng để tính toán **trạng thái dẫn xuất** (derived state) dựa trên các **atom** hoặc **selector** khác. Selector hoạt động như một **computed property**, tự động cập nhật khi trạng thái đầu vào thay đổi, giúp giảm mã lặp và tăng tính tái sử dụng.

-   **Cấu trúc của Selector**:

    -   **Key**: Tên duy nhất để xác định selector.

    -   **Get**: Hàm lấy dữ liệu từ atom hoặc selector khác và tính toán giá trị mới.

    -   **Set** (tùy chọn): Cho phép selector hoạt động như một atom có thể ghi (writeable).

-   **So sánh**:

    -   Trong **Redux**, bạn có thể dùng **reselect** để tạo selector, nhưng cần middleware và cấu hình phức tạp.

    -   Trong **Recoil**, selector tích hợp sẵn, sử dụng đơn giản như một hàm.


### 🚀 Ví dụ về Selector

Tiếp tục với ứng dụng đếm, giả sử bạn muốn hiển thị số đếm là **chẵn** hay **lẻ**:

```jsx
// counterState.js
import { selector } from 'recoil';
import { counterState } from './counterState';

export const counterParity = selector({
  key: 'counterParity',
  get: ({ get }) => {
    const count = get(counterState);
    return count % 2 === 0 ? 'Chẵn' : 'Lẻ';
  },
});
```

Sử dụng selector trong component:

```jsx
// CounterParity.js
import { useRecoilValue } from 'recoil';
import { counterParity } from './counterState';

function CounterParity() {
  const parity = useRecoilValue(counterParity);

  return <p>Số đếm là: {parity}</p>;
}
```

### 🔍 Tại sao Selector quan trọng?

-   **Tái sử dụng logic**: Selector cho phép bạn tính toán trạng thái dẫn xuất một lần và dùng lại ở nhiều component.

-   **Tối ưu hiệu suất**: Selector chỉ tính toán lại khi dữ liệu đầu vào thay đổi, nhờ cơ chế caching của Recoil.

-   **Xử lý phức tạp**: Có thể kết hợp nhiều atom hoặc selector để tạo trạng thái phức tạp, như lọc dữ liệu hoặc tính tổng.


### ⚠️ Lưu ý khi dùng Selector

-   **Hiệu suất**: Tránh logic tính toán nặng trong selector, vì nó chạy mỗi khi dữ liệu đầu vào thay đổi.

-   **Writeable selector**: Nếu cần cập nhật ngược trạng thái (như set giá trị cho atom), dùng **set** trong selector, nhưng cần cẩn thận để tránh logic phức tạp.


### 🌐 Ví dụ nâng cao: Selector bất đồng bộ

Recoil hỗ trợ selector bất đồng bộ, rất hữu ích khi lấy dữ liệu từ API:

```jsx
// userState.js
import { selector } from 'recoil';

export const userData = selector({
  key: 'userData',
  get: async () => {
    const response = await fetch('https://api.example.com/user');
    const data = await response.json();
    return data;
  },
});
```

Sử dụng với useRecoilValue:

```jsx
import { useRecoilValue } from 'recoil';
import { userData } from './userState';

function UserProfile() {
  const user = useRecoilValue(userData);

  return <div>Tên: {user.name}</div>;
}
```

## 🪝 3. Cú pháp Hook-centric - Tích hợp mượt mà với React

### 🔧 Cú pháp Hook-centric là gì?

**Cú pháp Hook-centric** đề cập đến cách Recoil sử dụng các **React Hooks** để quản lý trạng thái, thay vì các API phức tạp như trong Redux. Recoil cung cấp một loạt hook như useRecoilState, useRecoilValue, và useSetRecoilState, giúp việc đọc và ghi trạng thái trở nên tự nhiên, giống như sử dụng useState của React.

-   **Các hook chính**:

    -   useRecoilState(atom): Trả về một tuple [value, setValue], tương tự useState, để đọc và ghi trạng thái của atom.

    -   useRecoilValue(atom/selector): Chỉ trả về giá trị của atom hoặc selector, dùng khi component chỉ cần đọc.

    -   useSetRecoilState(atom): Trả về hàm để cập nhật trạng thái, dùng khi component chỉ cần ghi.

    -   useResetRecoilState(atom): Đặt lại atom về giá trị mặc định.

-   **So sánh**:

    -   Trong **Redux**, bạn cần dùng useSelector để đọc và useDispatch để ghi, thường đi kèm action và reducer.

    -   Trong **Recoil**, tất cả được gói gọn trong một hook, giảm boilerplate và dễ học hơn.


### 🚀 Ví dụ về Hook-centric

Sử dụng tất cả các hook trong một ứng dụng:

```jsx
// CounterApp.js
import { useRecoilState, useRecoilValue, useSetRecoilState, useResetRecoilState } from 'recoil';
import { counterState, counterParity } from './counterState';

function CounterApp() {
  const [count, setCount] = useRecoilState(counterState); // Đọc và ghi
  const parity = useRecoilValue(counterParity); // Chỉ đọc
  const setCounter = useSetRecoilState(counterState); // Chỉ ghi
  const resetCounter = useResetRecoilState(counterState); // Reset

  return (
    <div>
      <h1>Count: {count}</h1>
      <p>Parity: {parity}</p>
      <button onClick={() => setCount(count + 1)}>Tăng</button>
      <button onClick={() => setCounter(count - 1)}>Giảm</button>
      <button onClick={resetCounter}>Reset</button>
    </div>
  );
}
```

### 🔍 Tại sao Hook-centric quan trọng?

-   **Gần gũi với React**: Nếu bạn đã quen với useState hoặc useEffect, việc học Recoil rất dễ dàng.

-   **Giảm boilerplate**: Không cần action, reducer, hay middleware như Redux, giúp mã ngắn gọn.

-   **Linh hoạt**: Các hook cho phép bạn tùy chỉnh cách đọc/ghi trạng thái theo nhu cầu của component.


### ⚠️ Lưu ý khi dùng Hook-centric

-   **Quản lý re-render**: Dùng useRecoilValue thay vì useRecoilState khi chỉ cần đọc để tránh re-render không cần thiết.

-   **Error handling**: Khi dùng selector bất đồng bộ, cần xử lý lỗi bằng try-catch hoặc các thư viện như react-query để tăng độ bền vững.

## 🛠️ Cách sử dụng Recoil

Để minh họa cách hoạt động của Recoil, dưới đây là một ví dụ đơn giản về ứng dụng đếm (**counter**) sử dụng Recoil.

### Bước 1: Cài đặt Recoil

📦 Đầu tiên, cài đặt Recoil vào dự án React của bạn:

```bash
npm install recoil

```

### Bước 2: Khởi tạo Atom

🧩 Tạo một **atom** để lưu trữ trạng thái đếm:

```jsx
// counterState.js
import { atom } from 'recoil';

export const counterState = atom({
  key: 'counterState', // Tên duy nhất cho atom
  default: 0, // Giá trị mặc định
});

```

### Bước 3: Sử dụng Atom trong Component

🔧 Tạo component sử dụng **atom** để hiển thị và cập nhật số đếm:

```jsx
// Counter.js
import { useRecoilState } from 'recoil';
import { counterState } from './counterState';

function Counter() {
  const [count, setCount] = useRecoilState(counterState);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Tăng</button>
      <button onClick={() => setCount(count - 1)}>Giảm</button>
    </div>
  );
}

export default Counter;

```

### Bước 4: Kết nối Recoil với ứng dụng

🔗 Bọc ứng dụng của bạn trong `RecoilRoot` để kích hoạt Recoil:

```jsx
// App.js
import { RecoilRoot } from 'recoil';
import Counter from './Counter';

function App() {
  return (
    <RecoilRoot>
      <Counter />
    </RecoilRoot>
  );
}

export default App;

```

### Bước 5: Tạo Selector (nếu cần)

📊 Ví dụ về **selector** để tính trạng thái dẫn xuất (ví dụ: kiểm tra số đếm là chẵn hay lẻ):

```jsx
// counterState.js
import { selector } from 'recoil';
import { counterState } from './counterState';

export const counterParity = selector({
  key: 'counterParity',
  get: ({ get }) => {
    const count = get(counterState);
    return count % 2 === 0 ? 'Chẵn' : 'Lẻ';
  },
});

```

Sử dụng **selector** trong component:

```jsx
// CounterParity.js
import { useRecoilValue } from 'recoil';
import { counterParity } from './counterState';

function CounterParity() {
  const parity = useRecoilValue(counterParity);

  return <p>Số đếm là: {parity}</p>;
}

export default CounterParity;

```

Kết quả: Bạn sẽ có một ứng dụng đếm đơn giản, với số đếm được quản lý bởi **atom** và trạng thái chẵn/lẻ được tính toán bởi **selector**.

## 🏆 Ưu điểm của Recoil

✅ Recoil mang lại nhiều lợi ích cho các dự án React:

-   **Cú pháp đơn giản**: Gần gũi với React Hooks, dễ học cho người quen với React.
-   **Hiệu suất cao**: Chỉ re-render các component liên quan khi trạng thái thay đổi.
-   **Hỗ trợ bất đồng bộ**: Xử lý Promise và API dễ dàng mà không cần middleware phức tạp.
-   **Khả năng mở rộng**: Phù hợp cho cả ứng dụng nhỏ và lớn, tránh vấn đề scalability.
-   **Tính năng nâng cao**: Hỗ trợ du hành thời gian (time travel) và trạng thái bền vững (persistent state), dù ít được sử dụng trong các dự án thông thường.

## ⚠️ Nhược điểm của Recoil

😕 Dù mạnh mẽ, Recoil cũng có một số hạn chế:

-   **Giai đoạn thử nghiệm**: Vẫn là dự án **experimental**, có thể không ổn định trong môi trường production.
-   **Cộng đồng nhỏ hơn**: So với Redux, cộng đồng Recoil còn hạn chế, tài liệu và hỗ trợ chưa phong phú.
-   **Learning curve**: Cách tiếp cận **atom-based** có thể lạ lẫm với người quen dùng Redux hoặc Context API.

## 🌍 Ứng dụng thực tế

💼 Recoil phù hợp cho các dự án React cần:

-   **Chia sẻ trạng thái phức tạp**: Khi nhiều component cần truy cập và cập nhật cùng một mảnh dữ liệu.
-   **Xử lý dữ liệu bất đồng bộ**: Như gọi API hoặc xử lý Promise trong trạng thái.
-   **Ứng dụng lớn**: Nhờ khả năng mở rộng và hiệu suất cao, Recoil lý tưởng cho các dự án có quy mô lớn.

Một số công ty đã thử nghiệm Recoil trong môi trường production, tận dụng sự đơn giản và hiệu quả của nó.

## 🏁 Kết luận

🎉 **Recoil** là một thư viện quản lý trạng thái đầy triển vọng, mang đến cách tiếp cận **atom-based** hiện đại và tích hợp mượt mà với **React Hooks**. Với cú pháp đơn giản, hỗ trợ bất đồng bộ, và khả năng tối ưu hiệu suất, Recoil là lựa chọn đáng cân nhắc cho các dự án React, đặc biệt khi bạn cần xử lý trạng thái phức tạp. Tuy nhiên, vì vẫn trong giai đoạn thử nghiệm, hãy cân nhắc kỹ trước khi sử dụng trong các sản phẩm production.

🔗 Hãy khám phá Recoil tại [https://recoiljs.org](https://recoiljs.org/) và trải nghiệm cách nó đơn giản hóa việc quản lý trạng thái trong React! Để hiểu thêm, bạn có thể xem video của tác giả Recoil, Dave McCabe, tại [https://www.youtube.com/watch?v=\_ISAA_Jt9kI](https://www.youtube.com/watch?v=%5C_ISAA_Jt9kI).
