<!-- 用来做大纲树的，但是好像没什么卵用 -->


<template>
  <div class="baseDiv">
    <div class="editor" ref="editor" :style="styles"></div>
    <div class="rightMenu" v-if="rightMenuShow">
      <div class="lake-sidebar-title">
        <span>大纲</span>
        <button
          @click="sidebarClick"
          style="line-height: 0; top: 0px"
          type="button"
          aria-label="Close"
          class="el-dialog__headerbtn"
        >
          <i class="el-dialog__close el-icon el-icon-close"></i>
        </button>
      </div>
      <div
        class="lake-sidebar-content lake-scrollable scroll-y"
        style="position: relative"
      >
        <div class="lake-synopsis">
          <div class="larkui-synopsis-wrap">
            <ul class="larkui-synopsis">
              <li
                class="larkui-synopsis-item larkui-synopsis-item-active"
                v-for="(item, i) in outlineList"
              >
                <span
                  class="larkui-synopsis-item-link larkui-synopsis-item-link-1"
                  :class="
                    item.tagName == 'H3'
                      ? 'h3Padding'
                      : item.tagName == 'H4'
                      ? 'h4Padding'
                      : ''
                  "
                >
                  <a :href="item.id" :title="item.title">
                    <span>{{ item.title }}</span>
                  </a>
                </span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// 富文本组件
import Quill from "quill";
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";

export default {
  name: "Editors",
  props: {
    /* 编辑器的内容 */
    value: {
      type: String,
      default: "",
    },
    /* 高度 */
    height: {
      type: Number,
      default: null,
    },
    outline: {
      type: String,
      default: ""
    }
  },
  data() {
    const that = this;
    return {
      readOnly: false,
      rightMenuShow: true,
      Quill: null,
      currentValue: "",
      options: {
        theme: "snow",
        bounds: document.body,
        debug: "warn",
        modules: {
          // 工具栏配置
          toolbar: {
            container: [
              ["bold", "italic", "underline", "strike"], // 加粗 斜体 下划线 删除线
              ["blockquote", "code-block"], // 引用  代码块
              [{ list: "ordered" }, { list: "bullet" }], // 有序、无序列表
              [{ indent: "-1" }, { indent: "+1" }], // 缩进
              [{ size: [false, "small", "large", "huge"] }], // 字体大小
              [{ header: [false, 1, 2, 3, 4] }], // 标题
              [{ color: [] }, { background: [] }], // 字体颜色、字体背景颜色
              [{ align: [] }], // 对齐方式
              ["clean"], // 清除文本格式
              ["link", "image", "video"], // 链接、图片、视频
              ["sidebar"],
            ],
            handlers: {
              shadeBox: null,
              sidebar: function () {
                that.rightMenuShow = !that.rightMenuShow;
              },
            },
          },
        },
        placeholder: "            请输入标题",
      },
      outlineList: [],
    };
  },
  computed: {
    styles() {
      let style = {};
      if (this.height) {
        style.height = `${this.height}px`;
      }
      style.width = "50%";
      style.margin = "0 0 0 25%";
      style.height = "calc(100% - 42px)";
      return style;
    },
  },
  watch: {
    value: {
      handler(val) {
        if (val !== this.currentValue) {
          this.currentValue = val === null ? "" : val;
          if (this.Quill) {
            this.Quill.pasteHTML(this.currentValue);
            console.log(this.currentValue);
          }
        }
      },
      immediate: true,
    },
  },
  mounted() {
    this.init();
  },
  beforeDestroy() {
    this.Quill = null;
  },
  methods: {
    init() {
      this.outlineList = JSON.parse(this.outline);
      const editor = this.$refs.editor;
      this.Quill = new Quill(editor, this.options);
      this.Quill.pasteHTML(this.currentValue);
      this.parsingHtml(this.$refs.editor.children[0].innerHTML);
      this.Quill.on("text-change", (delta, oldDelta, source) => {
        const html = this.$refs.editor.children[0].innerHTML;
        const text = this.Quill.getText();
        const quill = this.Quill;
        this.currentValue = html;
        this.$emit("input", html);
        this.$emit("on-change", { html, text, quill});
        this.parsingHtml(html);
      });
      this.Quill.on("text-change", (delta, oldDelta, source) => {
        this.$emit("on-text-change", delta, oldDelta, source);
      });
      this.Quill.on("selection-change", (range, oldRange, source) => {
        this.$emit("on-selection-change", range, oldRange, source);
      });
      this.Quill.on("editor-change", (eventName, ...args) => {
        this.$emit("on-editor-change", eventName, ...args);
      });

      const sourceEditorButton = document.querySelector(".ql-sidebar");
      sourceEditorButton.icon = 'el-icon-s-unfold';
      sourceEditorButton.style.cssText =
        "width:40px; border:1px solid #ccc; border-radius:5px;";
      sourceEditorButton.innerText = "大纲";
    },
    sidebarClick() {
      this.rightMenuShow = !this.rightMenuShow;
    },
    parsingHtml(html) {
      const el = document.createElement("div");
      const that = this;
      el.innerHTML = html;
      let hList = [];
      that.outlineList = [];
      let index = 0;
      let bol = true;
      // 遍历所有html标签，寻找h标签作为大纲
      el.querySelectorAll("*").forEach((item) => {
        if (
          item.tagName == "H2" ||
          item.tagName == "H3" ||
          item.tagName == "H4"
        ) {
          index ++;
          that.outlineList.push({
            title: item.innerText,
            tagName: item.tagName,
            id: '#' + item.tagName + index
          });
        }
      });
      that.$emit("getOutlineList", that.outlineList);
      // 为富文本中的内容增加id属性，锚点使用
      index = 0;
      document.getElementsByClassName('ql-editor')[0].childNodes.forEach((item) => {
        if (item.innerText && bol) {
          this.$emit("title", item.innerText);
          bol = false;
        }
        if (
          item.tagName == "H2" ||
          item.tagName == "H3" ||
          item.tagName == "H4"
        ) {
          index++;
          item.id = item.tagName + index;
        }
      })
    },
  },
};
</script>
<style type="scss">
html,body{height:100%;overflow: hidden;}
#app,.baseDiv{height: 100%;}
</style>
<style>
.editor,
.ql-toolbar {
  white-space: pre-wrap !important;
  line-height: normal !important;
}
.quill-img {
  display: none;
}
.ql-snow .ql-tooltip[data-mode="link"]::before {
  content: "请输入链接地址:";
}
.ql-snow .ql-tooltip.ql-editing a.ql-action::after {
  border-right: 0px;
  content: "保存";
  padding-right: 0px;
}

