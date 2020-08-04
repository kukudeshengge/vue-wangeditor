<template>
  <div ref="experienceComment"></div>
</template>
<script>
import E from "wangeditor";
 
export default {
  props: {
    experienceComment: {
      type: String,
      default: ""
    },
    experienceCommentHtml: {
      type: String,
      default: ""
    },
    maxlength: {
      type: Number,
      default: 1000
    }
  },
  data() {
    return {
      editor: null
    };
  },
  mounted() {
    this.editor = new E(this.$refs["experienceComment"]);
    this.editor.customConfig = {
      zIndex: 1000,
      menus: [
        "bold", // 粗体
        "fontSize", // 字号
        "fontName", // 字体
        "italic", // 斜体
        "underline", // 下划线
        "strikeThrough", // 删除线
        "foreColor", // 文字颜色
        "backColor" // 背景
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
      ],
      onchange: html => {
        // 监听值得变化
        let text = this.editor.txt.text();
        if (this.maxlength < text.length) {
          this.editor.txt.text(text.substring(0, this.maxlength));
          return;
        }
        this.$emit("update:experienceCommentHtml", html);
        this.$emit("update:experienceComment", text);
      }
    };
    this.editor.create();
    this.editor.txt.html(
      this.experienceCommentHtml
        ? this.experienceCommentHtml
        : `<p>${this.experienceComment}</p>`
    );
    this.$refs["experienceComment"].addEventListener("input", this.inputEvent)
  },
  methods: {
    inputEvent() {
      let html = this.editor.txt.html();
      if (html === '' || html === '<p><br></p>') {
        this.editor.customConfig.onchange()
      }
    }
  },
  beforeDestroy() {
    this.$refs["experienceComment"].removeEventListener("input", this.inputEvent)
  },
  watch: {
    experienceCommentHtml: function(value) {
      this.editor.txt.html(value);
    }
  }
};
</script>
<style lang="less" scoped>
/deep/.w-e-droplist ul.w-e-block li.w-e-item:last-child {
  background: #ebeef5;
}
/deep/ .w-e-text-container {
  height: 200px !important;
}
/deep/ .w-e-text {
  text-align: left;
  p {
    margin: 0;
  }
}
</style>