<template>
  <vue-scroll class="page-profile" id="affix-container">
    <div class="card-base card-shadow--medium identity" id="boundary">
      <div class="cover"></div>
      <div class="username">
        <div class="cover-small"></div>
        <div class="avatar-small">
          <img src="@/assets/images/avatar.jpg" alt="avatar">
        </div>
        <span>Dangelababy 的小天地</span>
      </div>
      <img
        src="@/assets/images/cover-2.jpg"
        id="color-thief"
        class="color-thief"
        alt="profile cover"
      >
    </div>
    <el-row :gutter="12">
      <el-col :span="24">
        <el-table
          ref="memberTable"
          :data="tableData"
          style="width: 100%">
          <el-table-column
            prop="date"
            label="Date"
            sortable
            width="180"
            column-key="date"
            :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
            :filter-method="filterHandler"
          >
          </el-table-column>
          <el-table-column
            prop="name"
            label="Name"
            width="180">
          </el-table-column>
          <el-table-column
            prop="address"
            label="Address"
            :formatter="formatter">
          </el-table-column>
          <el-table-column
            prop="tag"
            label="Tag"
            width="100"
            :filters="[{ text: 'Home', value: 'Home' }, { text: 'Office', value: 'Office' }]"
            :filter-method="filterTag"
            filter-placement="bottom-end">
            <template slot-scope="scope">
              <el-tag
                :type="scope.row.tag === 'Home' ? 'primary' : 'success'"
                disable-transitions>{{scope.row.tag}}</el-tag>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
  </vue-scroll>
</template>

<script>
import { diaCodeList } from "@/utils/diaCode";
import firebase from "firebase";
import UserService from "@/services/UserService";
import _ from "lodash";

export default {
  name: "Profile",
  data() {
    return {
      tableData: [{
        date: '2016-05-03',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
        tag: 'Home'
      }, {
        date: '2016-05-02',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
        tag: 'Office'
      }, {
        date: '2016-05-04',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
        tag: 'Home'
      }, {
        date: '2016-05-01',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
        tag: 'Office'
      }],
    };
  },
  methods: {
    resetDateFilter() {
      this.$refs.filterTable.clearFilter('date');
    },
    clearFilter() {
      this.$refs.filterTable.clearFilter();
    },
    formatter(row, column) {
      return row.address;
    },
    filterTag(value, row) {
      return row.tag === value;
    },
    filterHandler(value, row, column) {
      const property = column['property'];
      return row[property] === value;
    }
  },
  mounted() {
    //
  },
  beforeDestroy() {
    // window.removeEventListener('resize', this.resizeAffixEnabled);
  },
  components: {
    //
  }
};
</script>

<style lang="scss" scoped>
@import "../../../assets/scss/_variables";

.card-form {
  padding: 20px;
}
.uploadedImg {
  margin-left: 10px;
  width: 50%;
}
.el-button {
  float: left;
}

.page-profile {
  overflow: auto;

  .identity {
    margin-bottom: 20px;
    position: relative;
    width: 100%;
    height: 220px;

    .cover {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-image: url("../../../assets/images/cover-2.jpg");
      background-position: 50%;
      background-size: cover;
      background-repeat: no-repeat;
      width: 100%;
      height: 100%;
    }
    .username {
      color: #32325d;
      position: absolute;
      width: 100%;
      height: 50px;
      bottom: 75px;
      left: 0;
      right: 0;
      background: #fff;
      line-height: 50px;
      box-sizing: border-box;
      padding-left: 250px;
      font-size: 25px;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      box-shadow: 0 7px 14px 0 rgba(50, 50, 93, 0.1),
        0 3px 6px 0 rgba(0, 0, 0, 0.07);

      .cover-small {
        width: 220px;
        height: 50px;
        overflow: hidden;
        display: block;
        float: left;
        margin-right: -220px;
        position: relative;
        left: -250px;
        border-radius: 9px;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        border: 4px solid white;
        opacity: 0;
        top: 0px;
        background-image: url("../../../assets/images/cover-2.jpg");
        background-position: 50%;
        background-size: cover;
        background-repeat: no-repeat;
        -webkit-transition: all 0.5s;
        transition: all 0.5s;
      }
      .avatar-small {
        width: 50px;
        height: 50px;
        overflow: hidden;
        display: block;
        float: left;
        margin-right: -50px;
        position: relative;
        left: -100px;
        border-radius: 50%;
        box-sizing: border-box;
        border: 4px solid white;
        opacity: 0;
        top: 0px;
        transform: rotate(-50deg);
        transition: all 0.5s;
      }
      .avatar-small img {
        width: 100%;
      }

      &.affix {
        z-index: 99;
        border-radius: 5px;

        .cover-small {
          opacity: 1;
        }

        .avatar-small {
          opacity: 1;
          left: -60px;
          transform: rotate(0deg);
        }
      }

      .colors-box {
        height: 50px;
        background: white; //091c2d
        float: right;

        .color {
          width: 50px;
          height: 50px;
          background: white; //091c2d
          float: right;
          transform: skew(-45deg);
          box-shadow: 1px 0px 1px 0px transparent;
          position: relative;
          right: -25px;
          margin-right: -50px;
          transition: margin-right 0.75s;

          &.active {
            margin-right: 0;
          }

          &:nth-child(2) {
            opacity: 0.8;
          }
          &:nth-child(3) {
            opacity: 0.6;
          }
          &:nth-child(4) {
            opacity: 0.4;
          }
          &:nth-child(5) {
            opacity: 0.2;
          }
        }
      }
    }
    .avatar {
      border: 6px solid #fff;
      position: absolute;
      bottom: 10px;
      left: 50px;
      width: 180px;
      height: 180px;
      overflow: hidden;
      border-radius: 50%;
      box-sizing: border-box;
      box-shadow: 0px 20px 15px -15px rgba(0, 0, 0, 0.15);

      img {
        width: 100%;
      }
    }

    .color-thief {
      display: block;
      width: 100px;
      visibility: hidden;
      z-index: -999999;
      position: absolute;
    }
  }

  .info {
    padding: 24px 32px;
  }
}

@media (max-width: 768px) {
  .page-profile {
    .identity {
      height: auto;

      .avatar {
        bottom: inherit;
        top: 10px;
        left: 50%;
        width: 100px;
        margin-left: -50px;
        height: 100px;
        border-width: 3px;
      }

      .username {
        position: inherit;
        padding: 10px;
        margin: 0;
        top: inherit;
        left: inherit;
        z-index: 1;
        right: inherit;
        text-align: center;
        bottom: inherit;
        white-space: inherit;
        line-height: inherit;
        height: auto;
        margin-top: 120px;
        width: 90%;
        margin-left: 5%;
        margin-bottom: 10px;
        border-radius: 50px;

        .colors-box {
          display: none;
        }

        .avatar-small {
          display: none;
        }

        .cover-small {
          display: none;
        }
      }
    }

    .info {
      padding: 8px 16px;
    }
  }
}
</style>

<style lang="scss">
.page-profile {
  .el-tabs:not(.el-tabs--border-card) {
    .el-tabs__item:not(.is-active) {
      color: #32325d;
    }
  }
}
</style>
