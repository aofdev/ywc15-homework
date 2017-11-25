<template>
  <div>
    <v-layout row wrap>
      <v-flex md4>
      </v-flex>
      <v-flex md4>
        <div class="text-xs-center">
          <img src="../assets/logo.png" alt="" height="auto" width="100%">
          <h1 class="color1">ประกาศผู้มีสิทธิ์เข้าสัมภาษณ์</h1>
        </div>
      </v-flex>
      <v-flex md4>
      </v-flex>
    </v-layout>
    <br>
    <div v-show="Show">
      <div class="text-xs-center">
        <p class="title">การสัมภาษณ์จะจัดขึ้นในวันที่ 26 พฤศจิกายน 2560 ณ อาคาร ซี.พี.ทาวเวอร์ 1 (สีลม) ซึ่งจะแบ่งออกเป็น 2 รอบ คือ รอบช่วงเช้าตั้งแต่เวลา 9.00 น. ถึง 12.00 น. และ รอบช่วงบ่ายตั้งแต่เวลา 13.00 น. ถึง 18.00 น.</p>
        <br>
      </div>
      <v-layout row wrap>
        <v-flex md12>
          <h2 class="color2">สิ่งที่ต้องเตรียมมาในวันสัมภาษณ์</h2>
          <p></p>
          <p class="subheading"><span class="color1 title">1. บัตรประชาชน</span>สำหรับการแลกบัตรเข้าอาคาร ซี.พี.ทาวเวอร์ 1 (สีลม) และ <span class="color1 title">บัตรนักศึกษา</span>สำหรับการลงทะเบียนสัมภาษณ์ กรุณาแต่งกายด้วยชุดนักศึกษา</p>
          <p class="subheading"><span class="color1 title">2. การบ้านและสิ่งที่กรรมการสาขากำหนดไว้</span> กรุณาอ่านรายละเอียดการบ้านและสิ่งที่กรรมการให้เตรียมมาให้ครบถ้วน หากสาขาใดต้องใช้โน้ตบุ๊ค ควรชาร์ตแบตเตอรี่และเตรียมอินเทอร์เน็ตส่วนตัวมาให้พร้อม เนื่องจากสถานที่ไม่มีบริการอินเทอร์เน็ตให้ใช้</p>
          <p class="subheading"><span class="color1 title">3. Portfolio</span> สามารถนำมาประกอบการสัมภาษณ์ได้ สำหรับน้อง ๆ สาขาดีไซน์จะต้องนำ Portfolio มาด้วยทุกคน</p>
        </v-flex>
      </v-layout>
      <v-layout row wrap>
        <v-flex md6>
          <h2 class="color2">การเดินทางมาสัมภาษณ์</h2>
          <p></p>
          <p class="subheading"><span class="color1 title">1. </span>ด้วยรถไฟฟ้า BTS สามารถลงสถานีศาลาแดง ณ ทางออกที่ 2</p>
          <p class="subheading"><span class="color1 title">2. </span>ด้วยรถไฟฟ้า MRT สามารถลงสถานีสีลม ณ ทางออกที่ 2 โดยเดินเรียบทางเท้าไปตามถนนสีลม</p>
          <p class="subheading"><span class="color1 title">3. </span>ด้วยรถประจำทาง สามารถขึ้นใช้บริการสาย 15, 77, 155, 504, 177, 76</p>
        </v-flex>
        <v-flex md2></v-flex>
        <v-flex md4>
          <h2 class="color2">สอบถามเพิ่มเติมติดต่อ</h2>
          <p></p>
          <p class="subheading"><span class="color1 title">พี่เบ๊บ: </span>064-174-7080</p>
          <p class="subheading"><span class="color1 title">พี่ฟง: </span>092-458-7067</p>
          <p class="subheading"><span class="color1 title">พี่เบนซ์: </span>พี่เบนซ์: 085-666-7571</p>
        </v-flex>
      </v-layout>
  
    </div>
    <br>
    <transition name="scale-transition">
      <itemMain v-if="getMajor == 'allMajor'" :items="majorAll"></itemMain>
    </transition>
    <transition name="scale-transition">
      <itemList v-if="getMajor == 'content'" :items="content" :icon="'content_paste'" :btnColor="'light-blue white--text'"></itemList>
      <itemList v-if="getMajor == 'design'" :items="design" :icon="'color_lens'" :btnColor="'amber white--text'"></itemList>
      <itemList v-if="getMajor == 'marketing'" :items="marketing" :icon="'pie_chart'" :btnColor="'red white--text'"></itemList>
      <itemList v-if="getMajor == 'programming'" :items="programming" :icon="'code'" :btnColor="'teal white--text'"></itemList>
    </transition>
  </div>
</template>

<script>
  export default {
    props: ['getMajor', 'content', 'design', 'marketing', 'programming', 'majorAll'],
    data() {
      return {
        Show: true
      }
    },
    watch: {
      getMajor: function(res) {
        if (res == "home") {
          this.Show = true
        } else {
          this.Show = false
        }
      }
    },
    components: {
      'itemList': () => import ('./itemList.vue'),
      'itemMain': () => import ('./itemMain.vue')
    }
  }
</script>
<style>
p {color: #5A5757}
.color1{color:#b04227}
.color2{color: #9118A7}
</style>



