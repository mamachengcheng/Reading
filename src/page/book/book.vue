<template>
  <div>
      <head-bar head_bar='true' :head_title="book.book_info.book_name"></head-bar>
      <!-- 书籍基本信息 -->
      <mu-content-block style="padding-top: 70px;">
          <div class="book-detail-info">

              <img class="cover" :src="coverBaseUrl + book.book_info.cover"/>
              <div class="describe">
                  <h2 class="book-name">{{book.book_info.book_name}}</h2>
                  <div class="book-author">{{book.book_info.author}}</div>
                  <div class="book-rate">
                      <el-rate v-model="book.book_info.book_rank" disabled show-score text-color="#969ba3" score-template="{value}分"></el-rate>
                  </div>
                  <div class="book-meta">{{book.book_info.book_type}}</div>
                  <div class="book-meta">{{book.book_info.word_number}}｜{{book.book_info.update_state}}</div>
              </div>
        </div>
    </mu-content-block>

    <mu-divider/>

    <!-- 书籍简介 -->
    <mu-content-block>
      <section class="brief-info" :style="{maxHeight}" v-on:click="expandMore">
          <content>内容简介：{{book.book_info.book_describe}}</content>
          <span class="expand-more" :style="{visibility}">
              <mu-icon value="expand_more"></mu-icon>
          </span>
      </section>
    </mu-content-block>

    <mu-divider/>

    <!-- 章节目录 -->
    <mu-list>
      <mu-list-item title="目录" :to="'/book/' + book_id + '/catalog'">
        <mu-icon slot="right" value="chevron_right"/>
      </mu-list-item>
      <mu-divider/>
    </mu-list>
    <mu-divider/>

    <!-- 打赏物品数量 -->
    <mu-list>
        <mu-list-item title="去打赏" @click="openBottomSheet">
            <mu-icon slot="right" value="monetization_on"/>
        </mu-list-item>
        <mu-divider/>
        <mu-list-item>
            <mu-flexbox>
                <mu-flexbox-item　class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/1.png">
                    <br/><span>{{book.book_info.cat_ball_num}}</span><br/>个
                </mu-flexbox-item>
                <mu-flexbox-item class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/2.png">
                    <br/><span>{{book.book_info.catnip_num}}</span><br/>个
                </mu-flexbox-item>
                <mu-flexbox-item class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/3.png">
                    <br/><span>{{book.book_info.cat_stick_num}}</span><br/>个
                </mu-flexbox-item>
                <mu-flexbox-item class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/4.png">
                    <br/><span>{{book.book_info.cat_food_num}}</span><br/>个
                </mu-flexbox-item class="r-donate-flexbox">
                <mu-flexbox-item class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/5.png">
                    <br/><span>{{book.book_info.cat_fish_num}}</span><br/>个
                </mu-flexbox-item>
                <mu-flexbox-item class="r-donate-flexbox">
                    <img class="r-donate-object" src="../../assets/images/donate/6.png">
                    <br/><span>{{book.book_info.cat_house_num}}</span><br/>个
                </mu-flexbox-item>
            </mu-flexbox>
        </mu-list-item>
    </mu-list>

    <!-- 打赏弹出框 -->
    <mu-bottom-sheet :open="bottomSheet" @close="closeBottomSheet">
        <mu-list>
          <mu-sub-header>
            捧场
          </mu-sub-header>
          <mu-list-item>
            <mu-text-field label="数量" fullWidth="true" type="number"　labelFloat/><br/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="猫球　100猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/1.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="猫薄荷　500猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/2.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="逗猫棒　1000猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/3.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="鱼　3000猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/4.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="猫粮　5000猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/5.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-radio label="猫窝　10000猫币/个" name="group" nativeValue="simple1" v-model="value"/>
            <mu-avatar src="../static/images/donate/6.png" slot="rightAvatar"/>
          </mu-list-item>
          <mu-list-item>
            <mu-raised-button @click="closeBottomSheet"  class="demo-raised-button" label="打赏" fullWidth="true" secondary/>
          </mu-list-item>
        </mu-list>
    </mu-bottom-sheet>
    <mu-divider/>

    <!-- 评论列表 -->
    <mu-list style="padding-bottom: 50px;">
        <mu-list-item title="书友评论" @click="openCommentDialog">
        <mu-icon slot="right" value="create"/>
        </mu-list-item>
        <mu-divider/>
        <mu-list-item :title="book_comment.user_name" :describeText="book_comment.comment_date" v-for="book_comment in book.book_info.book_comment">
            <mu-avatar src="../static/images/avatar/avatar.jpg" slot="leftAvatar"/>
                <span>
                    <br/>
                    {{book_comment.comment_content}}
                </span>
            </mu-list-item>
        <mu-list-item>
            <mu-raised-button label="查看全部书评" fullWidth/>
        </mu-list-item>
    </mu-list>

    <!-- 评论弹出框 -->
    <mu-dialog :open="dialogComment" @close="closeCommentDialog">
        <mu-list>
            <mu-list-item>
                <mu-text-field fullWidth="true"　label="评论 :" multiLine :rows="4" :rowsMax="4" maxLength="20" labelFloat/><br/>
            </mu-list-item>
            <mu-list-item>
                <mu-raised-button @click="closeCommentDialog"  class="demo-raised-button" label="评论" fullWidth="true" secondary/>
            </mu-list-item>
        </mu-list>
    </mu-dialog>
    <mu-divider/>

    <!-- 底部操作按钮 -->
    <div style="position: fixed; bottom: 0px;background-color: #ffffff; width: 100%;">
        <mu-flat-button @click="chaseBook" label="追书" style="width: 32%; height: 45px;"/>
        <mu-raised-button　label="开始阅读" style="width: 32%; height: 50px;" primary/>
        <mu-flat-button @click="subscriberBook" label="自动订阅" style="width: 32%; height: 45px;"/>
    </div>

    <transition name="router-slid" mode="out-in">
        <router-view></router-view>
    </transition>
  </div>

