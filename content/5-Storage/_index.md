---
title: "Image Storage with Amazon S3"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5. </b> "
---

To add image storage, we'll use Amazon S3, which can be configured and created via the Amplify CLI:

```html
amplify add storage
```

![API](/images/5-s3/s3-01.png)

To deploy the service, run the following command:

```html
amplify push
```

To save items to S3, we use the Storage API. The Storage API works like this.

#### Saving an item

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

#### Retrieving an item

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

Now we can start saving images to S3 and we can continue building the Photo Sharing App.
