<template>
  <div id="test">
        <div class="main-container">
          <div class="pd-ltr-20">
          
              <div class="row">
                <div class="col-xl-12 mb-30">
                  <div class="card-box height-100-p pd-20">
                    <div class="mt-5"></div>
                      <h4 style="text-align:center;">รายการ</h4>
                      <pre>{{this.$store.getters.getResultPd}}</pre>
                      <div>
                        <div class="mt-3"></div>
                      </div>
                      <hr>
                  </div>
                </div>
              </div>


          </div>
        </div>
  </div>
</template>

<script>
export default {
    created() {
      // this.$store.dispatch('fetchDataFromDatabase');
    },
    mounted() {
      
    },
    methods: {

    },
    computed:{
      // data(){
      //   return this.$store.getters.getResultPd;
      // },
    },
    watch: {
      
    },
}

// ฟังก์ชันเปรียบเทียบข้อมูลออบเจกต์สองออบเจกต์ว่าเหมือนกันหรือไม่
// function isEqual(obj1, obj2) {
//   return JSON.stringify(obj1) === JSON.stringify(obj2);
// }
</script>

<style>

</style>