<template>
  <div id="app">
    <h1>ISE! 2019 Certificate</h1>
    <div class="tab">
      <button class="tab-links" @click="mode = 'BIONIX'">
        BIONIX
      </button>
      <button class="tab-links" @click="mode = 'ICON'">
        ICON
      </button>
    </div>
    <div class="content">
      <h2>{{mode}}</h2>
      <label for="email">Masukkan email</label>
      <input type="text" v-model.lazy="email" id="email" placeholder="johndoe@gmail.com">
      <table class="list" v-if="certList.length > 0">
        <tr v-for="data in certList" :key="data.no_surat">
          <td>{{data.email}}</td>
          <td>{{data.nama}}</td>
          <td><button @click="downloadCert(data.nama, data.no_surat.toString(), mode)">Download</button></td>
        </tr>
      </table>
      <p v-if="certList.length === 0 && email">Sertifikat {{mode}} dengan email {{email}} tidak dapat ditemukan.</p>
    </div>
  </div>
</template>

<script>
import CertBionix from '@/data/bionix.json'
import CertIcon from '@/data/icon.json'
import {bgCertBionix, bgCertIcon} from '@/data/imgData.js'
import jsPDF from 'jspdf'

export default {
  data() {
    return {
      mode: "BIONIX",
      email: ""
    }
  },
  computed: {
    certList: function () {
      if (this.mode === 'BIONIX')
        return CertBionix.filter(data => data.email === this.email)
      else
        return CertIcon.filter(data => data.email === this.email)
    }
  },
  methods: {
    downloadCert: function (nama, no_surat, mode) {
      let doc = new jsPDF({
        orientation: 'landscape'
      })
      let width = doc.internal.pageSize.getWidth()
      let height = doc.internal.pageSize.getHeight()
      let imgData = mode === "BIONIX" ? bgCertBionix : bgCertIcon
      console.log(imgData)
      doc.addImage(imgData, 'JPEG', 0, 0, width, height)
      doc.setFontSize(25)
      doc.text(140, 60, nama)
      doc.setFontSize(15)
      doc.text(220, 38, no_surat +'/BSO/HMSI/18/X/2019')
      doc.save(`CERT${mode}${no_surat}.pdf`)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.list {
  margin: auto;
}
.list td {
  padding: 10px 20px;
}
</style>
