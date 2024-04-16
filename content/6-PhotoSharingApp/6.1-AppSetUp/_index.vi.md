---
title: "Dựng các component"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 6. </b> "
---

#### Tiến hành tạo các component

Ta cần tạo các component sau trong ứng dụng tại folder **_src_**

- Button.js
- CreatePost.js
- Header.js
- Post.js
- Posts.js

#### Button.js

```
import React from "react";
import { css } from "@emotion/css";

export default function Button({ title, onClick, type = "action" }) {
  return (
    <button className={buttonStyle(type)} onClick={onClick}>
      {title}
    </button>
  );
}

const buttonStyle = (type) => css`
  background-color: ${type === "action" ? "black" : "red"};
  height: 40px;
  width: 160px;
  font-weight: 600;
  font-size: 16px;
  color: white;
  outline: none;
  border: none;
  margin-top: 5px;
  cursor: pointer;
  \:hover {
    background-color: #363636;
  }
`;
```

---

#### Header.js

```
import React from "react";
import { css } from "@emotion/css";
import { Link } from "react-router-dom";

export default function Header() {
  return (
    <div className={headerContainer}>
      <h1 className={headerStyle}>Postagram</h1>
      <Link to="/" className={linkStyle}>
        All Posts
    </div>
  );
}

const headerContainer = css`
  padding-top: 20px;
`;

const headerStyle = css`
  font-size: 40px;
  margin-top: 0px;
`;

const linkStyle = css`
  color: black;
  font-weight: bold;
  text-decoration: none;
  margin-right: 10px;
  \:hover {
    color: #058aff;
  }
`;

```

#### Posts.js

Tiếp theo, ta cần tạo component **_Posts_** để render danh sách các bài post. Component này sẽ nhận 1 props là một array **_posts_** và hiển thị array đó

```
import React from "react";
import { css } from "@emotion/css";
import { Link } from "react-router-dom";

export default function Posts({ posts = [] }) {
  return (
    <>
      <h1>Posts</h1>
      {posts.map((post) => (
        <Link to={`/post/${post?.id}`} className={linkStyle} key={post?.id}>
          <div key={post?.id} className={postContainer}>
            <h1 className={postTitleStyle}>{post?.name}</h1>
            <img alt="post" className={imageStyle} src={post?.image} />
          </div>
        </Link>
      ))}
    </>
  );
}

const postTitleStyle = css`
  margin: 15px 0px;
  color: #0070f3;
`;

const linkStyle = css`
  text-decoration: none;
`;

const postContainer = css`
  border-radius: 10px;
  padding: 1px 20px;
  border: 1px solid #ddd;
  margin-bottom: 20px;
  \:hover {
    border-color: #0070f3;
  }
`;

const imageStyle = css`
  width: 100%;
  max-width: 400px;
`;
```

---

#### CreatePost.js

Tiếp theo là component **_CreatePost_**. Đây là một modal cho phép tạo mới post. Component nhận các props sau:

- **_updateOverlayVisibility_** - Hiển thị / Ẩn modal
- **_updatePosts_** - Func cho phép update danh sách bài posts
- **_posts_** - Danh sách posts ban đầu

Component này sẽ hoạt động như sau:

- Đầu tiên ta tạo 1 state để lưu dữ liệu form, khởi tạo giá trị cho state
- Func **_onChangeText_** để update form state khi người dùng input vào các field name, description, location
- Func **_onChangeImage_** để update form state cho trường image
- Func **_save_** sẽ hoạt động như sau:
  1. Đầu tiên check required tất cả các trường
  2. Update saving state để hiển thị loading
  3. Tạo unique id cho post bằng thư viện **_uuid_**
  4. Tạo object post để gọi API tạo mới post
  5. Gọi func **_uploadData()_** để upload ảnh lên S3
  6. Gọi API tạo bài post
  7. Update local state, close popup

