---
title: "Thêm tính năng xác thực"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

Ở phần này, ta sẽ thực hiện thêm tính năng xác thực người dùng cho ứng dụng. Để thêm xác thực với Amplify ta sử dụng câu lệnh dưới đây:

```html
amplify add auth
```

![API](/images/4-auth/auth-01.png)

Tiến hành deploy lại:

```html
amplify push
```

![API](/images/4-auth/auth-02.png)

Khi deploy hoàn thành, ta sẽ có các dịch vụ xác thực được thiết lập trong Amazon Cognito. Để xem thêm thông tin, chạy lệnh sau:

```html
amplify console auth
```

Chọn User Pool

![API](/images/4-auth/auth-03.png)
