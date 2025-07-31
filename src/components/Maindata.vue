<template>
  <div>

    <div style="overflow-x:auto;">
      <table id="dataMachinePlan" class="table table-bordered mt-3" cellspacing="0" style="width:5000px">
        <thead>
          <tr>
            <th style="width:150px;">PD</th>
            <th style="width:220px;">Work Center</th>

            <!-- <th style="width:80px;">Company</th> -->
            <th>Work type</th>
            <th>Item Details</th>
            <th>QTY</th>
            <th style="width:120px;">Start ใบเบิก</th>
            <th style="width:120px;">Startใบเบิก<br>ถึง<br>จองLot</th>
            <th style="width:120px;">จองLot</th>
            <th style="width:120px;">จองLot<br>ถึง<br>จ่ายของเสร็จ</th>
            <th style="width:120px;">จ่ายของเสร็จ</th>
            <th style="width:150px;">จ่ายของเสร็จ<br>ถึง<br>จัดเตรียมเสร็จ</th>
            <th style="width:120px;">จัดเตรียมเสร็จ</th>
            <th style="width:150px;">จัดเตรียมเสร็จ<br>ถึง<br>Work Center 10</th>
            <th style="width:130px;">Work Center 10<br>
              <span style="font-size:11px">(เวลาเริ่มของรอบแรก)</span>
            </th>
            <th>Work Center 10 Leadtime<br>
              <span style="font-size:11px">(เวลาที่ใช้ทั้งหมด)</span>
            </th>
            <th>Work Center 10 Waittime<br>
              <span style="font-size:11px">(เวลาที่สูญเสียในแต่ละรอบรวมกัน)</span>
            </th>
            <th>Next Station Waittime</th>
            <th style="width:130px;">Work Center 20<br>
              <span style="font-size:11px">(เวลาเริ่มของรอบแรก)</span>
            </th>
            <th>Work Center 20 Leadtime<br>
              <span style="font-size:11px">(เวลาที่ใช้ทั้งหมด)</span>
            </th>
            <th>Work Center 20 Waittime<br>
              <span style="font-size:11px">(เวลาที่สูญเสียในแต่ละรอบรวมกัน)</span>
            </th>
            <th>Next Station Waittime</th>
            <th style="width:130px;">Work Center 30<br>
              <span style="font-size:11px">(เวลาเริ่มของรอบแรก)</span>
            </th>
            <th>Work Center 30 Leadtime<br>
              <span style="font-size:11px">(เวลาที่ใช้ทั้งหมด)</span>
            </th>
            <th>Work Center 30 Waittime<br>
              <span style="font-size:11px">(เวลาที่สูญเสียในแต่ละรอบรวมกัน)</span>
            </th>
            <th>ผลิตเสร็จ</th>
            <th>ผลิตเสร็จ<br>ถึง<br>WH Post GR</th>
            <th>PD Create GR<br>
              <span style="font-size:11px">(ใบแรก)</span>
            </th>
            <th>WH Post GR<br>
              <span style="font-size:11px">(ใบสุดท้าย)</span>
            </th>
            <th>Confirm Ship Date</th>
          </tr>
        </thead>
        <tbody v-if="this.items !== 0">
          <template v-for="item in items">
            <tr :key="item.Prodid" :class="{
              'bgNextStationFail': changeTrcolor(item.nextStationFail, item.resultCheckAdjust) === 'special',
              'notNormaltype': changeTrcolor(item.nextStationFail, item.resultCheckAdjust) === 'adjust_rerun',
              'cursor-pointer': item.resultCheckAdjust === 'Special'
            }" @click="item.resultCheckAdjust == 'Special' && toggleSubRow(item.Prodid, item.dataAreaid)">
              <td>
                <b>PD : </b>{{ item.Prodid }}<br>
                <b>Company : </b>{{ item.dataAreaid }}
              </td>
              <td>
                <b>WC 10 :</b> {{ item.wrkctrid_10 }}<br>
                <b>WC 20 :</b> {{ item.wrkctrid_20 }}<br>
                <b>WC 30 :</b> {{ item.wrkctrid_30 }}<br>
              </td>
              <!-- <td>{{ item.dataAreaid }}</td> -->
              <td>{{ item.resultCheckAdjust }}</td>
              <td>
                <span><b>Item No.</b> {{ item.itemno }}</span><br>
                <span><b>Batch No.</b> {{ item.batchno }}</span><br>
              </td>
              <td>{{ item.qtysched }}</td>
              <td>{{ item.startDocDateTime }}</td>
              <td>{{ item.leadtimeStartDocToReservedDecimal }}<br>{{ item.leadtimeStartDocToReserved }}</td>
              <td>{{ item.reservedDocDateTime }}</td>
              <td>{{ item.leadtimeReservedToProcureDecimal }}<br>{{ item.leadtimeReservedToProcure }}</td>
              <td>{{ item.procureDocDateTime }}</td>
              <td>{{ item.leadtimeProcureToProcuredoneDecimal }}<br>{{ item.leadtimeProcureToProcuredone }}</td>
              <td>{{ item.procureDoneDocDateTime }}</td>
              <td>{{ item.procureDoneDocTomixStartDateDecimal }}<br>{{ item.procureDoneDocTomixStartDate }}</td>
              <td>{{ item.wc10StartDate }}</td>
              <td>{{ item.data10Leadtime }}<br>{{ item.data10Leadtime2 }}</td>
              <td>{{ item.totalWaitSeconds_10 }}<br>{{ item.totalWaitSeconds_10_2 }}</td>
              <td>{{ item.dataExtWait1 }}<br>{{ item.dataExtWait2 }}</td>
              <td>{{ item.wc20StartDate }}</td>
              <td>{{ item.data20Leadtime }}<br>{{ item.data20Leadtime2 }}</td>
              <td>{{ item.totalWaitSeconds_20 }}<br>{{ item.totalWaitSeconds_20_2 }}</td>
              <td>{{ item.dataSepWait1 }}<br>{{ item.dataSepWait2 }}</td>
              <td>{{ item.sepStartDate }}</td>
              <td>{{ item.totalLeadTimeInSeconds_30 }}<br>{{ item.totalLeadTimeInSeconds_30_2 }}</td>
              <td>{{ item.dataSepWaitTime }}<br>{{ item.dataSepWaitTime2 }}</td>
              <td>{{ item.production_completedDate }}</td>
              <td>{{ item.productionCompToGrDecimal }}<br>{{ item.productionCompToGr }}</td>
              <td>{{ item.createGRDatetime }}</td>
              <td>{{ item.grDatetime }}</td>
              <td>{{ item.confirmshippingdate }}</td>
            </tr>

            <!-- แถวย่อย (subtable) เฉพาะเมื่อเงื่อนไขตรง -->
            <!-- แถวย่อย Timeline -->
            <tr v-if="item.resultCheckAdjust === 'Special' && showSubRow[item.Prodid]" :key="item.Prodid + '_timeline'">
              <td colspan="30" style="background-color: #f2f2f2; padding: 20px;">
                <table>
                  <thead class="timeline-grid" ref="timeline">
                    <tr>
                      <th>วันที่</th>
                      <th v-for="h in 24" :key="h" :ref="'hour-' + h">
                        {{ String(h).padStart(2, '0') }}:00
                      </th>
                    </tr>
                  </thead>
                  <!-- <tbody>
                    <tr v-for="(group, dateStr) in groupByDate(subDataMap[item.Prodid])" :key="dateStr">
                      <td class="date-label">{{ dateStr }}</td>
                      <td colspan="24" style="position: relative; min-height: 120px;">
                        <div v-for="(row, i) in group" :key="i" class="timeline-block"
                          :style="[getBlockStyle(row, i, group), getOpelColor(row.OpeNum)]"
                          :title="row.OpeId + ' | ' + formatTime(row.TransDateFormTime) + ' - ' + formatTime(row.TransDateToTime)">
                          {{ row.OpeId }} : {{ formatTime(row.TransDateFormTime) }} - {{ formatTime(row.TransDateToTime)
                          }}
                        </div>
                      </td>
                    </tr>
                  </tbody> -->
                  <tbody>
  <template v-for="(rowArrs, dateStr) in groupByDate(subDataMap[item.Prodid])">
    <tr v-for="(rowArr, i) in rowArrs" :key="dateStr+'-'+i">
      <td v-if="i===0" class="date-label" :rowspan="rowArrs.length">{{ dateStr }}</td>
      <td v-else style="display:none"></td>
      <td colspan="24" style="position: relative; min-height: 24px;">
        <div v-for="row in rowArr" :key="row.OpeId+row.TransDateFormTime"
          class="timeline-block"
          :style="[getBlockStyle(row), getOpelColor(row.OpeNum)]"
          :title="row.OpeId + ' | ' + formatTime(row.TransDateFormTime) + ' - ' + formatTime(row.TransDateToTime)">
          {{ row.OpeId }} : {{ formatTime(row.TransDateFormTime) }} - {{ formatTime(row.TransDateToTime) }}
        </div>
      </td>
    </tr>
  </template>