</template>

<script>
    import Vue from 'vue'
    import headBar from '@/components/header/headBar'
    import {coverBaseUrl} from '@/config/env'
    import {mapState, mapActions} from 'vuex'
    import { Rate } from 'element-ui'
    import {chaseBookRequest, subscribeBookRequest} from '../../../src/service/getAccountData'

    Vue.use(Rate)

    export default {
      name: 'book',
      components: {
        headBar
      },
      data () {
        return {
          coverBaseUrl,
          book_id: '',
          maxHeight: '72px',        // 书籍简介行数
          visibility: 'visible',    // 简介更多图标是否可见

          head_title: '修仙神探',
          rate: 3.7,
          dialogComment: false,
          bottomSheet: false,
          bottomCommentSheet: false
        }
      },
      mounted() {
          this.book_id = this.$route.params.book_id;
          this.getBookInfoAction(this.book_id)
      },
      computed: {
          ...mapState([
                  'book',
              ])
      },
      methods: {
          ...mapActions([
            'getBookInfoAction'
          ]),
        expandMore () {
            this.maxHeight = this.maxHeight != 'none' ? 'none' : '72px'
            this.visibility = this.visibility != 'hidden' ? 'hidden' : 'visible'
        },
        closeBottomSheet () {
          this.bottomSheet = false;
        },
        openBottomSheet () {
          this.bottomSheet = true;
        },
        closeBottomCommentSheet () {
          this.bottomCommentSheet = false;
        },
        openBottomCommentSheet () {
          this.bottomCommentSheet = true;
        },
        openCommentDialog () {
          this.dialogComment = true;
        },
        closeCommentDialog () {
          this.dialogComment = false;
        },
        chaseBook () {
            chaseBookRequest(this.book_id)
        },
        subscriberBook () {
            subscribeBookRequest(this.book_id)
        }
      }
    }
</script>

<style scoped>

    .book-detail-info{
        position: relative;
        display: block;
    }

    .cover{
        float: left;
        margin-right: 10px;
        width: 5.25rem;
        height: 7.2rem;
        border-radius: 2px;
    }


    .describe{
        position: relative;
        line-height: 1.4rem;
    }

    .book-name{
        overflow: ellipsis;
        font-size: 100%;
        margin: 0;
    }

    .book-rate{
        font-size: .75rem;
    }

    .book-author{
        font-size: 0.75rem;
        margin-top: 0.2rem;
        color: #33373d;
    }

    .book-meta{
        margin-top: 0.15rem;
        font-size: 0.75rem;
        color: #33373d;
    }



    .brief-info{
        position: relative;
        line-height: 24px;
        overflow: hidden;
    }

    .expand-more {
        position: absolute;
        right: 0rem;
        width: 3rem;
        height: 1.5rem;
        bottom: 0px;
        color: #969ba3;
        text-align: right;
        background: linear-gradient(to right,rgba(255,255,255,0),#fff 1rem)
    }

    .r-bottom-button{
     width: 32%;
    }

    .r-donate-object{
      width: 80%;
    }

    .r-donate-flexbox{
    text-align: center;
    }

    .r-comment-sheet{
      position: fixed;
      top: 0px;
    }

    .router-slid-enter-active, .router-slid-leave-active {
      transition: all .4s;
    }
    .router-slid-enter, .router-slid-leave-active {
      transform: translate3d(2rem, 0, 0);
      opacity: 0;
    }
</style>
