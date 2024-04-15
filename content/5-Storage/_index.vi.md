---
title: "Storage Amazon S3"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5. </b> "
---

Để lưu trữ ảnh, ta sẽ sử dụng AWS S3, config S3 như sau:
```html
amplify add storage
```

![API](/images/5-s3/s3-01.png)

Deploy ứng dụng:

```html
amplify push
```

Ta sẽ sử dụng API của S3 để upload và get URL ảnh

#### Upload item

```html
import { uploadData } from "aws-amplify/storage";
const handleUpload = async (key, data) => {
  const operation = uploadData({
    key,
    data,
    options: {
      accessLevel: "guest",
    },
  });

  const result = await operation.result;
};
```

#### Truy xuất item

```html
import { getUrl } from "aws-amplify/storage";

const handleGetUrl = async (key) => {
  const res = await getUrl({
    key,
    options: {
      validateObjectExistence: true,
    },
  });
  return res.url.href;
};
```

Bây giờ chúng ta có thể bắt đầu lưu các hình ảnh vào S3 và tiếp tục xây dựng Photo Sharing App.