</tbody>
                </table>
              </td>
            </tr>
          </template>
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
      <span v-for="(n, indexdown) in Math.min(3, totalPages)" :key="'down_' + indexdown">
        <button class="btn btn-info  btn-sm" v-if="createBtnpage(currentPage, n) === true"
          @click="goToPage((currentPage - 1) - (3 - n))">
          {{ (currentPage - 1) - (3 - n) }}
        </button>
      </span>
      <button class="btn btn-info  btn-sm" disabled>{{ currentPage }}</button>
      <span v-for="(m, indexup) in Math.min(3, totalPages - currentPage)" :key="'up_' + indexup">
        <button class="btn btn-info  btn-sm" @click="goToPage(currentPage + m)">
          {{ currentPage + m }}
        </button>
      </span>
      <button class="btn btn-info  btn-sm" @click="nextPage" :disabled="currentPage === totalPages">Next</button>
      <p class="mt-2">
        <span><b>Total Item : </b>{{ totalItem }}</span>&nbsp;&nbsp;<span><b>Total Page : </b>{{ totalPages }}</span>
      </p>
      <!-- <p>
        <span id="showState"></span><br>
        <span>{{this.$store.state.filterInput.worktype}}</span>
      </p> -->
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
      url: this.$store.getters.getUrl,
      items: [],         // Data to display
      currentPage: 1,    // Current page
      totalPages: 1,     // Total pages
      itemsPerPage: 5,  // Items per page
      totalItem: 1,
      showSubRow: {}, // จะใช้เก็บค่าของแต่ละ Prodid ว่าเปิด subtable อยู่หรือไม่
      subDataMap: {}, // เก็บข้อมูลย่อยแต่ละ prodid

      hourWidths: [], // ใช้เก็บความกว้างแต่ละชั่วโมง
    };
  },
  mounted() {
    const proxy = this;

    proxy.getData();

    $('#btn-filter-search').click(function () {
      proxy.currentPage = 1;
      const startdate = $('#ip-filter-startdate').val();
      const enddate = $('#ip-filter-enddate').val();
      const prodid = $('#ip-filter-prodid').val();
      const batchno = $('#ip-filter-batchno').val();
      const itemno = $('#ip-filter-itemno').val();
      const worktype = $('input[name="ip-filter-wt[]"]:checked');
      let worktypeSelect = worktype.map(function () {
        return $(this).val();
      }).get();
      proxy.getData(startdate, enddate, prodid, batchno, itemno, worktypeSelect);

      proxy.$store.state.filterInput.startdate = startdate;
      proxy.$store.state.filterInput.enddate = enddate;
      proxy.$store.state.filterInput.prodid = prodid;
      proxy.$store.state.filterInput.batchno = batchno;
      proxy.$store.state.filterInput.itemno = itemno;
      proxy.$store.state.filterInput.worktype = worktypeSelect;
    });

    $('#btn-filter-export').click(function () {

      if ($('#ip-filter-startdate').val() === "" || $('#ip-filter-enddate').val() === "") {
        Swal.fire({
          title: 'กรุณาเลือกวันที่ของข้อมูล',
          icon: 'error',
          showConfirmButton: false,
          timer: 3000
        });
      } else {
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
          url: proxy.url + "intsys/pdl/pdl_backend/mainapi/exportdata",
          method: "POST",
          data: {
            startdate: startdate,
            enddate: enddate,
            prodid: prodid,
            batchno: batchno,
            itemno: itemno
          },
          xhrFields: {
            responseType: 'blob' // ระบุ responseType เป็น 'blob' เพื่อรับข้อมูลเป็นไบนารี
          },
          beforeSend: function () {
            Swal.fire({
              title: 'กำลังโหลดข้อมูล',
              text: 'กรุณารอสักครู่...',
              allowOutsideClick: false,
              allowEscapeKey: false,
            });
          },
          success: function (data) {
            Swal.close();
            let currentMilliseconds = Date.now();

            // สร้าง URL สำหรับไฟล์ Excel
            let blob = new Blob([data], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
            let url = window.URL.createObjectURL(blob);

            // สร้างลิงก์สำหรับดาวน์โหลดไฟล์ Excel
            let a = document.createElement('a');
            a.href = url;
            a.download = 'รายงาน_Production_leadtime_' + currentMilliseconds + '.xlsx';
            a.style.display = 'none';
            document.body.appendChild(a);
            a.click();
            window.URL.revokeObjectURL(url);

            // console.log(JSON.parse(data));

          },
        });
      }
    });

    $('#btn-filter-resetSearch').click(function () {
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
    getData(startdate = "", enddate = "", prodid = "", batchno = "", itemno = "") {
      Swal.fire({
        title: 'กำลังโหลดข้อมูล',
        text: 'กรุณารอสักครู่...',
        allowOutsideClick: false,
        allowEscapeKey: false,
      });
      axios.post(this.url + `intsys/pdl/pdl_backend/mainapi/checkdataworkplan/`, {
        action: "getdata",
        page: this.currentPage,
        startdate: startdate,
        enddate: enddate,
        prodid: prodid,
        batchno: batchno,
        itemno: itemno,
      }).then(response => {
        if (response.data.status == "Select Data Success") {
          Swal.close();
          console.log(response.data);
          this.items = response.data.data;
          this.totalPages = Math.ceil(response.data.total / this.itemsPerPage);
          this.totalItem = response.data.total;
        } else if (response.data.status == "Not Found Data") {
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
    createBtnpage(page, n) {
      if (page === 1 || (page - 1) - (3 - n) <= 0) {
        return false;
      } else {
        return true;
      }
    },
    changeTrcolor(nextStationFail, resultCheckAdjust) {
      if (nextStationFail === true) {
        return 'special';
      } else if (resultCheckAdjust !== 'Normal') {
        return 'adjust_rerun';
      }
    },
    async loadSubData(prodid, areaid) {
      //ถ้ามีข้อมูลแล้วไม่ต้องโหลดซ้ำ
      if (this.subDataMap[prodid]) return;

      const formData = new FormData();
      formData.append("prodid", prodid);
      formData.append("areaid", areaid);

      try {
        const response = await axios.post(this.url + "intsys/pdl/pdl_backend/mainapi/loadJobcard", formData);
        console.log(response.data);
        if (response.data.status == "success") {
          this.$set(this.subDataMap, prodid, response.data.result);
          this.$nextTick(() => {
            this.calculateHourWidths();
          });
        } else {
          this.$set(this.subDataMap, prodid, []);
        }
      } catch (error) {
        console.error("โหลดข้อมูล Subtable ล้มเหลว", error);
        this.$set(this.subDataMap, prodid, []);
      }
    },
    toggleSubRow(prodid, areaid) {
      this.$set(this.showSubRow, prodid, !this.showSubRow[prodid]);
      if (this.showSubRow[prodid]) {
        this.loadSubData(prodid, areaid);
        this.$nextTick(() => {
          this.calculateHourWidths();
        });
      }
    },
    formatDate(datetimeStr) {
      const d = new Date(datetimeStr);
      return d.toLocaleDateString('th-TH', {
        day: '2-digit', month: '2-digit', year: 'numeric'
      });
    },
    formatTime(datetimeStr) {
      const d = new Date(datetimeStr);
      return d.toLocaleTimeString('th-TH', {
        hour: '2-digit', minute: '2-digit'
      });
    },
    groupByDate(data) {
  if (!data) return {};
  const map = {};
  data.forEach(row => {
    const dateStr = this.formatDate(row.TransDateFormTime);
    if (!map[dateStr]) map[dateStr] = [];
    map[dateStr].push([row]); // ใส่เป็น array เดี่ยว
  });
  return map;
},

getBlockStyle(row) {
  const start = new Date(row.TransDateFormTime);
  const end = new Date(row.TransDateToTime);
  const startHour = start.getHours();
  const startMin = start.getMinutes();
  const endHour = end.getHours();
  const endMin = end.getMinutes();

  let left = 0, width = 0;

  for (let h = 0; h < startHour; h++) {
    left += this.hourWidths[h] || 0;
  }
  let minWidth = this.hourWidths[startHour] ? this.hourWidths[startHour] / 60 : 0;
  left += minWidth * startMin;

  if (startHour === endHour) {
    width = ((endMin - startMin) * minWidth);
  } else {
    width += (60 - startMin) * minWidth;
    for (let h = startHour + 1; h < endHour; h++) {
      width += this.hourWidths[h] || 0;
    }
    let lastMinWidth = this.hourWidths[endHour] ? this.hourWidths[endHour] / 60 : 0;
    width += endMin * lastMinWidth;
  }

  return {
    left: left + 'px',
    width: width + 'px',
    top: '4px', // ไม่ต้อง stack top index แล้ว
    position: 'absolute'
  };
},


    // getBlockStyle(row, index, allRows) {
    //   // เวลาต้น-จบ
    //   const start = new Date(row.TransDateFormTime);
    //   const end = new Date(row.TransDateToTime);

    //   const startHour = start.getHours();
    //   const startMin = start.getMinutes();
    //   const endHour = end.getHours();
    //   const endMin = end.getMinutes();

    //   // ความกว้างรวมของแต่ละชม.
    //   let left = 0, width = 0;

    //   // คำนวณ left (px) = ผลรวม width ของ hour ก่อนหน้า + (width เฉลี่ยต่อ 1 นาที x นาที)
    //   for (let h = 0; h < startHour; h++) {
    //     left += this.hourWidths[h] || 0;
    //   }
    //   // เฉลี่ย 1 นาทีใน hour นี้กี่ px
    //   let minWidth = this.hourWidths[startHour] ? this.hourWidths[startHour] / 60 : 0;
    //   left += minWidth * startMin;

    //   // คำนวณ width
    //   // (กรณีข้ามหลายชม. เช่น 09:30 - 13:45)
    //   if (startHour === endHour) {
    //     width = ((endMin - startMin) * minWidth);
    //   } else {
    //     // นาทีของชั่วโมงเริ่มต้น
    //     width += (60 - startMin) * minWidth;
    //     // ชั่วโมงเต็มระหว่างนั้น
    //     for (let h = startHour + 1; h < endHour; h++) {
    //       width += this.hourWidths[h] || 0;
    //     }
    //     // นาทีของชั่วโมงสุดท้าย
    //     let lastMinWidth = this.hourWidths[endHour] ? this.hourWidths[endHour] / 60 : 0;
    //     width += endMin * lastMinWidth;
    //   }

    //   // Stack top เช่นเดิม
    //   let topIndex = 0;
    //   for (let j = 0; j < index; j++) {
    //     const other = allRows[j];
    //     const oStart = new Date(other.TransDateFormTime);
    //     const oEnd = new Date(other.TransDateToTime);
    //     if (
    //       start < oEnd &&
    //       end > oStart
    //     ) {
    //       topIndex++;
    //     }
    //   }

    //   return {
    //     left: left + 'px',
    //     width: width + 'px',
    //     top: `${topIndex * 26}px`,
    //     position: 'absolute'
    //   };
    // },

    getOpelColor(openum) {
      const colors = {
        10: '#4CAF50',
        20: '#2196F3',
        30: '#FF9800'
      };
      return {
        backgroundColor: colors[openum.toLowerCase()] || '#9E9E9E'
      };
    },

    calculateHourWidths() {
      this.hourWidths = [];
      for (let i = 1; i <= 24; i++) {
        const th = this.$refs['hour-' + i];
        if (th && th[0]) {
          this.hourWidths.push(th[0].offsetWidth);
        } else {
          this.hourWidths.push(100); // fallback เผื่อหาไม่ได้
        }
      }
    },


  }
};
</script>
<style scoped>
.bgNextStationFail {
  background-color: #FF6600 !important;
}

.notNormaltype {
  background-color: #00CCFF !important;
}

th {
  vertical-align: middle !important;
  text-align: center;
}

.cursor-pointer {
  cursor: pointer;
  transition: background 0.2s;
}

.cursor-pointer:hover {
  background-color: #ad4a08 !important;
}

.timeline-grid {
  border-collapse: collapse;
  width: 100%;
  font-size: 12px;
}

.timeline-grid td[colspan="24"] {
  position: relative;
  min-height: 48px;
}

.timeline-grid th,
.timeline-grid td {
  border: 1px solid #ccc;
  text-align: center;
  vertical-align: top;
}

.date-label {
  font-weight: bold;
  background: #f5f5f5;
  min-width: 120px;
}

.timeline-block {
  position: absolute;
  top: 4px;
  height: 20px;
  background-color: #2196f3;
  color: #fff;
  font-size: 11px;
  padding: 2px 6px;
  border-radius: 4px;
  white-space: nowrap;
  overflow: hidden;
}
</style>
