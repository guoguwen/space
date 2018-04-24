---
title: Vue 编辑器quill-edit的扩展
date: 2018-04-24 15:36:07
tags: 
    - Vue组件封装
---

##### 做后台管理系统少不了编辑器，找一款易于根据项目扩展的编辑器势在必行，现在正用Vue做项目，当然用
##### Vue封装的组件更能融为一体

## 安装vue-quill-editor

``` bash
$ npm install vue-quill-editor --save
```
## use

``` bash
$ import Vue from 'vue'
$ import VueQuillEditor from 'vue-quill-editor'

Vue.use(VueQuillEditor)


<quill-editor ref="myTextEditor"
              :content="content"
              :config="editorOption"
              @change="onEditorChange($event)">
</quill-editor>


// editor option example:
export default {
  data () {
    return {
      content: '<h2>I am Example</h2>',
      editorOption: {
       // something config
      }
    }
  },
  // if you need to manually control the data synchronization, parent component needs to explicitly emit an event instead of relying on implicit binding
  // 如果需要手动控制数据同步，父组件需要显式地处理changed事件
  methods: {
    onEditorBlur(editor) {
      console.log('editor blur!', editor)
    },
    onEditorFocus(editor) {
      console.log('editor focus!', editor)
    },
    onEditorReady(editor) {
      console.log('editor ready!', editor)
    },
    onEditorChange({ editor, html, text }) {
      // console.log('editor change!', editor, html, text)
      this.content = html
    }
  },
  // if you need to get the current editor object, you can find the editor object like this, the $ref object is a ref attribute corresponding to the dom redefined
  // 如果你需要得到当前的editor对象来做一些事情，你可以像下面这样定义一个方法属性来获取当前的editor对象，实际上这里的$refs对应的是当前组件内所有关联了ref属性的组件元素对象
  computed: {
    editor() {
      return this.$refs.myTextEditor.quillEditor
    }
  },
  mounted() {
    // you can use current editor object to do something(editor methods)
    console.log('this is my editor', this.editor)
    // this.editor to do something...
  }
}
```

使用过程中vue-quill-editor默认插入图片是直接将图片转化为base64放进内容的,如果图片比较大可想而知，所以有了将图片上传服务器将地址存进内容返回回来

## 改写图片上传组件（基于ElementUI）
``` bash
<quill-editor ref="myTextEditor" v-model="content" :config="editorOption"                     @blur="onEditorBlur($event)" @focus="onEditorFocus($event)"
    @ready="onEditorReady($event)">
</quill-editor>
<el-upload style="display:none" name="img" :headers="uploaderHeader"  :action="apiurl"      :show-file-list="false" :before-upload='newEditorbeforeupload'                            :on-success='newEditorSuccess'ref="uniqueId" id="uniqueId">
</el-upload >

<style>
.image-uploader{
    height: 105px;
  }
  .image-uploader .el-upload {
    border: 1px solid #d9d9d9;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  .image-uploader .el-upload:hover {
    border-color: #20a0ff;
  }

  .image-uploader .image-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 187px;
    height: 105px;
    line-height: 105px;
    text-align: center;
  }

  .image-uploader .image-show {
    width: 187px;
    height: 105px;
    display: block;
  }

  .image-uploader.new-image-uploader {
    font-size: 28px;
    color: #8c939d;
    width: 165px;
    height: 105px;
    line-height: 105px;
    text-align: center;
  }

  .image-uploader.new-image-uploader .image-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 165px;
    height: 105px;
    line-height: 105px;
    text-align: center;
  }

  .image-uploader.new-image-uploader .image-show {
    width: 165px;
    height: 105px;
    display: block;
  }
</style>
    data(){
        return 
        content: '',
        editorOption: {
          // something config
        },
    }
    newEditorbeforeupload(file){                       
              const isJPG = file.type === 'image/jpeg' ||file.type ===  'image/png';
              const isLt2M = file.size / 1024 / 1024 < 2;
              if (!isJPG) {
                  this.$message.error('上传图片只能是 JPG或PNG 格式!');
              }
              if (!isLt2M) {
                  this.$message.error('上传图片大小不能超过 2MB!');
              }
          if(isJPG && isLt2M)this.imageLoading =true
                return isJPG && isLt2M;                                            
    },
    newEditorSuccess(response, file, fileList){
        if(response.errno===0){                        
            this.addImgRange = this.$refs.myTextEditor.quill.getSelection()
            this.$refs.myTextEditor.quill.insertEmbed(this.addImgRange != null?this.addImgRange.index:0, 'image',response.data.path)
        }
        },
    },
    computed:{
      editor() {
        return this.$refs.myTextEditor.quillEditor
      }
    },
    mounted() {
        var imgHandler = async function(state) {
            if (state) {
            var fileInput =document.querySelector('#uniqueId input') //隐藏的file元素
            fileInput.click() //触发事件
            }
        }
        this.$refs.myTextEditor.quill.getModule("toolbar").addHandler("image", imgHandler)
    }
```

##### [参考文档1](https://www.awesomes.cn/repo/surmon-china/vue-quill-editor)[参考文档2](https://www.cnblogs.com/zhengweijie/p/7305903.html)