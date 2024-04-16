---
title: "Clean up resources"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7.</b> "
---

#### Xoá một services

Khi kết thúc bài thực hành này, để xoá 1 service ta sẽ thực hiện lệnh sau

```
amplify remove auth

amplify push

```

Để kiểm tra cách dịch vụ đang được kích hoạt, ta sử dụng câu lệnh:

```
amplify status
```

Danh sách các dịch vụ đang được sử dụng sẽ được hiển thị

#### Xoá toàn bộ service

Để xoá toàn bộ service được liên kết, ta sử dụng lệnh sau:

```
amplify delete
```
