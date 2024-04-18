---
title: "Hosting"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 6.5 </b> "
---

Bên cạnh đó, Amplify hỗ trợ CI/CD rất dễ dàng

Đầu tiên, ta cần tạo 1 Github repo sau đó làm theo hướng dẫn để đẩy toàn bộ source code của ứng dụng lên Github

```
git init

git remote add origin git@github.com\:username/project-name.git

git add .

git commit -m 'initial commit'

git push origin master

```

Mở Amplify console

```
amplify console
```

Chọn tab Hosting Environments, Chọn Github và click Connect branch.
![Deploy](/images/6-photosharingapp/app-04.png)

Authorize Github repo

Chọn repo và chọn nhánh master, click Next
![Deploy](/images/6-photosharingapp/app-05.png)

Chọn role và chọn Environments, click Next
![Deploy](/images/6-photosharingapp/app-06.png)

Cuối cùng, chọn Save and Deploy để deploy ứng dụng
![Deploy](/images/6-photosharingapp/app-07.png)

Quá trình deploy có thể mất vài phút
![Deploy](/images/6-photosharingapp/app-08.png)

Hoàn thành quá trình deploy, ta có thể xem thành quả của mình tại domain mới được tạo ra: https://main.d2svtmsd73lg46.amplifyapp.com/
