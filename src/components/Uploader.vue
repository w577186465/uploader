<template>
  <transition name="fade">
    <div class="uploader">
      <transition name="fade">
        <div class="bg" @click="close" v-if="state"></div>
      </transition>
      <transition name="enlarge">
        <div class="warp" v-if="state">
          <button class="close" @click="close">
            <svg class="icon" aria-hidden="true">
              <use xlink:href="#icon-close"></use>
            </svg>
          </button>

          <div class="files">
            <ul>
              <li v-for="v of images">
                <img :src="v">
              </li>
            </ul>
          </div>

          <div class="nofile" v-if="images">
            <button @click="clickupload">add files</button>
            <input type="file" style="display:none;" @change="upload" ref="upload" multiple/>
          </div>
        </div>
      </transition>
      
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Uploader',
  props: {
    open: Boolean
  },
  data () {
    return {
      images: []
    }
  },
  computed: {
    state: function () {
      return this.open
    }
  },
  methods: {
    close: function () {
      this.$emit('update:open', false)
    },
    clickupload: function () {
      this.$refs.upload.click()
    },
    upload: function (e) {
      var files = e.target.files || e.dataTransfer.files
      if (!files.length) return
      if (typeof FileReader === 'undefined') {
        alert('您的浏览器不支持图片上传，请升级您的浏览器')
        return false
      }
      // var image = new Image()
      var vm = this
      var leng = files.length
      for (var i = 0; i < leng; i++) {
        var reader = new FileReader()
        reader.readAsDataURL(files[i])
        reader.onload = function (e) {
          vm.images.push(e.target.result)
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  .uploader {
    .bg {
      background: #000;
      opacity: 0.1;
      top: 0;
      right:0;
      bottom: 0;
      left: 0;
      position: fixed;
    }

    .warp {
      top: 10%;
      right: 10%;
      bottom: 10%;
      left:10%;

      background: #fff;
      position: fixed;
      box-shadow: 0 0 10px #c1c1c1;
      border-radius: 5px;
    }

    .close {
      float: right;
      background: none;
      border: 0;
      font-size: 16px;
      color: #555;
      cursor: pointer;
      outline: none;
      margin: 10px;
      padding: 0;
    }
  }

  // fade transition
  .fade-enter-active, .fade-leave-active {
    transition: opacity .4s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }

  // enlarge transition
  .enlarge-enter-active, .enlarge-leave-active {
    transition: all .5s cubic-bezier(0.7, -0.3, 0.3, 1.3)
  }
  .enlarge-enter, .enlarge-leave-to {
    opacity: 0;
    transform: scale(.3);
    -ms-transform: scale(.3);
    -moz-transform: scale(.3);
    -webkit-transform: scale(.3);
    -o-transform: scale(.3);
  }
</style>
