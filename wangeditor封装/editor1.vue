<template>
  <div>
    <div :ref="id" :wordNumber="wordNumber"></div>
    <div class="gl-word-number" v-if="isCount">
      <span>字数统计：</span>
      <span>{{ count }} / {{ wordNumber }}</span>
    </div>
  </div>
</template>
 
<script>
import E from "wangeditor";
 
export default {
  props: {
    id: {
      type: String,
      default: "wangEditorEl"
    },
    isCount: {
      type: Boolean,
      default: true
    },
    wordNumber: {
      type: Number,
      default: 500
    },
    value: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      count: 0,
      editor: {}
    };
  },
  mounted() {
    let editor = new E(this.$refs[this.id]);
    this.editor = editor;
    editor.customConfig = {
      onchange: html => {
        // 监听值得变化
        let text = editor.txt.text();
        if (this.isCount) {
          if (this.wordNumber < text.length) {
            editor.txt.text(text.substring(0, this.wordNumber));
            return;
          }
        }
        this.count = text.length;
        this.$emit("input", html);
      },
      zIndex: 1000,
      showLinkImg: true,
      customUploadImg: (files, insert) => {
        // 文件上传
      },
      uploadImgMaxSize: 5 * 1024 * 1024, // 限制上传图片大小
      uploadImgMaxLength: 1, // 限制上传图片数量
      menus: [
        "head", // 标题
        "bold", // 粗体
        "fontSize", // 字号
        "fontName", // 字体
        "italic", // 斜体
        "underline", // 下划线
        "strikeThrough", // 删除线
        "foreColor", // 文字颜色
        "backColor", // 背景颜色
        "link", // 插入链接
        "list", // 列表
        "justify", // 对齐方式
        // 'quote',  // 引用
        // 'emoticon',  // 表情
        "image", // 插入图片
        "table", // 表格
        // 'video',  // 插入视频
        // 'code',  // 插入代码
        "undo", // 撤销
        "redo" // 重复
      ],
      colors: [
        "#535353",
        "#eeece0",
        "#1c487f",
        "#4d80bf",
        "#c24f4a",
        "#8baa4a",
        "#7b5ba1",
        "#46acc8",
        "#f9963b",
        "#ffffff"
      ]
    };
    editor.create();
    editor.txt.html(this.value);
    this.count = editor.txt.text().length;
  },
  methods: {
    readyHtml() {
      this.editor.txt.html(this.value);
      this.count = this.editor.txt.text().length;
    }
  }
};
</script>
<style lang="less" scoped>
/deep/.w-e-droplist ul.w-e-block li.w-e-item:last-child {
  background: #EBEEF5;
}
</style>