.ql-snow .ql-tooltip[data-mode="video"]::before {
  content: "请输入视频地址:";
}

.ql-snow .ql-picker.ql-size .ql-picker-label::before,
.ql-snow .ql-picker.ql-size .ql-picker-item::before {
  content: "14px";
}
.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="small"]::before,
.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="small"]::before {
  content: "10px";
}
.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="large"]::before,
.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="large"]::before {
  content: "18px";
}
.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="huge"]::before,
.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="huge"]::before {
  content: "32px";
}

.ql-snow .ql-picker.ql-header .ql-picker-label::before,
.ql-snow .ql-picker.ql-header .ql-picker-item::before {
  content: "正文";
}
.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="1"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="1"]::before {
  content: "标题1";
}
.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="2"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="2"]::before {
  content: "标题2";
}
.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="3"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="3"]::before {
  content: "标题3";
}
.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="4"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="4"]::before {
  content: "标题4";
}
/* .ql-snow .ql-picker.ql-header .ql-picker-label[data-value="5"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="5"]::before {
  content: "标题5";
} */
/* .ql-snow .ql-picker.ql-header .ql-picker-label[data-value="6"]::before,
.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="6"]::before {
  content: "标题6";
} */

.ql-snow .ql-picker.ql-font .ql-picker-label::before,
.ql-snow .ql-picker.ql-font .ql-picker-item::before {
  content: "标准字体";
}
.ql-snow .ql-picker.ql-font .ql-picker-label[data-value="serif"]::before,
.ql-snow .ql-picker.ql-font .ql-picker-item[data-value="serif"]::before {
  content: "衬线字体";
}
.ql-snow .ql-picker.ql-font .ql-picker-label[data-value="monospace"]::before,
.ql-snow .ql-picker.ql-font .ql-picker-item[data-value="monospace"]::before {
  content: "等宽字体";
}

.editor {
  float: left;
  overflow-y: hidden;
}
.rightMenu {
  float: left;
  width: 22%;
  margin: 20px 0 0 20px;
}

.lake-sidebar-title {
  position: relative;
  margin-bottom: 10px;
  font-size: 14px;
  font-weight: bold;
  padding: 0 2px 10px;
  border-bottom: 1px solid #e8e8e8;
}

.lake-toc-sidebar .lake-sidebar-content {
  overflow: auto;
  height: calc(100vh - 164px);
  margin-left: -60px;
  padding: 0 2px;
  box-sizing: border-box;
}

.larkui-synopsis-wrap {
  overflow: hidden;
}
.larkui-synopsis {
  width: 100%;
  overflow-y: auto;
  overflow-x: hidden;
  font-size: 12px;
  list-style: none;
  padding: 0;
  margin: 0;
  display: block;
}
.larkui-synopsis .larkui-synopsis-item {
  line-height: 24px;
  color: #595959;
  display: list-item;
  text-align: -webkit-match-parent;
}

.larkui-synopsis .larkui-synopsis-item-link-1 {
  padding-left: 0;
}

.larkui-synopsis .larkui-synopsis-item-link {
  margin: 0 10px 0 0;
  width: calc(100% - 28px);
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

li {
  text-align: -webkit-match-parent;
}

.larkui-synopsis .larkui-synopsis-item-active a:link {
  color: black;
  text-decoration:none;
}

.larkui-synopsis .larkui-synopsis-item-active a:visited {
  color: black;
  text-decoration:none;
}

.larkui-synopsis .larkui-synopsis-item-active a:active {
  color: #25b864;
  text-decoration:none;
}

.ql-editor .ql-indent-1:not(.ql-direction-rtl) {
  padding-left: 0px !important;
}

.title {
  font-size: 36px !important;
  color: #262626;
}

.h4Padding {
  padding-left: 2.4em !important;
}
.h3Padding {
  padding-left: 1.2em !important;
}
.ql-toolbar.ql-snow {
  width: 100%;
  background-color: #FCFCFC !important;
  z-index: 999999;
  text-align: center;
}
body {
  margin: 0;
}

.baseDiv {
  background-color: #F9F9F9;
}

.ql-editor {
  padding: 32px 60px 0 60px;
  background-color: white;
  min-height: 700px;
  overflow-y: auto;
}
/** 滚动条样式重写 */
.ql-editor::-webkit-scrollbar {
    width: 5px;
    height: 1px;
  }
.ql-editor::-webkit-scrollbar-thumb {
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
  background:#7D7D7D;
}
.ql-editor::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
  border-radius: 10px;
  background: #EDEDED;
}

</style>