# 🌟 Hướng Dẫn Sử Dụng Zustand Trong Next.js

## 📖 Zustand là gì?

🚀 **Zustand** là một thư viện quản lý trạng thái (**state management**) siêu nhẹ cho **React**, được phát triển bởi nhóm **pmndrs**. Với kích thước chỉ khoảng **1KB** (nén gzip), Zustand cung cấp API đơn giản, linh hoạt, và hiệu suất cao, giúp bạn quản lý trạng thái trong ứng dụng **React** hoặc **Next.js** mà không cần viết mã phức tạp như **Redux**. Nếu bạn đang tìm kiếm một giải pháp thay thế cho **Context API** hoặc **Redux** để quản lý trạng thái toàn cục, Zustand là lựa chọn lý tưởng!

🔍 **Tại sao chọn Zustand?**

-   **Nhẹ và nhanh**: Kích thước nhỏ, không làm phình to bundle ứng dụng.
-   **API đơn giản**: Không cần **reducer**, **action**, hay **middleware** như Redux.
-   **Tối ưu render**: Tránh re-render không cần thiết nhờ cơ chế chọn lọc trạng thái.
-   **Hỗ trợ TypeScript**: Định nghĩa kiểu sẵn có, không cần cài thêm package.
-   **Linh hoạt**: Phù hợp cho cả dự án nhỏ và lớn, từ ứng dụng cá nhân đến hệ thống phức tạp.

Trong bài viết này, mình sẽ hướng dẫn từng bước cách tích hợp **Zustand** vào dự án **Next.js**, kèm ví dụ thực tế và mẹo xử lý các vấn đề như **SSR hydration**. Hãy cùng bắt đầu nào! 🎉

## 🛠️ Cài đặt Zustand trong Next.js

### Bước 1: Tạo dự án Next.js

📦 Nếu bạn chưa có dự án **Next.js**, hãy tạo một dự án mới bằng lệnh:

```bash
npx create-next-app@latest my-zustand-app
cd my-zustand-app

```

Chọn các tùy chọn như **TypeScript**, **ESLint**, và **App Router** nếu muốn (mình sẽ dùng **App Router** trong bài này).

### Bước 2: Cài đặt Zustand

🔧 Cài đặt **Zustand** bằng npm hoặc yarn:

```bash
npm install zustand
# hoặc
yarn add zustand

```

🌟 Điểm tuyệt vời là **Zustand** đã tích hợp sẵn hỗ trợ **TypeScript**, nên bạn không cần cài thêm package định nghĩa kiểu.

## 🧩 Tạo Store với Zustand

### 📚 Cấu trúc cơ bản của Store

Trong **Zustand**, **store** là một **hook** lưu trữ trạng thái và các hàm để cập nhật trạng thái (**action**). Bạn có thể tạo nhiều store tùy theo nhu cầu. Cấu trúc cơ bản như sau:

```tsx
import { create } from 'zustand';

interface State {
  // Định nghĩa trạng thái
}

const useStore = create<State>((set) => ({
  // Khởi tạo trạng thái và action
}));

export default useStore;

```

### 🚀 Ví dụ: Store quản lý số đếm

Hãy tạo một store để quản lý một biến đếm (**counter**) và các hành động tăng/giảm:

```tsx
// lib/store.ts
import { create } from 'zustand';

interface CounterState {
  count: number;
  increment: () => void;
  decrement: () => void;
}

const useCounterStore = create<CounterState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

export default useCounterStore;

```

🔍 **Giải thích**:

-   `create`: Hàm chính của Zustand để tạo store.
-   `set`: Hàm cập nhật trạng thái, nhận trạng thái hiện tại và trả về trạng thái mới.
-   `CounterState`: Giao diện TypeScript định nghĩa kiểu cho trạng thái và action.
-   `increment` **và** `decrement`: Các hàm action để thay đổi trạng thái.

### 🌐 Sử dụng Store trong Component

Giờ hãy sử dụng store này trong một component:

