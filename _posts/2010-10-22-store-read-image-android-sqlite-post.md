---
layout: post
title: Store and read an image on Android SQlite
tags: android sqlite image
---

**{{ page.title }}**
{{ post.date | date: "%B %e, %Y" }}

To store an image file inside your SQLite db you should use a Blog field, and due Android SQlite limitations you should store info in this way:

<pre><code class="java">
ContentValues cv = new ContentValues();
//your table fields goes here
...
// adding the bitmap in byte array way to the blob field
ByteArrayOutputStream out = new ByteArrayOutputStream();
friendInfo.getImage_bmp().compress(Bitmap.CompressFormat.PNG, 100,out);
cv.put("avatar_img", out.toByteArray());
db.insert(TABLE_NAME, null, cv);
</code></pre>

and to read it you can do:

<pre><code class="java">
byte[] blob = cur.getBlob(cur.getColumnIndex("avatar_img"));
Bitmap bmp = BitmapFactory.decodeByteArray(blob, 0,blob.length, opts);
friend.setImage_bmp(bmp);
</code></pre>

`friend` is an instance from a class, having a Bitmap property called Image_bmp ;)
