<template>
  <div class="contentRightBasicsSet">
    <p>基础设置</p>
    <el-form
      :disabled="isReadOnly"
      :model="basicsSetData"
      :rules="basicsSetDataRules"
      ref="basicsSetDataRuleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="活动标题" prop="name">
        <el-input
          v-model="basicsSetData.name"
          :disabled="isEdit && conductStatus === '进行中'"
          @blur="cannotEnterSpacesRoules('name', $event)"
        ></el-input>
      </el-form-item>
      <el-form-item label="关键词" prop="keyword">
        <el-input
          v-model="basicsSetData.keyword"
          @blur="cannotEnterSpacesRoules('keyword', $event)"
        ></el-input>
      </el-form-item>
      <el-form-item label="描述" prop="des">
        <el-input
          type="textarea"
          :rows="5"
          placeholder="请输入内容"
          v-model="basicsSetData.des"
          @blur="cannotEnterSpacesRoules('des', $event)"
        >
        </el-input>
      </el-form-item>
      <el-form-item label="缩略图" required :show-message="false">
        <el-upload
          v-if="!isReadOnly"
          :headers="elUploadHeaders"
          :file-list="thumbnailImgList"
          list-type="picture-card"
          :action="uploadUrl"
          :on-success="thumbnailDataHandleAvatarSuccess"
          :before-upload="HandleBeforeUpload"
          auto-upload
        >
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{ file }">
            <img
              class="el-upload-list__item-thumbnail"
              :src="basicsSetData.thumbnail"
              alt=""
            />
            <span class="el-upload-list__item-actions">
              <span
                class="el-upload-list__item-preview"
                @click="thumbnailHandlePictureCardPreview(file)"
              >
                <i class="el-icon-zoom-in"></i>
              </span>
              <span
                class="el-upload-list__item-delete"
                @click="thumbnailHandleRemove(file)"
              >
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
          <div slot="tip" class="el-upload__tip uploadImgTypeAndSizeTitle">
            备注：仅支持jpeg/jpg/png/bmp/gif格式，且文件小于5M
          </div>
        </el-upload>
        <el-dialog :visible.sync="thumbnailDialogVisible">
          <img width="100%" :src="basicsSetData.thumbnail" alt="" />
        </el-dialog>
        <img
          v-if="isReadOnly"
          class="isReadOnlyShowImg"
          :src="basicsSetData.thumbnail"
        />
        <span v-if="!isReadOnly" ref="thumbnailRef" class="spanTips"></span>
      </el-form-item>
      <el-form-item label="背景图" required :show-message="false">
        <el-upload
          v-if="!isReadOnly"
          :disabled="isReadOnly"
          :headers="elUploadHeaders"
          :file-list="basicsSetImgList"
          list-type="picture-card"
          :action="uploadUrl"
          :on-success="basicsSetDataHandleAvatarSuccess"
          :before-upload="HandleBeforeUpload"
          auto-upload
        >
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{ file }">
            <img
              class="el-upload-list__item-thumbnail"
              :src="basicsSetData.backgroundImage"
              alt=""
            />
            <span class="el-upload-list__item-actions">
              <span
                class="el-upload-list__item-preview"
                @click="handlePictureCardPreview(file)"
              >
                <i class="el-icon-zoom-in"></i>
              </span>
              <span
                class="el-upload-list__item-delete"
                @click="handleRemove(file)"
              >
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
          <div slot="tip" class="el-upload__tip uploadImgTypeAndSizeTitle">
            备注：仅支持jpeg/jpg/png/bmp/gif格式，且文件小于5M
          </div>
        </el-upload>
        <el-dialog :visible.sync="basicsSetDialogVisible">
          <img width="100%" :src="basicsSetData.backgroundImage" alt="" />
        </el-dialog>
        <img
          v-if="isReadOnly"
          class="isReadOnlyShowImg"
          :src="basicsSetData.backgroundImage"
        />
        <span v-if="!isReadOnly" ref="isImgRef" class="spanTips"></span>
      </el-form-item>
      <el-form-item label="抽奖时间" prop="valueData">
        <div class="valueDataClass">
          <el-date-picker
            :disabled="isEdit && conductStatus === '进行中'"
            value-format="yyyy-MM-dd HH:mm:ss"
            v-model="basicsSetData.valueData[0]"
            type="datetime"
            placeholder="开始日期"
            default-time="12:00:00"
          >
          </el-date-picker>
          <span>至</span>
          <el-date-picker
            value-format="yyyy-MM-dd HH:mm:ss"
            v-model="basicsSetData.valueData[1]"
            type="datetime"
            placeholder="结束日期"
          >
          </el-date-picker>
        </div>
      </el-form-item>
      <el-form-item label="操作说明" prop="operatingInstructions">
        <el-input
          type="textarea"
          :rows="5"
          placeholder="请输入内容"
          v-model="basicsSetData.operatingInstructions"
          @blur="cannotEnterSpacesRoules('operatingInstructions', $event)"
        >
        </el-input>
      </el-form-item>
      <el-form-item label="初始次数" prop="initialNum">
        <el-input
          :disabled="isEdit && conductStatus === '进行中'"
          @blur="startStepRouler"
          size="small"
          v-model="basicsSetData.initialNum"
          class="elInputWidth"
        ></el-input>
        次
      </el-form-item>
      <el-form-item label="限制方式" prop="limitOption">
        <el-radio-group
          v-model="basicsSetData.limitOption"
          class="el-radio-group-flex"
        >
          <el-radio
            v-for="(val, index) in limitOptionlist"
            :key="val.label"
            :label="val.label"
          >
            {{ val.startText }}
            <el-input
              v-if="val.hasInput"
              :disabled="+basicsSetData.limitOption !== +val.label"
              @input="inputValueSet(val.flag, index, $event)"
              size="small"
              v-model="val.bundleValue"
              class="elInputWidth"
            ></el-input>
            {{ val.endText }}
          </el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="活动规则" required :show-message="false">
        <textarea
          v-if="isReadOnly"
          ref="isEditDisabledTextarea"
          readonly
          name=""
          id=""
          cols="100"
          rows="10"
          disabled
        ></textarea>
        <span ref="isRoulerRef" class="spanTips"></span>
      </el-form-item>
      <el-form-item label="邀请奖励" prop="inviteRewards">
        <el-radio-group
          v-model="basicsSetData.inviteRewards"
          class="el-radio-group-flex"
        >
          <el-radio
            v-for="(val, index) in +userChoiceCountryId === 1
              ? inviteRewardslist
              : inviteRewardslist.slice(0, 2)"
            :key="val.label"
            :label="val.label"
          >
            {{ val.startText }}
            <el-input
              v-if="val.hasInput"
              :disabled="+basicsSetData.inviteRewards !== +val.label"
              @input="inputValueSet(val.flag, index, $event)"
              size="small"
              v-model="val.bundleValue"
              class="elInputWidth"
            ></el-input>
            {{ val.endText }}
          </el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  props: ["isReadOnly", "isEdit"],
  components: {},
  data() {
    return {
      basicsSetData: {
        name: "",
        backgroundImage: "",
        // valueData: ['2000-01-01 11:11:11'],
        valueData: [],
        operatingInstructions: "", // 操作说明
        initialNum: "", // 初始次数
        // 限制方式
        limitOption: "",
        perDayNum: "",
        totalNum: "",
        // 活动规则
        activityRule: "",
        // 邀请奖励
        inviteRewards: "",
        shareAwardNum: "",
        inviteNeverAwardNum: "",
        keyword: "",
        des: "",
        thumbnail: "",
      },
      elUploadHeaders: {
        Authorization: window.localStorage.getItem("user_token"),
        appType: 2,
      },
      basicsSetDialogVisible: false, // 基础设置图片放大对话框
      basicsSetImgList: [],
      thumbnailDialogVisible: false,
      thumbnailImgList: [],
      // 基础配置数据验证
      basicsSetDataRules: {
        name: [
          { required: true, message: "请输入活动标题", trigger: "blur" },
          { max: 20, message: "标题长度最大20个字符", trigger: "blur" },
        ],
        keyword: [{ required: true, message: "请输入关键字", trigger: "blur" }],
        des: [{ required: true, message: "请输入描述", trigger: "blur" }],
        valueData: [
          { required: "date", message: "请选择日期", trigger: "change" },
        ],
        operatingInstructions: [
          { required: true, message: "请填写操作说明", trigger: "blur" },
        ],
        initialNum: [
          { required: true, message: "请填写初始次数", trigger: "blur" },
        ],
        limitOption: [
          { required: true, message: "请选择限制方式", trigger: "change" },
        ],
        inviteRewards: [
          { required: true, message: "请选择邀请奖励方式", trigger: "change" },
        ],
      },
      limitOptionlist: [
        {
          label: "1",
          startText: "不限",
          endText: "",
          hasInput: false,
          flag: "",
          bundleValue: "",
        },
        {
          label: "2",
          startText: "每人每天可参加",
          endText: "次",
          hasInput: true,
          flag: "perDayNum",
          bundleValue: "",
        },
        {
          label: "3",
          startText: "每人总共可参加",
          endText: "次",
          hasInput: true,
          flag: "totalNum",
          bundleValue: "",
        },
      ],
      inviteRewardslist: [
        {
          label: "1",
          startText: "不奖励",
          endText: "",
          hasInput: false,
          flag: "",
          bundleValue: "",
        },
        {
          label: "2",
          startText: "分享成功奖励，每分享一次奖励",
          endText: "次抽奖机会",
          hasInput: true,
          flag: "shareAwardNum",
          bundleValue: "",
        },
        {
          label: "3",
          startText: "邀请新用户奖励，每成功邀请一人奖励",
          endText: "次抽奖机会",
          hasInput: true,
          flag: "inviteNeverAwardNum",
          bundleValue: "",
        },
      ],

      conductStatus: "", // 未开始、进行中可以编辑的不一样1未开始，2进行中，3已结束
    };
  },
  computed: {
    ...mapState({
      userChoiceCountryId: (state) => 0,
      uploadUrl: (state) => '',
    }),
  },
  watch: {
    basicsSetData: {
      handler(val) {
        this.$emit("sendBasicsSetAllData", val);
      },
      immediate: true,
      deep: true,
    },
    // 长度大于20无效
    "basicsSetData.name": function (val) {
      if (val.length >= 20) {
        this.basicsSetData.name = val.substr(0, 20);
      }
    },
    "basicsSetData.backgroundImage": function (val) {
      if (!this.isReadOnly) {
        val
          ? (this.$refs.isImgRef.textContent = "")
          : (this.$refs.isImgRef.textContent = "请上传图片");
      }
    },
    "basicsSetData.activityRule": function (val) {
      if (!this.isReadOnly) {
        val
          ? (this.$refs.isRoulerRef.textContent = "")
          : (this.$refs.isRoulerRef.textContent = "请填写活动规则");
      }
    },
    "basicsSetData.thumbnail": function (val) {
      if (!this.isReadOnly) {
        val
          ? (this.$refs.thumbnailRef.textContent = "")
          : (this.$refs.thumbnailRef.textContent = "请上传缩略图");
      }
    },
    "basicsSetData.limitOption": function (val) {
      if (+val === 2) {
        this.limitOptionlist[2].bundleValue = "";
        this.basicsSetData.totalNum = "";
      } else {
        this.limitOptionlist[1].bundleValue = "";
        this.basicsSetData.perDayNum = "";
      }
    },
    "basicsSetData.inviteRewards": function (val) {
      if (+val === 2) {
        // 外国活动无
        if (+this.userChoiceCountryId === 1) {
          this.inviteRewardslist[2].bundleValue = "";
          this.basicsSetData.inviteNeverAwardNum = "";
        }
      } else {
        this.inviteRewardslist[1].bundleValue = "";
        this.basicsSetData.shareAwardNum = "";
      }
    },
  },
  mounted() {
    this.$emit("sendBasicsSetAllData", this.basicsSetData);
    // 校验
    if (this.$refs.basicsSetDataRuleForm) {
      // this.$store.commit("storageBasicsSetDataRuleRef", [
      //   this.$refs.basicsSetDataRuleForm,
      //   this.$refs.isImgRef,
      //   this.$refs.isRoulerRef,
      //   this.$refs.thumbnailRef,
      // ]);
    }
    // 获取数据展示
    if (this.$route.query.id) {
      // 未开始全部可编辑  进行中部分可编辑 标记
      this.conductStatus = this.$route.params.status;
      delete this.$route.params.status;
      // 根据活动id获取数据
      getAllInfoShow(this.$route.query.id).then((res) => {
        // 编辑时默认languageid
        this.$store.commit("changeUserCurrentClooselanguageId", res.languageId); // 用户当前选择的语言包id(基础配置请求用)
        const copeRes = JSON.parse(JSON.stringify(res));
        copeRes.valueData = [copeRes.startTime, copeRes.endTime];
        // 图片默认展示
        this.basicsSetImgList = [{ url: copeRes.backgroundImage }];
        this.thumbnailImgList = [{ url: copeRes.thumbnail }];
        // 展示的时候清空多余的字符(后端默认返回0, 只有清空)
        if (+copeRes.limitOption === 2) {
          copeRes.totalNum = "";
        } else if (+copeRes.limitOption === 3) {
          copeRes.perDayNum = "";
        } else if (+copeRes.limitOption === 1) {
          copeRes.totalNum = "";
          copeRes.perDayNum = "";
        }
        if (+copeRes.inviteRewards === 2) {
          copeRes.inviteNeverAwardNum = "";
        } else if (+copeRes.inviteRewards === 3) {
          copeRes.shareAwardNum = "";
        } else if (+copeRes.inviteRewards === 1) {
          copeRes.inviteNeverAwardNum = "";
          copeRes.shareAwardNum = "";
        }
        this.limitOptionlist[1].bundleValue = copeRes.perDayNum;
        this.limitOptionlist[2].bundleValue = copeRes.totalNum;
        this.inviteRewardslist[1].bundleValue = copeRes.shareAwardNum;
        if (+this.userChoiceCountryId === 1) {
          this.inviteRewardslist[2].bundleValue = copeRes.inviteNeverAwardNum;
        }
        this.basicsSetData = copeRes;
        // if (this.basicsSetData.activityRule && this.isReadOnly) {
        //   const newDiv = document.createElement('div')
        //   newDiv.innerHTML = this.basicsSetData.activityRule
        //   this.$refs.isEditDisabledTextarea.textContent = newDiv.innerText
        // }
      });
    }
  },
  methods: {
    // 初始次数验证
    startStepRouler(e) {
      const num = e.target.value;
      if (!/^[0-9]\d*$/.test(num)) {
        this.$message.error("请填写合法值");
        this.basicsSetData.initialNum = "";
      }
    },
    // 限制方式宴请奖励输入框校验
    inputValueSet(flag, index, e) {
      if (!/^[1-9]\d*$/.test(+e) || +e < 0 || +e > 999999999) {
        this.$message.error("请填写合法值");
        if (flag === "perDayNum" || flag === "totalNum") {
          this.limitOptionlist[index].bundleValue = "";
        }
        if (flag === "shareAwardNum" || flag === "inviteNeverAwardNum") {
          this.inviteRewardslist[index].bundleValue = "";
        }
        return;
      }
      this.basicsSetData[flag] = e;
    },
    // 缩略图上传成功
    thumbnailDataHandleAvatarSuccess(res, file, fileList) {
      if (+res.code === 1) {
        this.basicsSetData.thumbnail = res.data;
        this.thumbnailImgList &&
          (this.thumbnailImgList = [fileList[fileList.length - 1]]);
      } else {
        this.$message.error(res.msg);
        this.basicsSetData.thumbnail = "";
        this.thumbnailImgList = [];
      }
    },
    // 上传之前
    HandleBeforeUpload(file) {
      // 设置上传的图片的类型
      // bmp,jpg,png,tif，gif
      const isJPG = [
        "image/jpeg",
        "image/jpg",
        "image/png",
        "image/bmp",
        "image/gif",
      ].includes(file.type);
      const isLt2M = file.size / 1024 / 1024 < 5;
      // 限制图片的格式类型
      if (!isJPG) {
        this.$message.error("请上传正确格式的图片!");
      }
      // 限制图片的大小
      if (!isLt2M) {
        this.$message.error("上传图片大小不能超过5MB!");
      }
      return isJPG && isLt2M;
    },
    thumbnailHandlePictureCardPreview() {
      this.thumbnailDialogVisible = true;
    },
    thumbnailHandleRemove() {
      this.thumbnailImgList = [];
      this.basicsSetData.thumbnail = "";
    },
    // 图片上传成功
    basicsSetDataHandleAvatarSuccess(res, file, fileList) {
      if (+res.code === 1) {
        this.basicsSetData.backgroundImage = res.data;
        fileList.length > 0 &&
          (this.basicsSetImgList = [fileList[fileList.length - 1]]);
      } else {
        this.$message.error(res.msg);
        this.basicsSetData.backgroundImage = "";
        this.basicsSetImgLis = [];
      }
    },
    handlePictureCardPreview() {
      this.basicsSetDialogVisible = true;
    },
    handleRemove() {
      this.basicsSetImgList = [];
      this.basicsSetData.backgroundImage = "";
    },
    tinymceeditorValueChange(val) {
      // console.log(val);
      // console.log(/^(\<p\>){1}((&nbsp;)(\s)?)+(\<\/p\>){1}$/.test(val));
      // console.log(/^(<p>){1}((&nbsp;)(\s)?)+(<\/p>){1}$/.test(val));
      if (/^(<p>){1}((&nbsp;)(\s)?)+(<\/p>){1}$/.test(val)) {
        val = "";
      }
      this.basicsSetData.activityRule = val;
      this.$emit("sendBasicsSetAllData", this.basicsSetData);
    },
    // 不可以输入空格
    cannotEnterSpacesRoules(flag, e) {
      if (!e.target.value.trim()) {
        this.basicsSetData[flag] = "";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
// 基础的设置
.contentRightBasicsSet {
  // border: 1px solid red;
  padding: 30px 50px;
  height: 100%;
  overflow: auto;
  box-sizing: border-box;
  p {
    font-weight: bold;
    border-bottom: 1px solid black;
    padding-bottom: 12px;
  }
  .el-form {
    .el-input.elInputWidth {
      width: 100px;
    }
    .el-radio-group {
      margin-top: 15px;
    }
    textarea {
      color: #c3c4d4;
      cursor: not-allowed;
      width: 100%;
    }
  }
}
// 提示
.spanTips {
  font-size: 12px;
  color: #f56c6c;
}
.isReadOnlyShowImg {
  width: 150px;
  height: 150px;
}
.el-radio-group-flex {
  display: flex;
  flex-direction: column;
}
// 抽奖
.valueDataClass {
  display: flex;
  span {
    padding: 0px 15px;
  }
}
.uploadImgTypeAndSizeTitle {
  margin: 0;
  padding: 0;
  padding-top: 11px;
  line-height: 0px;
  font-weight: bold;
}
</style>
