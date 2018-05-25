<template>
    <div>
        <quill-editor ref="myTextEditor" v-model="content" :config="editorOption" @blur="onEditorBlur($event)" @focus="onEditorFocus($event)" @ready="onEditorReady($event)" @change="onEditorChange($event)">
        </quill-editor>

        <!-- If you need to manually control the data synchronization, you can monitor the code change event like this（如果你需要手动控制数据流，就需要像这样手动监听changed事件） -->
       
    </div>
</template>

<script>
    import { quillEditor } from 'vue-quill-editor'
    export default {
      data () {
        return {
          content: '<h2>I am Example</h2>',
          editorOption: {
            modules: {
//            toolbar: [
//              ['bold', 'underline']
//            ]
            }
          }
        }
      },
        // 如果需要手动控制数据同步，父组件需要显式地处理changed事件
      methods: {
        onEditorBlur (editor) {
          // console.log('editor blur!', editor)失去焦点
        },
        onEditorFocus (editor) {
          // console.log('editor focus!', editor)获得焦点
        },
        onEditorReady (editor) {
          // console.log('editor ready!', editor)
        },
        onEditorChange ({
          editor,
          html,
          text
        }) {
          // console.log('editor change!', editor, html, text)
          this.content = html
          this.$emit('richTextContent', html)
          // console.log('this is the content')
          // console.log(html)
        }
      },
        // 如果你需要得到当前的editor对象来做一些事情，你可以像下面这样定义一个方法属性来获取当前的editor对象，实际上这里的$refs对应的是当前组件内所有关联了ref属性的组件元素对象
      computed: {
        editor () {
          return this.$refs.myTextEditor.quillEditor
        }
      },
      mounted () {
        // you can use current editor object to do something(editor methods)
        // console.log('this is my editor', this.editor)
        // this.editor to do something...
      },
      components: {
        quillEditor
      }
    }
</script>

<style>
  .quill-editor{
  	height: 500px;
  }
</style>