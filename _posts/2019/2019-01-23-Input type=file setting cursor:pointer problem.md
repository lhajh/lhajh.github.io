---
layout: post
title: input 框 type=file 设置 cursor:pointer 的问题
categories: [JS]
description: input 框 type=file 设置 cursor:pointer 的问题
keywords: js
---

为了美化上传文件框, 设置了 `cursor:pointer;`, 然而不起作用, 然后百度找到了解决方法, 设置 `font-size:0`, 这样就可以了。

input 设置 `title=""` 可以去除鼠标悬浮上去后提示 `未选择文件`

附上完整代码:

css

```less
<style lang="less" scoped>
@border-color: #3999ff;
@change-btn: #3999ff;
.file-wrp {
  display: inline;
  position: relative;
  input {
    display: block;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: #ffffff;
    border: none;
    opacity: 0;
    font-size: 0;
    &:focus {
      border: none;
      outline: none;
      background-color: #e1e1e1;
    }
    &:hover {
      cursor: pointer;
    }
    cursor: pointer;
  }
  input[disabled]:hover {
    cursor: not-allowed;
  }
}
</style>
```

html

```html
<template>
  <div class="file-wrp">
    <slot></slot>
    <input
      type="file"
      title=""
      v-if="!disabledInput"
      :disabled="disabledInput"
      :class="{'disabled-input':disabledInput}"
      :multiple="allowMore"
      name="file"
      :accept="accept"
      @change="selectFile"
      ref="input"
    >
  </div>
</template>
```

js

```js
<script>
export default {
  name: 'FileButton',
  props: {
    useRow: {
      type: Object,
      default() {
        return {}
      }
    },
    accept: {
      type: String,
      default: '*'
    },
    disabledInput: {
      type: Boolean,
      default: false
    },
    allowMore: {
      type: Boolean,
      default: false
    }
  },
  computed: {},
  data() {
    return {
      fileValue: '',
      fileList: []
    }
  },
  watch: {},
  components: {},
  methods: {
    selectFile(event) {
      if (this.allowMore) {
        // 处理 IE 触发两次 change 事件
        if (event.target.files.length) {
          for (var i = 0; i < event.target.files.length; i++) {
            this.fileList.push(event.target.files[i])
          }
          if (this.fileList.length > 0) {
            this.$emit('file-result', this.useRow, this.fileList)
          }
        }
      } else {
        this.fileValue = event.target.files[0]
        if (this.fileValue != 'undefined' && this.fileValue != undefined && this.fileValue != '') {
          this.$emit('file-result', this.useRow, this.fileValue)
        }
      }
      event.target.value = null
    }
  },
  created() {},
  mounted() {},
  updated() {
    this.$nextTick(() => {
      this.fileValue = ''
      this.fileList = []
    })
  },
  beforeDestroy() {}
}
</script>

```
