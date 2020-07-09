<template>
  <div class="container mx-auto px-4">
    <h1 class="text-center text-3xl font-extrabold py-5">ISE! 2019 Certificate</h1>
    <div class="flex justify-center">
      <button class="px-6 py-3 mx-1 font-bold rounded-full" :class="mode === 'BIONIX' ? 'active' : null"  @click="mode = 'BIONIX'">
        BIONIX
      </button>
      <button class="px-6 py-3 mx-1 font-bold rounded-full" :class="mode === 'ICON' ? 'active' : null" @click="mode = 'ICON'">
        ICON
      </button>
    </div>
    <div class="container mx-auto max-w-md mt-10 bg-gray-200 rounded-lg">
      <h2 class="text-center text-5xl font-extrabold tracking-widest relative" style="top: -40px">{{mode}}</h2>
      <div class="p-10 pt-0">
      <div class="pb-5">
        <label for="email" class="block text-xs p-1">Masukkan email</label>
        <input type="text" class="block px-5 py-3 rounded-lg text-sm text-gray-800 max-w-full" v-model.lazy="email" id="email" placeholder="johndoe@gmail.com">
      </div>
      <table class="text-sm w-full" v-if="certList.length > 0">
        <tr v-for="data in certList" :key="data.no_surat">
          <td class="py-3 pr-3">{{data.nama}}</td>
          <td class="py-3 pl-3"><button class="text-xs px-4 py-2 bg-white rounded" @click="downloadCert(data.nama, data.no_surat.toString(), mode)">Download</button></td>
        </tr>
      </table>
      <p v-if="certList.length === 0 && email">Sertifikat {{mode}} dengan email <span class="text-xs p-1 bg-yellow-100 font-mono font-semibold rounded">{{email}}</span> tidak dapat ditemukan.</p>
      </div>
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
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700;800&display=swap');

html, body {
  font-family: 'Montserrat';
  font-size: 20px;
  color: #2c3e50;
}

input, button {
  font-family: 'Montserrat';
}

input:focus, button:active {
  outline: none;
}

.active {
  background-color: #63b3ed;
  color: #ffffff;
}

@media (max-width: 600px) {
  html, body {
    font-size: 18px;
  }
}

@media (max-width: 400px) {
  html, body {
    font-size: 16px;
  }
}
</style>
