# Shadcn UI Là Gì? Bộ Công Cụ Định Nghĩa Lại Cách Xây Dựng Giao Diện React

Nếu bạn từng làm việc với **React** và cảm thấy mệt mỏi với việc phải **override CSS** phức tạp của các thư viện UI như **Material UI**, **Ant Design**, hay **Chakra UI**, thì **Shadcn UI** có thể là giải pháp bạn đang tìm kiếm. Không giống như các thư viện UI truyền thống, **Shadcn UI** mang đến một cách tiếp cận hoàn toàn mới: bạn không chỉ sử dụng các **component**, mà còn **sở hữu chúng**. Trong bài viết này, mình sẽ giải thích chi tiết **Shadcn UI là gì**, cách nó hoạt động, và tại sao nó đang trở thành xu hướng trong cộng đồng **React**.

## Shadcn UI là gì?

**Shadcn UI** không phải là một **thư viện component** thông thường mà là một **bộ sưu tập các React component** được thiết kế để bạn **sao chép** và **tùy chỉnh** trực tiếp trong dự án của mình. Thay vì cài đặt qua **npm** và phụ thuộc vào một package bên ngoài, **Shadcn UI** cho phép bạn **copy-paste** mã nguồn của các **component** hoặc sử dụng **CLI** để tự động thêm chúng vào dự án. Điều này có nghĩa là bạn **sở hữu hoàn toàn mã nguồn** và có thể chỉnh sửa theo ý muốn mà không cần lo lắng về việc **override style** hay **xung đột phiên bản**.

**Shadcn UI** được xây dựng dựa trên hai công nghệ cốt lõi:

- **Radix UI**: Một thư viện **headless UI** cung cấp các **component** với **accessibility** (a11y) tuyệt vời, tập trung vào hành vi (behavior) mà không áp đặt style.
- **Tailwind CSS**: Một **utility-first CSS framework** giúp styling nhanh chóng và linh hoạt.

Kết hợp hai công nghệ này, **Shadcn UI** mang đến các **component** vừa đẹp, vừa dễ tùy chỉnh, đồng thời đảm bảo **hiệu suất** và **trải nghiệm người dùng** tốt nhất. Ra mắt vào tháng 3/2023, **Shadcn UI** đã nhanh chóng thu hút cộng đồng với gần **70k stars trên GitHub** và được sử dụng bởi nhiều công ty lớn, bao gồm cả **Vercel**.

## Đặc điểm nổi bật của Shadcn UI

**Shadcn UI** không chỉ là một bộ **component** mà còn định nghĩa lại cách bạn xây dựng giao diện. Dưới đây là những đặc điểm nổi bật:

### 1. Tùy chỉnh không giới hạn

Với **Shadcn UI**, bạn **sở hữu mã nguồn** của các **component**. Thay vì phải **override CSS** hay **hack** style của một thư viện bên ngoài, bạn có thể chỉnh sửa trực tiếp file **component** trong dự án của mình. Ví dụ, nếu bạn muốn thay đổi giao diện của **Button**, chỉ cần mở file `button.tsx` và chỉnh sửa theo ý muốn.

### 2. Bundle size tối ưu

Vì bạn chỉ sao chép những **component** cần thiết, **Shadcn UI** không làm tăng kích thước **bundle** của dự án như các thư viện truyền thống. Điều này đặc biệt hữu ích khi bạn chỉ cần một vài **component** thay vì toàn bộ thư viện.

### 3. Không lo xung đột phiên bản

Khi sử dụng các thư viện như **Material UI** hay **Chakra UI**, bạn có thể gặp vấn đề **dependency hell** (xung đột phiên bản). Với **Shadcn UI**, vì mã nguồn nằm trong dự án, bạn không cần lo lắng về việc thư viện bên ngoài cập nhật và làm hỏng ứng dụng của mình.

### 4. CLI thông minh

**Shadcn UI** cung cấp một **CLI** giúp bạn dễ dàng thêm **component** vào dự án. Ví dụ, để thêm **Button**:

```bash
npx shadcn-ui@latest add button

```

Lệnh này sẽ tạo file `components/ui/button.tsx` trong dự án của bạn, sẵn sàng để sử dụng hoặc tùy chỉnh.

### 5. Dễ bảo trì

Mỗi **component** trong **Shadcn UI** là một file độc lập, không phụ thuộc vào package bên ngoài. Nếu bạn cần cập nhật hoặc thay đổi một **component**, chỉ cần chỉnh sửa file tương ứng mà không ảnh hưởng đến phần còn lại của dự án.

## So sánh Shadcn UI với các thư viện UI truyền thống

Để hiểu rõ hơn về **Shadcn UI**, hãy so sánh nó với các thư viện UI truyền thống như **Material UI**, **Ant Design**, hay **Chakra UI**:

