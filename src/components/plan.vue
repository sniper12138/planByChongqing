<template>
  <div class="plan">
    <div class="title"><span>五</span> 四人跨年重庆之旅</div>
    <div class="cnt" :class="cur == 3 ? 'commentView' : ''">
      <div v-show="cur == 0">
        <h1>我们粗发啦~~！</h1>
        <p>
          预计31号晚上19：15到机场，出机场后搭地铁到较口场站（民宿在洪崖洞是小什字，在解放碑是较场口），车程40分-1小时，然后去找民宿歇一会~
        </p>
        <p>八点半-九点半后吃吃喝喝准备跨年。</p>
        <p>
          跨年首选
          <span>洪崖洞 => 八一好吃街</span> 附近，随意走走，哪好玩逛哪。
        </p>
        <p></p>
        <h1>DAY 1</h1>
        <p>早上吃完早饭出发</p>
        <p>
          <span
            >解放碑 => 长江索道（20单程/30往返，琪姐强烈要求要去的） =>
            朝天门码头 => 来福士广场（一路拍拍拍~打卡打卡打卡~~）</span
          >
        </p>
        <p></p>
        <p></p>
        <h1>行李：</h1>
        <p>身份证，雨伞，休闲鞋（别穿高跟鞋），肠胃药，拍照设备。</p>
        <p></p>
        <h1>干饭建议：</h1>
        <p>大众点评上选，建议：<span>重庆火锅，一只酸奶牛</span></p>
      </div>
      <div v-show="cur == 1">
        <h1>DAY 2</h1>
        <p>
          <span
            >鹅岭二厂 => 李子坝（穿楼城轨） => 白象居（少年的你取景地） =>
            磁器口（有卡通人物找你拍照要拒绝，要钱的） =>
            （不确定）两江游（5元怀旧轮渡，虽然不知道在哪）</span
          >
        </p>
        <p></p>
        <p></p>
        <h1>干饭建议：</h1>
        <p>大众点评上选，建议：<span>重庆小面，各种小吃</span></p>
      </div>
      <div v-show="cur == 2">
        <h1>DAY 3 （游玩建议）</h1>
        <p>
          <span
            >马房湾七彩巷 => 弹子石老街 => 交通茶馆 =>
            （不知道了，到时再来吧看见哪去哪）</span
          >
        </p>
        <p></p>
        <p></p>
        <h1>干饭建议：</h1>
        <p>大众点评。。。 建议：<span>江湖菜，小酒馆</span></p>
      </div>
      <div v-show="cur == 3" class="view">
        <div class="commentList">
          <div class="li" v-for="(item, index) in commentData" :key="index">
            {{ item.comment }}
          </div>
        </div>
        <div class="input">
          <input
            type="text"
            maxlength="30"
            placeholder="输入个评论"
            v-model="commentVal"
            @keyup.enter="postComment()"
          />
          <div class="send">
            <button @click="postComment()">发送</button>
          </div>
        </div>
      </div>
    </div>
    <div class="nav">
      <div class="li" :class="cur == 0 ? 'act' : ''" @click="changeCur(0)">
        DAY 1
      </div>
      <div class="li" :class="cur == 1 ? 'act' : ''" @click="changeCur(1)">
        DAY 2
      </div>
      <div class="li" :class="cur == 2 ? 'act' : ''" @click="changeCur(2)">
        DAY 3
      </div>
      <div class="li" :class="cur == 3 ? 'act' : ''" @click="changeCur(3)">
        评个论
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "plan",
  data() {
    return {
      cur: 0,
      commentData: [],
      commentVal: "",
    };
  },
  mounted() {
    this.initComment();
  },
  methods: {
    changeCur(i) {
      this.cur = i;
    },
    initComment() {
      axios({
        url: "http://lu.impermanence.cn:5555/plan/getcomment",
        method: "get",
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.commentData = res.data?.commentDatas || [];
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    postComment() {
      if (this.commentVal.trim() == "") {
        alert("请输入评论");
        return false;
      }

      axios({
        url: "http://lu.impermanence.cn:5555/plan/postComment",
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        data: {
          comment: this.commentVal,
        },
      })
        .then((res) => {
          console.log(res);
          if (res.data.code == 200) {
            alert("评论成功");
            this.commentVal = "";
            this.initComment();
          } else {
            alert(res.data.msg);
          }
        })
        .catch((err) => {
          console.log(err);
          alert("网络错误");
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.plan {
  width: 100vw;
  height: 100vh;
  background: #000;
  color: #fff;
  display: flex;
  flex-direction: column;
  .title {
    text-align: center;
    padding: 40px 0;
    font-size: 120px;
    font-weight: bold;
    color: #486187;
    span {
      position: relative;
      &::before {
        content: "";
        position: absolute;
        width: 0.22667rem;
        height: 2.03333rem;
        background: #486187;
        transform: rotate(-45deg);
        top: 0.2rem;
        left: 0.6rem;
      }
    }
  }
  .cnt {
    flex: 1;
    padding: 40px 60px;
    overflow: auto;
    p {
      padding: 20px 0;
      font-size: 70px;
    }
    h1 {
      padding: 20px 0;
      font-size: 80px;
    }
    span {
      color: #ffff00;
    }
    &.commentView {
      display: flex;
      overflow: inherit;
      flex-direction: column;
      .view {
        flex: 1;
        overflow: auto;
        display: flex;
        flex-direction: column;
        .commentList {
          flex: 1;
          overflow: auto;
          .li {
            padding: 40px 0;
            + .li{
              border-top: 1px solid #fff;
            }
          }
        }
        .input {
          display: flex;
          input {
            display: block;
            width: 80%;
            line-height: 1.5;
            padding: 20px;
            font-size: 14px;
            border: 1px solid #dcdee2;
            border-radius: 20px;
            color: #515a6e;
            position: relative;
            cursor: text;
            outline: none;
            border: none;
          }
          .send {
            width: 20%;
            padding: 0 10px;
            display: flex;
            justify-content: flex-end;
            button {
              color: #fff;
              background-color: #2d8cf0;
              border-color: #2d8cf0;
              margin-bottom: 0;
              font-weight: 400;
              font-size: 48px;
              text-align: center;
              vertical-align: middle;
              -ms-touch-action: manipulation;
              touch-action: manipulation;
              cursor: pointer;
              background-image: none;
              outline: none;
              border: 1px solid transparent;
              white-space: nowrap;
              -webkit-user-select: none;
              -moz-user-select: none;
              -ms-user-select: none;
              user-select: none;
              border-radius: 20px;
              padding: 40px 80px;
            }
          }
        }
      }
    }
  }
  .nav {
    display: flex;
    .li {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 25%;
      padding: 100px 0;
      font-size: 80px;
      text-align: center;
      transition: all 0.2s;
      &.act {
        color: #ffff00;
        font-weight: bold;
      }
    }
  }
}
</style>
