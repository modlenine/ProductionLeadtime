<template>
  <div>

    <div style="overflow-x:auto;">
    <table id="dataMachinePlan" class="table table-striped table-bordered mt-3" cellspacing="0" style="width:3700px">
        <thead>
            <tr>
            <th>PD</th>
            <th>Mix</th>
            <th>Extrude</th>
            <th>Separate</th>
            <th>Company</th>
            <th>Work type</th>
            <th>Item No</th>
            <th>Batch No</th>
            <th>QTY</th>
            <th>Start ใบเบิก</th>
            <th>Start ใบเบิก > จอง Lot</th>
            <th>จอง Lot</th>
            <th>จอง Lot > จ่ายของ</th>
            <th>จ่ายของ</th>
            <th>จ่ายของ > จัดเตรียม</th>
            <th>จัดเตรียม</th>
            <th>Mixer</th>
            <th>Mix Leadtime</th>
            <th>Mix Waittime</th>
            <th>Next Station Waittime</th>
            <th>Extrude</th>
            <th>Ext Leadtime</th>
            <th>Ext Waittime</th>
            <th>Next Station Waittime</th>
            <th>Separate</th>
            <th>Sep Leadtime</th>
            <th>Sep Waittime</th>
            </tr>
        </thead>
        <tbody v-if="this.items !== 0">
            <tr v-for="item in items" :key="item.Prodid" :class="{
              'bgNextStationFail': changeTrcolor(item.nextStationFail , item.resultCheckAdjust) === 'special',
              'notNormaltype': changeTrcolor(item.nextStationFail , item.resultCheckAdjust) === 'adjust_rerun'
              }">
                <td>{{ item.Prodid }}</td>
                <td>{{ item.wrkctrid_10 }}</td>
                <td>{{ item.wrkctrid_20 }}</td>
                <td>{{ item.wrkctrid_30 }}</td>
                <td>{{ item.dataAreaid }}</td>
                <td>{{ item.resultCheckAdjust }}</td>
                <td>{{ item.itemno }}</td>
                <td>{{ item.batchno }}</td>
                <td>{{ item.qtysched }}</td>
                <td>{{ item.startDocDateTime }}</td>
                <td>{{ item.leadtimeStartDocToReservedDecimal }}<br>{{ item.leadtimeStartDocToReserved }}</td>
                <td>{{ item.reservedDocDateTime }}</td>
                <td>{{ item.leadtimeReservedToProcureDecimal }}<br>{{ item.leadtimeReservedToProcure }}</td>
                <td>{{ item.procureDocDateTime }}</td>
                <td>{{ item.leadtimeProcureToProcuredoneDecimal }}<br>{{ item.leadtimeProcureToProcuredone }}</td>
                <td>{{ item.procureDoneDocDateTime }}</td>
                <td>{{item.mixStartDate}}</td>
                <td>{{ item.dataMixLeadtime }}<br>{{ item.dataMixLeadtime2 }}</td>
                <td>{{ item.dataMixWaitTime }}<br>{{ item.dataMixWaitTime2 }}</td>
                <td>{{ item.dataExtWait1 }}<br>{{ item.dataExtWait2 }}</td>
                <td>{{item.extStartDate}}</td>
                <td>{{ item.dataExtLeadtime }}<br>{{ item.dataExtLeadtime2 }}</td>
                <td>{{ item.dataExtWaitTime }}<br>{{ item.dataExtWaitTime2 }}</td>
                <td>{{ item.dataSepWait1 }}<br>{{ item.dataSepWait2 }}</td>
                <td>{{item.sepStartDate}}</td>
                <td>{{ item.dataSepLeadtime }}<br>{{ item.dataSepLeadtime2 }}</td>
                <td>{{ item.dataSepWaitTime }}<br>{{ item.dataSepWaitTime2 }}</td>
            </tr>
        </tbody>

        <tbody v-else>
          <tr>
            <td colspan="20">ไม่พบข้อมูล</td>
          </tr>
        </tbody>
    </table>
    </div>

    <div class="text-center mt-3" v-if="this.items !== 0">
      <button class="btn btn-info  btn-sm" @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span v-for="(n , indexdown) in Math.min(3, totalPages)" :key="'down_'+indexdown">
        <button class="btn btn-info  btn-sm" v-if="createBtnpage(currentPage , n) === true" @click="goToPage((currentPage - 1) - (3 - n))">
          {{ (currentPage - 1) - (3 - n) }}
        </button>
      </span>
      <button class="btn btn-info  btn-sm" disabled>{{ currentPage }}</button>
      <span v-for="(m , indexup) in Math.min(3, totalPages - currentPage)" :key="'up_'+indexup">
        <button class="btn btn-info  btn-sm" @click="goToPage(currentPage + m)">
          {{ currentPage + m }}
        </button>
      </span>
      <button class="btn btn-info  btn-sm" @click="nextPage" :disabled="currentPage === totalPages">Next</button>
      <p class="mt-2">
        <span><b>Total Item : </b>{{totalItem}}</span>&nbsp;&nbsp;<span><b>Total Page : </b>{{totalPages}}</span>
      </p>
      <p>
        <span id="showState"></span><br>
        <span>{{this.$store.state.filterInput.worktype}}</span>
      </p>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2'