- **Thư viện truyền thống**: Giống như bạn thuê một căn nhà được thiết kế sẵn. Bạn có thể sử dụng, nhưng nếu muốn thay đổi cấu trúc (như đập tường, sơn lại), bạn phải làm việc với chủ nhà (override style, hack CSS). Điều này thường phức tạp và tốn thời gian.
- **Shadcn UI**: Giống như bạn mua một bản thiết kế nhà. Bạn sở hữu hoàn toàn và có thể tùy chỉnh mọi thứ theo ý muốn, từ màu sắc đến bố cục, mà không cần xin phép ai.

Ví dụ, với **Material UI**, để thay đổi giao diện của một **Button**, bạn cần sử dụng **sx prop**, **styled()**, hoặc **theme overrides**, và đôi khi vẫn gặp khó khăn nếu style mặc định quá cứng nhắc. Với **Shadcn UI**, bạn chỉ cần mở file `button.tsx` và chỉnh sửa trực tiếp mã nguồn.

## Công nghệ cốt lõi của Shadcn UI

**Shadcn UI** được xây dựng trên các công nghệ hiện đại, đảm bảo chất lượng và hiệu suất:

### 1. Tailwind CSS

**Tailwind CSS** là một **utility-first CSS framework**, cho phép bạn styling nhanh chóng bằng các **class** như `bg-blue-500`, `text-center`, hoặc `p-4`. Với **Shadcn UI**, các **component** sử dụng **Tailwind** để tạo giao diện đẹp mắt và dễ tùy chỉnh.

### 2. Radix UI

**Radix UI** cung cấp các **headless component** (chỉ tập trung vào hành vi, không có style mặc định). Ví dụ, **Radix** cung cấp **Button** với các tính năng như **keyboard navigation**, **focus management**, và **ARIA attributes**, đảm bảo **accessibility** cao. **Shadcn UI** thêm style từ **Tailwind** lên các **component** của **Radix** để tạo ra giao diện hoàn chỉnh.

### 3. TypeScript

**Shadcn UI** được viết bằng **TypeScript**, đảm bảo code chất lượng cao, dễ bảo trì, và hỗ trợ mạnh mẽ cho các nhà phát triển với **type safety**. Điều này đặc biệt hữu ích khi bạn làm việc trong các dự án lớn.

## Ví dụ sử dụng Shadcn UI

Để minh họa cách **Shadcn UI** hoạt động, mình sẽ hướng dẫn cách thêm và sử dụng **Button** trong dự án **React**.

### Bước 1: Cài đặt CLI và khởi tạo dự án

Trước tiên, bạn cần một dự án **React** với **Tailwind CSS** và **TypeScript**. Nếu chưa có, bạn có thể tạo bằng **Vite**:

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install

```

Cài đặt **Tailwind CSS** theo hướng dẫn tại [https://tailwindcss.com/docs/guides/vite](https://tailwindcss.com/docs/guides/vite).

Sau đó, chạy lệnh khởi tạo **Shadcn UI**:

```bash
npx shadcn-ui@latest init

```

CLI sẽ hỏi bạn một số câu hỏi:

- **TypeScript or JavaScript?** Chọn **TypeScript**.
- **Base color?** Chọn màu chủ đạo, ví dụ **Slate**.
- **Global CSS file?** Thường là `app/globals.css`.
- **CSS variables?** Chọn **Yes** để hỗ trợ **dark mode** và tùy chỉnh dễ dàng.

Lệnh này sẽ tạo các file cấu hình như:

- `components.json`: Lưu trữ cấu hình **Shadcn UI**.
- `lib/utils.ts`: Hàm tiện ích để kết hợp **class** Tailwind.

### Bước 2: Thêm component Button

Chạy lệnh để thêm **Button**:

```bash
npx shadcn-ui@latest add button

```

Lệnh này sẽ tạo file `components/ui/button.tsx` trong dự án của bạn. Nội dung file `button.tsx` trông như sau:

```tsx
import * as React from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline:
          "border border-input hover:bg-accent hover:text-accent-foreground",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "underline-offset-4 hover:underline text-primary",
      },
      size: {
        default: "h-10 py-2 px-4",
        sm: "h-9 px-3 rounded-md",
        lg: "h-11 px-8 rounded-md",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, ...props }, ref) => {
    return (
      <button
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    );
  }
);
Button.displayName = "Button";

export { Button, buttonVariants };
```

### Bước 3: Sử dụng Button

Trong file `src/App.tsx`, bạn có thể sử dụng **Button** như sau:

```tsx
import { Button } from "@/components/ui/button";

