<template>
  <div class="components-container" style="margin: 0;">

    <el-alert
      type="info"
      title="注意"
      description="选择头像提交后将会自动提交修改头像为选择头像"
      class="profile-avatar-tip"
      show-icon
      :closable="false"
    />

    <pan-thumb :image="user.avatar" />

    <el-button type="primary" icon="el-icon-upload" style="position: absolute;bottom: 15px;margin-left: 40px;" @click="imagecropperShow=true">
      选择头像
    </el-button>

    <image-cropper
      v-show="imagecropperShow"
      :key="imagecropperKey"
      :width="300"
      :height="300"
      field="file"
      url="attachment"
      lang-type="zh"
      @close="close"
      @crop-upload-success="cropSuccess"
    />
  </div>
</template>

<script>
import ImageCropper from '@/components/ImageCropper'
import PanThumb from '@/components/PanThumb'
import { updateAvatar } from '@/api/user'

export default {
  name: 'ProfileAvatar',
  components: {
    ImageCropper,
    PanThumb
  },
  props: {
    user: {
      type: Object,
      default: () => {
        return {
          avatar: ''
        }
      }
    }
  },
  data() {
    return {
      imagecropperShow: false,
      imagecropperKey: 0
    }
  },
  methods: {
    cropSuccess(resData) {
      this.imagecropperShow = false
      this.imagecropperKey = this.imagecropperKey + 1
      this.user.avatar = resData.url

      updateAvatar({
        avatar: resData.id
      }).then(response => {
        this.$message({
          message: '更新头像成功',
          type: 'success',
          duration: 5 * 1000
        })
      })
    },
    close() {
      this.imagecropperShow = false
    }
  }
}
</script>

<style scoped>
.profile-avatar-tip {
  margin-bottom: 20px;
}
.avatar{
  width: 200px;
  height: 200px;
  border-radius: 50%;
}
</style>