```tsx
// app/page.tsx
'use client';

import useCounterStore from '@/lib/store';

export default function Home() {
  const { count, increment, decrement } = useCounterStore();

  return (
    <div className="flex flex-col items-center justify-center min-h-screen">
      <h1 className="text-4xl font-bold">Count: {count}</h1>
      <div className="mt-4 space-x-4">
        <button
          onClick={increment}
          className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          Tăng
        </button>
        <button
          onClick={decrement}
          className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
        >
          Giảm
        </button>
      </div>
    </div>
  );
}

```

🎉 **Kết quả**: Bạn sẽ thấy một giao diện với số đếm và hai nút để tăng/giảm. Zustand tự động quản lý trạng thái và chỉ re-render khi cần.

## 📈 Tích hợp Zustand với TypeScript

🌟 **Zustand** hỗ trợ **TypeScript** cực tốt. Trong ví dụ trên, chúng ta đã dùng interface `CounterState` để định nghĩa kiểu. Nếu muốn thêm các kiểu phức tạp hơn, bạn có thể làm như sau:

```tsx
// lib/store.ts
import { create } from 'zustand';

interface User {
  id: number;
  name: string;
}

interface UserState {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

const useUserStore = create<UserState>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));

export default useUserStore;

```

Sử dụng trong component:

```tsx
// app/profile.tsx
'use client';

import useUserStore from '@/lib/store';

export default function Profile() {
  const { user, setUser, clearUser } = useUserStore();

  return (
    <div className="p-4">
      {user ? (
        <div>
          <p>Xin chào, {user.name} (ID: {user.id})</p>
          <button
            onClick={clearUser}
            className="mt-2 px-4 py-2 bg-red-500 text-white rounded"
          >
            Đăng xuất
          </button>
        </div>
      ) : (
        <button
          onClick={() => setUser({ id: 1, name: 'Nguyen Van A' })}
          className="px-4 py-2 bg-green-500 text-white rounded"
        >
          Đăng nhập
        </button>
      )}
    </div>
  );
}

```

🔧 **Lợi ích**: TypeScript đảm bảo bạn không vô tình truyền sai kiểu dữ liệu, giúp mã an toàn và dễ bảo trì.

## 🌍 Xử lý SSR trong Next.js với Zustand

### ⚠️ Vấn đề Hydration trong SSR

Khi dùng **Next.js** với **Server-Side Rendering (SSR)**, trạng thái khởi tạo trên server và client có thể không khớp, dẫn đến lỗi **hydration**. Ví dụ, nếu bạn lấy dữ liệu từ API trên server và khởi tạo trạng thái, client cần đồng bộ trạng thái đó.

### 🚀 Giải pháp: Sử dụng getServerSideProps

Dùng `getServerSideProps` để truyền dữ liệu từ server vào store:

```tsx
// lib/store.ts
import { create } from 'zustand';

interface DataState {
  data: string[];
  setData: (data: string[]) => void;
}

const useDataStore = create<DataState>((set) => ({
  data: [],
  setData: (data) => set({ data }),
}));

export default useDataStore;

```

Tích hợp trong page:

```tsx
// app/data/page.tsx
'use client';

import useDataStore from '@/lib/store';

export default function DataPage({ initialData }: { initialData: string[] }) {
  const { data, setData } = useDataStore();

  // Đồng bộ dữ liệu ban đầu từ server
  useEffect(() => {
    setData(initialData);
  }, [initialData, setData]);

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold">Dữ liệu từ Server</h1>
      <ul>
        {data.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export async function getServerSideProps() {
  // Giả lập lấy dữ liệu từ API
  const initialData = ['Item 1', 'Item 2', 'Item 3'];

  return {
    props: {
      initialData,
    },
  };
}

```

🔍 **Giải thích**:

-   `getServerSideProps`: Lấy dữ liệu trên server và truyền vào props.
-   `useEffect`: Đồng bộ dữ liệu từ props vào store khi component mount trên client.
-   **Kết quả**: Tránh lỗi hydration, đảm bảo trạng thái đồng bộ giữa server và client.

## 🎨 Mẹo tối ưu khi sử dụng Zustand

### 1. Chia nhỏ Store

📌 Nếu dự án lớn, hãy chia trạng thái thành nhiều store để dễ quản lý:

