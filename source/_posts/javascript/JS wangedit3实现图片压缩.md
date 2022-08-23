---
title: JS wangedit3实现图片压缩
date: 2022-08-19 14:48:27
tags: ["wangedit","图片压缩"]
categories: ["wangedit"]
---

```javascript
import {compressAccurately} from "image-conversion";
import {ediotImgUpload} from "@/api/upload"

 editorConfig.MENU_CONF['uploadImage'] = {
    // 自定义上传
    async customUpload(resultFiles, insertImgFn) {
        if (resultFiles.size / 1024 > 200) { // 大于 200 kb 就压缩
            compressAccurately(resultFiles, 200).then(res => {
                let formData = new FormData();
                formData.append('file', res);
                ediotImgUpload(formData).then(data=>{
                    if(data.code === 200){
                        insertImgFn(import.meta.env.VITE_APP_BASE_URL + data.fileName)
                    }
                })
            })
        }else{
            let formData = new FormData();
            formData.append('file', resultFiles);
            ediotImgUpload(formData).then(data=>{
                if(data.code === 200){
                    insertImgFn(import.meta.env.VITE_APP_BASE_URL + data.fileName)
                }
            })
        }
    }
}
```
