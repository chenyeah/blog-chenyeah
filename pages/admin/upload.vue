<template>
  <div>
    <div class="imgC">
      <div class="up">
        <form action="">
          <label>
            <input type="file" @change="upload" accept="image/gif,image/jpeg,image/png">
            <span>
              <i class="icon iconfont icon-add"></i>
            </span>
          </label>
        </form>
      </div>
      <div class="list" v-for="(n,i) in imgList" :key="i">
        <a :href="`//cloud.chenyeah.com/${n.Key}`"><img :src="`//cloud.chenyeah.com/${n.Key}`" alt=""></a>
        <input type="text" :value="`//cloud.chenyeah.com/${n.Key}`">
      </div>
    </div>
  </div>
</template>
<script>
export default {
    layout: "admin",
    head() {
        return {
            title: "腾讯对象存储上传",
            titleTemplate: "%s - 羽叶丶"
        };
    },
    data() {
        return {
            imgList: []
        };
    },
    methods: {
        getLists() {
            this.$axios.$post("/api/cos/list.php").then(D => {
                this.imgList = D;
            });
        },
        upload() {
            let formdata = new FormData();
            formdata.append("body", event.target.files[0]);
            // formdata.append("action", "test");
            this.$axios({
                url: "/api/cos/upload.php",
                method: "post",
                data: formdata,
                headers: { "Content-Type": "multipart/form-data" }
            }).then(res => {
                if (res.code == 0) {
                    this.getLists();
                }
            });
        }
    },
    mounted() {
        this.getLists();
    }
};
</script>

<style lang="less" scoped>
.imgC {
    overflow: hidden;
}
.up {
    float: left;
    margin: 10px;
    margin-right: 0;
    margin-bottom: 0;
    width: 204px;
    input {
        display: none;
    }
    span {
        display: inline-block;
        width: 100%;
        height: 124px;
        border: 1px solid #ccc;
        text-align: center;
        line-height: 124px;
        cursor: pointer;
        .icon {
            font-size: 60px;
        }
    }
}
.list {
    float: left;
    margin: 10px;
    margin-right: 0;
    margin-bottom: 0;
    width: 204px;
    > a {
        display: block;
        overflow: hidden;
        width: 100%;
        height: 124px;
        img {
            display: block;
            width: 100%;
        }
    }
    input {
        margin-top: 10px;
        width: 100%;
        outline: none;
        border: none;
        background: transparent;
        text-align: center;
    }
}
</style>