import $ from 'jquery'
export default {
  data() {
    return {
      url:this.$store.getters.getUrl,
      items: [],         // Data to display
      currentPage: 1,    // Current page
      totalPages: 1,     // Total pages
      itemsPerPage: 5 ,  // Items per page
      totalItem:1,
    };
  },
  mounted() {
    const proxy = this;
    proxy.getData();

    $('#btn-filter-search').click(function(){
      proxy.currentPage = 1;
      const startdate = $('#ip-filter-startdate').val();
      const enddate = $('#ip-filter-enddate').val();
      const prodid = $('#ip-filter-prodid').val();
      const batchno = $('#ip-filter-batchno').val();
      const itemno = $('#ip-filter-itemno').val();
      const worktype = $('input[name="ip-filter-wt[]"]:checked');
      let worktypeSelect = worktype.map(function() {
        return $(this).val();
      }).get();
      proxy.getData(startdate , enddate , prodid , batchno , itemno , worktypeSelect);

      proxy.$store.state.filterInput.startdate = startdate;
      proxy.$store.state.filterInput.enddate = enddate;
      proxy.$store.state.filterInput.prodid = prodid;
      proxy.$store.state.filterInput.batchno = batchno;
      proxy.$store.state.filterInput.itemno = itemno;
      proxy.$store.state.filterInput.worktype = worktypeSelect;
    });

    $('#btn-filter-export').click(function(){

      if($('#ip-filter-startdate').val() === "" || $('#ip-filter-enddate').val() === ""){
        Swal.fire({
            title: 'กรุณาเลือกวันที่ของข้อมูล',
            icon: 'error',
            showConfirmButton: false,
            timer:3000
        });
      }else{
        proxy.currentPage = 1;
        const startdate = $('#ip-filter-startdate').val();
        const enddate = $('#ip-filter-enddate').val();
        const prodid = $('#ip-filter-prodid').val();
        const batchno = $('#ip-filter-batchno').val();
        const itemno = $('#ip-filter-itemno').val();

        proxy.$store.state.filterInput.startdate = startdate;
        proxy.$store.state.filterInput.enddate = enddate;
        proxy.$store.state.filterInput.prodid = prodid;
        proxy.$store.state.filterInput.batchno = batchno;
        proxy.$store.state.filterInput.itemno = itemno;

        $.ajax({
          url:proxy.url+"intsys/pdl/pdl_backend/mainapi/exportdata",
          method:"POST",
          data:{
            startdate:startdate,
            enddate:enddate,
            prodid:prodid,
            batchno:batchno,
            itemno:itemno
          },
          xhrFields: {
            responseType: 'blob' // ระบุ responseType เป็น 'blob' เพื่อรับข้อมูลเป็นไบนารี
          },
          beforeSend:function(){
            Swal.fire({
              title: 'กำลังโหลดข้อมูล',
              text: 'กรุณารอสักครู่...',
              allowOutsideClick: false,
              allowEscapeKey: false,
            });
          },
          success:function(data){
            Swal.close();
            let currentMilliseconds = Date.now();

            // สร้าง URL สำหรับไฟล์ Excel
            let blob = new Blob([data], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
            let url = window.URL.createObjectURL(blob);
            
            // สร้างลิงก์สำหรับดาวน์โหลดไฟล์ Excel
            let a = document.createElement('a');
            a.href = url;
            a.download = 'รายงาน_Production_leadtime_'+currentMilliseconds+'.xlsx';
            a.style.display = 'none';
            document.body.appendChild(a);
            a.click();
            window.URL.revokeObjectURL(url);

          },
        });
      }
    });

    $('#btn-filter-resetSearch').click(function(){
      proxy.currentPage = 1;
      proxy.$store.state.filterInput.startdate = "";
      proxy.$store.state.filterInput.enddate = "";
      proxy.$store.state.filterInput.prodid = "";
      proxy.$store.state.filterInput.batchno = "";
      proxy.$store.state.filterInput.itemno = "";
      proxy.$store.state.filterInput.worktype = "";
      $('#ip-filter-startdate').val('');
      $('#ip-filter-enddate').val('');
      $('#ip-filter-prodid').val('');
      $('#ip-filter-batchno').val('');
      $('#ip-filter-itemno').val('');
      $('input[name="ip-filter-wt[]"]:checked').prop('checked', false);
      proxy.getData();
    });

  },
  methods: {
    getData(startdate="" , enddate="" , prodid="" , batchno="" , itemno="") {
      Swal.fire({
        title: 'กำลังโหลดข้อมูล',
        text: 'กรุณารอสักครู่...',
        allowOutsideClick: false,
        allowEscapeKey: false,
      });
      axios.post(this.url+`intsys/pdl/pdl_backend/mainapi/checkdataworkplan/`,{
        action:"getdata",
        page:this.currentPage,
        startdate:startdate,
        enddate:enddate,
        prodid:prodid,
        batchno:batchno,
        itemno:itemno,
      }).then(response => {
        if(response.data.status == "Select Data Success"){
          Swal.close();
          console.log(response.data);
          this.items = response.data.data;
          this.totalPages = Math.ceil(response.data.total / this.itemsPerPage);
          this.totalItem = response.data.total;
        }else if(response.data.status == "Not Found Data"){
          Swal.close();
          console.log(response.data);
          this.items = response.data.data;
          this.totalPages = Math.ceil(response.data.total / this.itemsPerPage);
          this.totalItem = response.data.total;
        }

      }).catch(error => console.error(error));
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.getData(
          this.$store.state.filterInput.startdate,
          this.$store.state.filterInput.enddate,
          this.$store.state.filterInput.prodid,
          this.$store.state.filterInput.batchno,
          this.$store.state.filterInput.itemno,
        );
        Swal.fire({
            title: 'กำลังโหลดข้อมูล',
            text: 'กรุณารอสักครู่...',
            allowOutsideClick: false,
            allowEscapeKey: false,
        });
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.getData(
          this.$store.state.filterInput.startdate,
          this.$store.state.filterInput.enddate,
          this.$store.state.filterInput.prodid,
          this.$store.state.filterInput.batchno,
          this.$store.state.filterInput.itemno,
        );
        Swal.fire({
            title: 'กำลังโหลดข้อมูล',
            text: 'กรุณารอสักครู่...',
            allowOutsideClick: false,
            allowEscapeKey: false,
        });
      }
    },
    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
        this.getData(
          this.$store.state.filterInput.startdate,
          this.$store.state.filterInput.enddate,
          this.$store.state.filterInput.prodid,
          this.$store.state.filterInput.batchno,
          this.$store.state.filterInput.itemno,
        );
        Swal.fire({
            title: 'กำลังโหลดข้อมูล',
            text: 'กรุณารอสักครู่...',
            allowOutsideClick: false,
            allowEscapeKey: false,
        });
      }
    },
    createBtnpage(page , n){
      if(page === 1 || (page - 1) - (3 - n) <= 0){
        return false;
      }else{
        return true;
      }
    },
    changeTrcolor(nextStationFail , resultCheckAdjust){
      if(nextStationFail === true){
        return 'special';
      }else if(resultCheckAdjust !== 'Normal'){
        return 'adjust_rerun';
      }
    },
  }
};
</script>
<style scoped>
  .bgNextStationFail{
    background-color:#FF6600 !important;
  }
  .notNormaltype{
    background-color:#00CCFF !important;
  }
</style>
