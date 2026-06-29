<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Laporan Kerja IT SIMRS</title>

<style>
body {
  font-family: Arial, sans-serif;
  background: #f4f6f8;
  padding: 10px;
}
.container {
  max-width: 480px;
  margin: auto;
  background: white;
  padding: 15px;
  border-radius: 8px;
}
h2 {
  text-align: center;
}
label {
  font-weight: bold;
  margin-top: 10px;
  display: block;
}
input, textarea, select, button {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
}
button {
  background: #27ae60;
  color: white;
  border: none;
  margin-top: 10px;
  font-size: 15px;
}
button.secondary {
  background: #2980b9;
}
button.print {
  background: #8e44ad;
}
hr {
  margin: 15px 0;
}
</style>
</head>

<body>
<div class="container">
<h2>Laporan Kerja IT SIMRS</h2>

<label>Lokasi / Ruangan</label>
<input type="text" id="lokasi" required>

<label>Nama Perangkat</label>
<input type="text" id="perangkat" required>

<label>Permasalahan</label>
<textarea id="permasalahan" required></textarea>

<label>Tindakan</label>
<textarea id="tindakan" required></textarea>

<label>Status</label>
<select id="status">
  <option>Selesai</option>
  <option>Proses</option>
  <option>Pending</option>
</select>

<label>Upload Foto</label>
<input type="file" id="foto" accept="image/*" capture="environment" required>

<button onclick="kirimData()">📤 Kirim Laporan</button>

<button id="btnPdf" class="secondary" style="display:none" onclick="openPDF()">
  📄 Lihat PDF
</button>

<button id="btnPrint" class="print" style="display:none" onclick="printPDF()">
  🖨️ Cetak PDF
</button>

<hr>

<button class="secondary" onclick="openRekapHarian()">📅 Rekap Harian</button>
<button class="secondary" onclick="openRekapBulanan()">📊 Rekap Bulanan</button>

</div>

<script>
let pdfLink = "";

// 🔴 GANTI DENGAN URL WEB APP ANDA
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzWxFYX-M5P0vULmRQSVgGdUZW6yV8BOhvvZDAHf1HY_pIyeD3XOp8S924koShPnidITA/exec";

function kirimData() {
  const file = document.getElementById("foto").files[0];
  if (!file) {
    alert("Foto wajib diupload");
    return;
  }

  const reader = new FileReader();
  reader.onloadend = function () {
    const data = {
      lokasi: lokasi.value,
      perangkat: perangkat.value,
      permasalahan: permasalahan.value,
      tindakan: tindakan.value,
      status: status.value,
      foto: reader.result.split(",")[1],
      tipeFoto: file.type
    };

    fetch(SCRIPT_URL, {
      method: "POST",
      body: JSON.stringify(data)
    })
    .then(res => res.json())
    .then(res => {
      if (res.status === "success") {
        pdfLink = res.pdf;

        document.getElementById("btnPdf").style.display = "block";
        document.getElementById("btnPrint").style.display = "block";

        alert("✅ Laporan berhasil dikirim");
        resetForm();
      } else {
        alert("❌ Gagal mengirim laporan");
      }
    })
    .catch(() => alert("❌ Koneksi ke server gagal"));
  };
  reader.readAsDataURL(file);
}

function openPDF() {
  if (!pdfLink) {
    alert("PDF belum tersedia");
    return;
  }
  window.open(pdfLink, "_blank");
}

function printPDF() {
  if (!pdfLink) {
    alert("PDF belum tersedia");
    return;
  }
  // Cetak HARUS dari PDF Viewer
  window.open(pdfLink + "#toolbar=1", "_blank");
}

function openRekapHarian() {
  window.open(SCRIPT_URL + "?rekap=harian", "_blank");
}

function openRekapBulanan() {
  window.open(SCRIPT_URL + "?rekap=bulanan", "_blank");
}

function resetForm() {
  lokasi.value = "";
  perangkat.value = "";
  permasalahan.value = "";
  tindakan.value = "";
  status.value = "Selesai";
  foto.value = "";
}
</script>
</body>
</html>s
