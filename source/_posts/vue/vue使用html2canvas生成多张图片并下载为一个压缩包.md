---
title: vue使用html2canvas生成多张图片并下载为一个压缩包
date: 2023-8-31 16:50:46
tags: ["vue","html2canvas","vue生成压缩包"]
categories: ["vue"]
---

### html代码

```html
<div :id="'printMe'+index" v-for="(item,index) in studentinfo" :key="item.studentId" :ref="'content'+index">
    //页面部分的代码
</div>
<button @click="printImage()">下载按钮</button>

```

```vue
<script>
	//注：这三个插件需要先使用npm安装
	import html2canvas from 'html2canvas';
  	import JSZip from 'jszip'
  	import FileSaver from "file-saver";
  	export default{
  		data(){
			return{
				...
			}
		},
		methods:{
			printImage(page) {
		        let t = this;
		        //生成图片
		        t.studentinfo.forEach((item, index) => {
		          var ref = t.$refs['content' + index];
		          console.log(ref[0])
		          html2canvas(ref[0], {
		              backgroundColor: '#f5f5f9',
		              useCORS: true, // 解决文件跨域问题
		              scrollY: 0,
		              scrollX: 0
		            })
		            .then(canvas => {
		              var dataURL = canvas.toDataURL('image/png');
		              t.finalCanvas.push({
		                fileUrl: dataURL,
		                renameFileName: '准考证' + index + '.png'
		              })
		              //注意 文件名不可重复 如果重复zip包里只会有一张图片
		              if(生成图片结束后的判断){
						this.filesToRar()
					  }
		            })
		            .catch(err => {});
		        })
      		},
      		//生成图片结束后 下载zip包
      		filesToRar() {
		        let _this = this;
		        let zip = new JSZip();
		        let cache = {};
		        let promises = [];
		
		        var arrImages = _this.finalCanvas;
		        console.log(arrImages)
		        for (let item of arrImages) {
		          const promise = _this.getImgArrayBuffer(item.fileUrl).then(data => {
		            // 下载文件, 并存成ArrayBuffer对象(blob)
		            zip.file(item.renameFileName, data, {
		              binary: true
		            }); // 逐个添加文件
		            // zip.file(item.renameFileName, data, {base64: true}); // 逐个添加文件
		            cache[item.renameFileName] = data;
		          });
		          promises.push(promise);
		        }
		
		        Promise.all(promises).then(() => {
		          zip.generateAsync({
		            type: "blob"
		          }).then(content => {
		            // 生成二进制流
		            FileSaver.saveAs(content, '准考证图片列表'); // 利用file-saver保存文件  自定义文件名
		          });
		        }).catch(res => {
		          _this.$message.error('文件压缩失败');
		        });
		    },
		    //获取文件blob
      		getImgArrayBuffer(url) {
		        let _this = this;
		        return new Promise((resolve, reject) => {
		          //通过请求获取文件blob格式
		          let xmlhttp = new XMLHttpRequest();
		          xmlhttp.open("GET", url, true);
		          xmlhttp.responseType = "blob";
		          xmlhttp.onload = function() {
		            if (this.status == 200) {
		              resolve(this.response);
		            } else {
		              reject(this.status);
		            }
		          }
		          xmlhttp.send();
		        });
		
		    },
		}
  	}
</script>

```