```
import React, { useState } from "react";
import { css } from "@emotion/css";
import Button from "./Button";
import { v4 as uuid } from "uuid";
import { createPost } from "./graphql/mutations";
import { generateClient } from "aws-amplify/api";
import { uploadData } from "aws-amplify/storage";
/* Initial state to hold form input, saving state */
const initialState = {
  name: "",
  description: "",
  image: {},
  file: "",
  location: "",
  saving: false,
};

export default function CreatePost({
  updateOverlayVisibility,
  updatePosts,
  posts,
  user,
}) {
  /* 1. Create local state with useState hook */
  const [formState, updateFormState] = useState(initialState);
  const client = generateClient();

  /* 2. onChangeText handler updates the form state when a user types into a form field */
  function onChangeText(e) {
    e.persist();
    updateFormState((currentState) => ({
      ...currentState,
      [e.target.name]: e.target.value,
    }));
  }

  /* 3. onChangeFile handler will be fired when a user uploads a file  */
  function onChangeFile(e) {
    e.persist();
    if (!e.target.files[0]) return;
    const fileExtPosition = e.target.files[0].name.search(/.png|.jpg|.gif/i);
    const firstHalf = e.target.files[0].name.slice(0, fileExtPosition);
    const secondHalf = e.target.files[0].name.slice(fileExtPosition);
    const fileName = firstHalf + "_" + uuid() + secondHalf;
    const image = { fileInfo: e.target.files[0], name: fileName };
    updateFormState((currentState) => ({
      ...currentState,
      file: URL.createObjectURL(e.target.files[0]),
      image,
    }));
  }

  const handleUpload = async (key, data) => {
    // Upload a file with access level `guest` as  the equivalent of `public` in v5
    const operation = uploadData({
      key,
      data,
      options: {
        accessLevel: "guest",
      },
    });

    const result = await operation.result;
    return result;
  };

  /* 4. Save the post  */
  async function save() {
    console.log("save");
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
      await client.graphql({
        query: createPost,
        variables: { input: postInfo },
      });
      updatePosts([...posts, { ...postInfo, image: formState.file }]);
      updateFormState((currentState) => ({ ...currentState, saving: false }));
      updateOverlayVisibility(false);
    } catch (err) {
      console.log("error: ", err);
    }
  }

  return (
    <div className={containerStyle}>
      <input
        placeholder="Post name"
        name="name"
        className={inputStyle}
        onChange={onChangeText}
      />
      <input
        placeholder="Location"
        name="location"
        className={inputStyle}
        onChange={onChangeText}
      />
      <input
        placeholder="Description"
        name="description"
        className={inputStyle}
        onChange={onChangeText}
      />
      <input type="file" onChange={onChangeFile} />
      {formState.file && (
        <img className={imageStyle} alt="preview" src={formState.file} />
      )}
      <Button title="Create New Post" onClick={save} />
      <Button
        type="cancel"
        title="Cancel"
        onClick={() => updateOverlayVisibility(false)}
      />
      {formState.saving && <p className={savingMessageStyle}>Saving post...</p>}
    </div>
  );
}

const inputStyle = css`
  margin-bottom: 10px;
  outline: none;
  padding: 7px;
  border: 1px solid #ddd;
  font-size: 16px;
  border-radius: 4px;
`;

const imageStyle = css`
  height: 120px;
  margin: 10px 0px;
  object-fit: contain;
`;

const containerStyle = css`
  display: flex;
  flex-direction: column;
  width: 400px;
  height: 420px;
  position: fixed;
  left: 0;
  border-radius: 4px;
  top: 0;
  margin-left: calc(50vw - 220px);
  margin-top: calc(50vh - 230px);
  background-color: white;
  border: 1px solid #ddd;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 0.125rem 0.25rem;
  padding: 20px;
`;

const savingMessageStyle = css`
  margin-bottom: 0px;
`;

```

---

#### Post.js

Tiếp theo ta sẽ tạo component **_Post_**

Component này sẽ lấy id từ url parameter, gọi API get detail post và hiển thị detail bài posst

```
// Post.js

import React, { useState, useEffect } from "react";
import { css } from "@emotion/css";
import { useParams } from "react-router-dom";
import { getPost } from "./graphql/queries";
import { generateClient } from "aws-amplify/api";
import { getUrl } from "aws-amplify/storage";

export default function Post() {
  const [loading, updateLoading] = useState(true);
  const [post, updatePost] = useState(null);
  const client = generateClient();

  const { id } = useParams();
  useEffect(() => {
    fetchPost();
  }, []);

  const handleGetUrl = async (key) => {
    const url = await getUrl({
      key,
      options: {
        validateObjectExistence: true,
      },
    });
    return url.url.href;
  };
  async function fetchPost() {
    try {
      const postData = await client.graphql({
        query: getPost,
        variables: { id },
      });
      const currentPost = postData.data.getPost;
      const image = await handleGetUrl(currentPost.image);
      currentPost.image = image;
      updatePost(currentPost);
      updateLoading(false);
    } catch (err) {
      console.log("error: ", err);
    }
  }
  if (loading) return <h3>Loading...</h3>;
  return (
    <>
      <h1 className={titleStyle}>{post.name}</h1>
      <h3 className={locationStyle}>{post.location}</h3>
      <p>{post.description}</p>
      <img alt="post" src={post.image} className={imageStyle} />
    </>
  );
}

const titleStyle = css`
  margin-bottom: 7px;
`;

const locationStyle = css`
  color: #0070f3;
  margin: 0;
`;

const imageStyle = css`
  max-width: 500px;
  @media (max-width: 500px) {
    width: 100%;
  }
`;

```