```tsx
// lib/counterStore.ts
import { create } from 'zustand';

interface CounterState {
  count: number;
  increment: () => void;
}

export const useCounterStore = create<CounterState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

// lib/userStore.ts
import { create } from 'zustand';

interface UserState {
  user: string | null;
  setUser: (user: string) => void;
}

export const useUserStore = create<UserState>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));

```

### 2. Tối ưu Render với Selector

🔧 Để tránh re-render không cần thiết, chỉ lấy những phần trạng thái cần thiết:

```tsx
// app/optimized/page.tsx
'use client';

import { useCounterStore } from '@/lib/store';

export default function OptimizedCounter() {
  const count = useCounterStore((state) => state.count); // Chỉ lấy count

  return <div>Count: {count}</div>;
}

```

### 3. Sử dụng Middleware

🌐 **Zustand** hỗ trợ middleware như `persist` để lưu trạng thái vào **localStorage**:

```tsx
// lib/store.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface CounterState {
  count: number;
  increment: () => void;
}

const useCounterStore = create<CounterState>()(
  persist(
    (set) => ({
      count: 0,
      increment: () => set((state) => ({ count: state.count + 1 })),
    }),
    {
      name: 'counter-storage', // Tên key trong localStorage
    }
  )
);

export default useCounterStore;

```

🔍 **Kết quả**: Trạng thái sẽ được lưu và khôi phục khi tải lại trang.

## 🏆 Ưu điểm của Zustand trong Next.js

✅ **Zustand** mang lại nhiều lợi ích:

-   **Nhẹ và hiệu quả**: Kích thước nhỏ, không làm chậm ứng dụng.
-   **Dễ tích hợp**: Chỉ cần vài dòng mã để bắt đầu.
-   **Hỗ trợ SSR**: Dễ dàng đồng bộ trạng thái với Next.js.
-   **TypeScript-ready**: Định nghĩa kiểu rõ ràng, giảm lỗi khi phát triển.
-   **Cộng đồng mạnh**: Được sử dụng rộng rãi, tài liệu phong phú.

## ⚠️ Nhược điểm cần lưu ý

😕 Một số hạn chế của **Zustand**:

-   **Không phù hợp cho logic siêu phức tạp**: Với các dự án cần middleware mạnh mẽ, **Redux Toolkit** có thể phù hợp hơn.
-   **Learning curve nhỏ**: Nếu bạn mới làm quen với quản lý trạng thái, cần thời gian để hiểu cách tổ chức store.

## 🌍 Ứng dụng thực tế

💼 **Zustand** lý tưởng cho:

-   **Ứng dụng thương mại điện tử**: Quản lý giỏ hàng, thông tin người dùng.
-   **Dashboard quản trị**: Xử lý dữ liệu động, trạng thái biểu đồ.
-   **Dự án cá nhân**: Nhanh chóng xây dựng prototype với trạng thái toàn cục.

Nhiều dự án thực tế sử dụng **Zustand** nhờ tính đơn giản và hiệu suất cao, đặc biệt trong các ứng dụng **Next.js** cần SSR hoặc SSG.

## 🏁 Kết luận

🎉 **Zustand** là một công cụ mạnh mẽ, giúp quản lý trạng thái trong **Next.js** trở nên đơn giản và hiệu quả. Với API trực quan, hỗ trợ **TypeScript**, và khả năng tích hợp mượt mà với **SSR**, Zustand là lựa chọn tuyệt vời để thay thế **Redux** hoặc **Context API**. Từ việc tạo store, sử dụng trong component, đến xử lý hydration, bạn chỉ cần vài bước để có một hệ thống trạng thái mạnh mẽ.

🔗 Hãy thử **Zustand** trong dự án **Next.js** của bạn tại [https://zustand-demo.pmnd.rs](https://zustand-demo.pmnd.rs/) hoặc tham khảo tài liệu chính thức tại [https://docs.pmnd.rs/zustand/getting-started/introduction](https://docs.pmnd.rs/zustand/getting-started/introduction). Bắt đầu ngay hôm nay và cảm nhận sự khác biệt