export default function App() {
  return (
    <div className="p-4">
      <Button variant="default">Click me</Button>
      <Button variant="destructive" className="ml-2">
        Delete
      </Button>
      <Button variant="outline" className="ml-2">
        Outline
      </Button>
    </div>
  );
}
```

### Kết quả

Bạn sẽ thấy các nút với giao diện đẹp, sử dụng style từ **Tailwind CSS**, và có thể tùy chỉnh thêm bằng cách chỉnh sửa `button.tsx` hoặc thêm **className**.

## Ưu điểm của Shadcn UI

**Shadcn UI** mang lại nhiều lợi ích vượt trội so với các thư viện UI truyền thống:

- **Kiểm soát hoàn toàn mã nguồn**: Bạn có thể tùy chỉnh **component** mà không cần lo lắng về **override** hay **breaking changes** từ thư viện.
- **Hiệu suất cao**: Vì chỉ sử dụng các **component** cần thiết, kích thước **bundle** được tối ưu hóa.
- **Tích hợp với AI**: Mã nguồn mở và dễ đọc, rất phù hợp để tích hợp với các công cụ AI như **LLM** (Large Language Models) để tạo hoặc cải tiến **component**.
- **Hỗ trợ đa framework**: **Shadcn UI** hoạt động tốt với **Next.js**, **Vite**, **Astro**, **Remix**, và nhiều framework khác.
- **Cộng đồng mạnh mẽ**: Với gần **70k stars trên GitHub** và được các công ty như **Vercel** sử dụng, **Shadcn UI** có cộng đồng hỗ trợ tích cực.

## Nhược điểm của Shadcn UI

Mặc dù có nhiều ưu điểm, **Shadcn UI** cũng có một số hạn chế:

- **Không phù hợp cho prototyping nhanh**: Nếu bạn cần dựng giao diện nhanh trong vài giờ, các thư viện như **Material UI** hoặc **Chakra UI** sẽ phù hợp hơn vì chúng cung cấp **component** sẵn có mà không cần tùy chỉnh nhiều.
- **Yêu cầu hiểu biết về Tailwind và Radix**: Để tận dụng tối đa **Shadcn UI**, bạn cần quen với **Tailwind CSS** và cách hoạt động của **Radix UI**.
- **Quản lý mã nguồn**: Vì bạn sở hữu mã **component**, bạn phải tự chịu trách nhiệm bảo trì và cập nhật chúng.

## Ứng dụng thực tế của Shadcn UI

**Shadcn UI** đặc biệt phù hợp cho các dự án cần **tùy chỉnh cao** và **hiệu suất tối ưu**. Một số ứng dụng thực tế bao gồm:

- **Hệ thống quản trị (Admin Dashboards)**: Xây dựng **dashboard**, **CRM**, hoặc **ERP** với giao diện tùy chỉnh theo nhu cầu.
- **Ứng dụng cần thiết kế độc đáo**: Khi bạn muốn giao diện của mình khác biệt hoàn toàn so với phong cách mặc định của **Material UI** hay **Ant Design**.
- **Dự án tích hợp AI**: Vì mã nguồn của **Shadcn UI** dễ đọc và tùy chỉnh, nó rất phù hợp để tích hợp với các công cụ AI như **LLM** để tạo **component** hoặc cải tiến giao diện.
- **Dự án mã nguồn mở**: Với tính linh hoạt và cộng đồng mạnh mẽ, **Shadcn UI** là lựa chọn tuyệt vời cho các dự án cộng đồng.

Nhiều công ty, bao gồm **Vercel**, đã sử dụng **Shadcn UI** để xây dựng các giao diện đẹp và hiệu quả. Các **component** phổ biến như **Button**, **Dialog**, **Table**, **Form**, và **Dropdown** được thiết kế để đáp ứng hầu hết nhu cầu phát triển giao diện.

## Kết luận

**Shadcn UI** đang định nghĩa lại cách các nhà phát triển **React** xây dựng giao diện. Với cách tiếp cận **copy-paste**, kết hợp **Radix UI**, **Tailwind CSS**, và **TypeScript**, **Shadcn UI** mang đến sự linh hoạt, hiệu suất, và khả năng tùy chỉnh không giới hạn. Dù không phù hợp cho các dự án cần **prototyping** nhanh, nó là lựa chọn lý tưởng cho các ứng dụng cần giao diện **độc đáo**, **dễ bảo trì**, và **tích hợp AI**.

Nếu bạn muốn nâng tầm giao diện **React** của mình, hãy khám phá **Shadcn UI** tại [https://ui.shadcn.com](https://ui.shadcn.com/). Chỉ với vài lệnh **CLI**, bạn có thể bắt đầu xây dựng các **component** đẹp mắt và hoàn toàn thuộc về bạn!
