---
title: "Thêm xác thực cho API"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 6.4 </b> "
---

Ta có thể update AppSync API để enable authorization modes
Trong ví dụ này, ta sẽ update API sử dụng Cognito và API Key để kích hoạt phân quyền truy cập
Để enable authorization modes, ta thực hiện:

```
amplify update api
```

![API](/images/6-photosharingapp/app-03.png)

Update **_schema_**

```
type Post
  @model
  @auth(
    rules: [
      { allow: owner }
      { allow: public, operations: [read] }
      { allow: private, operations: [read] }
    ]
  ) {
  id: ID!
  name: String!
  location: String!
  description: String!
  image: String
  owner: String
}
```

Deploy lại thay đổi

```
amplify push -y
```

Now, you will have two types of API access:
Bây giờ ta đã có 2 loại API

1. Private (Cognito): User được phép xem toàn bộ bài post. Cho phép tạo mới bài post nếu user đăng nhập thành công. Sau khi đăng nhập, user có thể sửa xoá các bài post của họ.
2. Public (API Key): Bất kì người dùng nào dù chưa đăng nhập cũng có thể query danh sách bài post

Để gọi đến API Private, ta cần thêm **_authMode_** vào câu lệnh query hoặc mutation

```
await client.graphql({
    query: createPost,
    variables: { input: postInfo },
    authMode: "userPool",
});
```

#### Thêm route mới chỉ cho phép hiển thị các bài post của bản thân

Tiếp theo, ta sẽ tạo thêm 1 route mới, route này có tên là My post, chỉ hiển thị các bài post của user đang đăng nhập.

Mở file **_CreatePost.js_** và update như sau

```
async function save() {
    try {
      const { name, description, location, image } = formState;
      if (!name || !description || !location || !image.name) return;
      updateFormState((currentState) => ({ ...currentState, saving: true }));
      const postId = uuid();
      const postInfo = {
        name,
        description,
        location,
        image: formState.image.name,
        id: postId,
      };

      const result = handleUpload(
        formState.image.name,
        formState.image.fileInfo
      );
      // await uploadData(formState.image.name, formState.image.fileInfo);
      console.log("result", result);
      await client.graphql({
        query: createPost,
        variables: { input: postInfo },
        authMode: "userPool",
      });
      updatePosts([
        ...posts,
        { ...postInfo, image: formState.file, owner: user.username },
      ]); // updated
      updateFormState((currentState) => ({ ...currentState, saving: false }));
      updateOverlayVisibility(false);
    } catch (err) {
      console.log("error: ", err);
    }
  }
```

Mở file **_App.js_**, tạo một state mới để lưu trữ các bài viết của user

```
const [myPosts, updateMyPosts] = useState([]);
```

Viết func **_setPostState()_** để set giá trị cho myPosts là các bài post có **_owner = user.username_**

```
async function setPostState(postsArray) {
    const myPostData = postsArray.filter((p) => p?.owner === user?.username);
    updateMyPosts(myPostData);
    updatePosts(postsArray);
  }
```

Thêm router mới

```
<Route exact path="/myposts">
  <Posts posts={myPosts} />
</Route>

```

Thêm link đến route mới tại file **_Header.js_**

```
<Link to="/myposts" className={linkStyle}>
  My Posts
</Link>
```
