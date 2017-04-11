#Android打开系统拍照&相册获取头像

现在许多应用都有上传头像的功能，再次奉上代开系统相册或打开系统相机拍照的实现，有的同学在测试小米手机上打开选择相册有奔溃，此代码已完美解决此问题...

楼主，做的头像需要经过裁剪之后和圆形处理...

##1.设置点击打开相机&打开系统图库

```
				@Override
                public void onClick(View v) {
                    // 打开系统拍照程
                    Intent camera = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                    startActivityForResult(camera, CAMERA);
                }
            

                @Override
                public void onClick(View v) {
                    // 打开系统图库选择图片
                    Intent picture = new Intent(Intent.ACTION_PICK, MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
                    startActivityForResult(picture, PICTURE);
                }
```

##2.重写带结果启动的回调onActivityResult

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode == CAMERA && resultCode == RESULT_OK && data != null) {

            // 拍照
            Bundle bundle = data.getExtras();
            // 获取相机返回的数据，并转换为图片格式
            Bitmap bitmap = (Bitmap) bundle.get("data");
            // bitmap圆形裁剪
//            bitmap = BitmapUtils.zoom(bitmap, DensityUtil.dp2px(this, 62), DensityUtil.dp2px(this, 62));
            bitmap = BitmapUtils.zoom(bitmap, DensityUtil.dp2px(this, 20), DensityUtil.dp2px(this, 20));
            Bitmap circleBitmap = BitmapUtils.circleBitmap(bitmap);

            //TODO 将图片上传到服务器
//            ivIcon.setImageBitmap(circleBitmap);
            uploadHeadpic(bitmap);
            // 将图片保存在本地
            try {
                saveImage(circleBitmap);
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            }
        } else if (requestCode == PICTURE && resultCode == RESULT_OK && data != null) {
            //图库

            String pathResult = null;  // 获取图片路径的方法调用
            try {
                Uri uri = data.getData();
                pathResult = getPath(uri);
                Log.e("TAG", "图片路径===" + pathResult);
            } catch (Exception e) {
                e.printStackTrace();
            }

            //这里返回的uri情况就有点多了
            //**:在4.4.2之前返回的uri是:content://media/external/images/media/3951或者file://....在4.4.2返回的是content://com.android.providers.media.documents/document/image:3951或者
            //总结：uri的组成，eg:content://com.example.project:200/folder/subfolder/etc
            //content:--->"scheme"
            //com.example.project:200-->"host":"port"--->"authority"[主机地址+端口(省略) =authority]
            //folder/subfolder/etc-->"path" 路径部分
            //android各个不同的系统版本,对于获取外部存储上的资源，返回的Uri对象都可能各不一样,所以要保证无论是哪个系统版本都能正确获取到图片资源的话
            //就需要针对各种情况进行一个处理了

            Bitmap decodeFile = BitmapFactory.decodeFile(pathResult);
            Bitmap zoomBitmap = BitmapUtils.zoom(decodeFile, DensityUtil.dp2px(this, 20), DensityUtil.dp2px(this, 20));
            // bitmap圆形裁剪p
            Bitmap circleImage = BitmapUtils.circleBitmap(zoomBitmap);
            // 真实项目当中，是需要上传到服务器的..这步我们就不做了。
            uploadHeadpic(zoomBitmap);
            try {
                // 保存图片到本地
                saveImage(circleImage);
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            }
        }
    }
```

##3.获取图片路径

```
// 根据系统相册选择的文件获取路径
    @SuppressLint("NewApi")
    private String getPath(Uri uri) {
//        int sdkVersion = Build.VERSION.SDK_INT;
        final boolean isKitKat = Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT;
        // 高于4.4.2的版本
//        if (sdkVersion >= 19) {
        if (isKitKat && DocumentsContract.isDocumentUri(mContext, uri)) {
            Log.e("TAG", "uri auth: " + uri.getAuthority());
            if (isExternalStorageDocument(uri)) {
                String docId = DocumentsContract.getDocumentId(uri);
                String[] split = docId.split(":");
                String type = split[0];
                if ("primary".equalsIgnoreCase(type)) {
                    return Environment.getExternalStorageDirectory() + "/" + split[1];
                }
            } else if (isDownloadsDocument(uri)) {
                final String id = DocumentsContract.getDocumentId(uri);
                final Uri contentUri = ContentUris.withAppendedId(Uri.parse("content://downloads/public_downloads"),
                        Long.valueOf(id));
                return getDataColumn(this, contentUri, null, null);
            } else if (isMediaDocument(uri)) {
                final String docId = DocumentsContract.getDocumentId(uri);
                final String[] split = docId.split(":");
                final String type = split[0];

                Uri contentUri = null;
                if ("image".equals(type)) {
                    contentUri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;
                } else if ("video".equals(type)) {
                    contentUri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;
                } else if ("audio".equals(type)) {
                    contentUri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;
                }
                final String selection = "_id=?";
                final String[] selectionArgs = new String[]{split[1]};
                return getDataColumn(this, contentUri, selection, selectionArgs);
            } else if (isMedia(uri)) {
                String[] proj = {MediaStore.Images.Media.DATA};
                Cursor actualimagecursor = this.managedQuery(uri, proj, null, null, null);
                int actual_image_column_index = actualimagecursor.getColumnIndexOrThrow(MediaStore.Images.Media.DATA);
                actualimagecursor.moveToFirst();
                return actualimagecursor.getString(actual_image_column_index);
            }
        } else if ("content".equalsIgnoreCase(uri.getScheme())) {
            // Return the remote address
            if (isGooglePhotosUri(uri))
                return uri.getLastPathSegment();
            return getDataColumn(this, uri, null, null);

        } else if ("file".equalsIgnoreCase(uri.getScheme())) { // File
            return uri.getPath();
        }
        return null;
    }

private String getDataColumn(Context context, Uri uri, String selection, String[] selectionArgs) {
        Cursor cursor = null;
        final String column = "_data";
        final String[] projection = {column};
        try {
            cursor = context.getContentResolver().query(uri, projection, selection, selectionArgs, null);
            if (cursor != null && cursor.moveToFirst()) {
                final int index = cursor.getColumnIndexOrThrow(column);
                return cursor.getString(index);
            }
        } finally {
            if (cursor != null)
                cursor.close();
        }
        return null;
    }


    private boolean isExternalStorageDocument(Uri uri) {
        return "com.android.externalstorage.documents".equals(uri.getAuthority());
    }

    public static boolean isDownloadsDocument(Uri uri) {
        return "com.android.providers.downloads.documents".equals(uri.getAuthority());
    }

    public static boolean isMediaDocument(Uri uri) {
        return "com.android.providers.media.documents".equals(uri.getAuthority());
    }

    public static boolean isMedia(Uri uri) {
        return "media".equals(uri.getAuthority());
    }

    /**
     * @param uri The Uri to check.
     * @return Whether the Uri authority is Google Photos.
     */
    public static boolean isGooglePhotosUri(Uri uri) {
        return "com.google.android.apps.photos.content".equals(uri.getAuthority());
    }
```

以上就是比较完整的代码了...

最后附上2篇参考文章：

![文章1](http://blog.csdn.net/iamwutianbao/article/details/52459509)

![文章2](http://blog.csdn.net/coderinchina/article/details/50799501)

Thanks all.

