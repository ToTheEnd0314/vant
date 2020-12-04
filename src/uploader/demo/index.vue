<template>
  <demo-section>
    <demo-block :title="t('basicUsage')">
      <van-uploader :after-read="afterRead" />
    </demo-block>

    <demo-block :title="t('preview')">
      <van-uploader v-model="fileList" multiple accept="*" />
    </demo-block>

    <demo-block :title="t('disabled')">
      <van-uploader :after-read="afterRead" disabled />
    </demo-block>

    <demo-block v-if="!isWeapp" :title="t('status')">
      <van-uploader v-model="statusFileList" :after-read="afterReadFailed" />
    </demo-block>

    <demo-block :title="t('maxCount')">
      <van-uploader v-model="fileList2" multiple :max-count="2" />
    </demo-block>

    <demo-block :title="t('maxSize')">
      <van-uploader
        v-model="fileList4"
        multiple
        :max-count="5"
        :max-size="3 * 1024 * 1024"
        @oversize="onOversize"
      />
    </demo-block>

    <demo-block :title="t('uploadStyle')">
      <van-uploader>
        <van-button type="primary" icon="photo">
          {{ t('upload') }}
        </van-button>
      </van-uploader>
    </demo-block>

    <demo-block :title="t('beforeRead')">
      <van-uploader v-model="fileList3" :before-read="beforeRead" />
    </demo-block>
  </demo-section>
</template>

<script>
export default {
  i18n: {
    'zh-CN': {
      repeat: '重复选择',
      status: '上传状态',
      failed: '上传失败',
      upload: '上传文件',
      preview: '文件预览',
      disabled: '禁用',
      maxCount: '限制上传数量',
      uploading: '上传中...',
      beforeRead: '上传前校验',
      uploadStyle: '自定义上传样式',
      invalidType: '请上传 jpg 格式图片',
      maxSize: '限制上传大小(3M)',
    },
    'en-US': {
      repeat: 'Single Repeat',
      status: 'Upload Status',
      failed: 'Failed',
      upload: 'Upload File',
      preview: 'Preview File',
      disabled: 'Disabled',
      maxCount: 'Max Count',
      uploading: 'Uploading...',
      beforeRead: 'Before Read',
      uploadStyle: 'Upload Style',
      invalidType: 'Please upload an image in jpg format',
      maxSize: 'Max Size(3M)',
    },
  },

  data() {
    return {
      repeatFileList: [
        {
          url:
            'https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/241f95cad1c8a786262fb6e86509c93d71cf50cb.jpg',
        },
      ],

      fileList: [
        { url: 'https://img.yzcdn.cn/vant/leaf.jpg' },
        { url: 'https://img.yzcdn.cn/vant/tree.jpg' },
      ],
      fileList2: [{ url: 'https://img.yzcdn.cn/vant/sand.jpg' }],
      fileList3: [],
      fileList4: [],
      statusFileList: [],
    };
  },

  created() {
    this.statusFileList.push(
      {
        url: 'https://img.yzcdn.cn/vant/leaf.jpg',
        status: 'uploading',
        message: this.t('uploading'),
      },
      {
        url: 'https://img.yzcdn.cn/vant/tree.jpg',
        status: 'failed',
        message: this.t('failed'),
      }
    );
  },

  methods: {
    beforeRead(file) {
      if (file.type !== 'image/jpeg') {
        this.$toast(this.t('invalidType'));
        return false;
      }

      return true;
    },

    afterRead(file, detail) {
      console.log(file, detail);
    },

    afterReadFailed(item) {
      item.status = 'uploading';
      item.message = this.t('uploading');

      setTimeout(() => {
        item.status = 'failed';
        item.message = this.t('failed');
      }, 1000);
    },

    onOversize(file, detail) {
      console.log(file, detail);
    },
  },
};
</script>

<style lang="less">
@import '../../style/var';

.demo-uploader {
  background-color: @white;

  .van-uploader {
    margin-left: @padding-md;
  }
}
</style>
