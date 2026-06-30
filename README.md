<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Laporan Kerja IT SIMRS</title>

<style>
body {
  font-family: 'Segoe UI', Arial, sans-serif;
  background: linear-gradient(135deg, #eaf1f8, #f6f9fc);
  padding: 15px;
}

/* Card utama */
.container {
  max-width: 480px;
  margin: auto;
  background: white;
  padding: 20px;
  border-radius: 14px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.08);
}

/* Header + logo */
.header {
  text-align: center;
  margin-bottom: 20px;
}

.header img {
  width: 120px;
  margin-bottom: 8px;
}

.header h2 {
  margin: 5px 0;
  font-size: 22px;
  color: #2c3e50;
}

.header p {
  margin: 0;
  font-size: 13px;
  color: #6c757d;
}

/* Form */
label {
  font-weight: 600;
  margin-top: 14px;
  display: block;
  color: #34495e;
}

input, textarea, select {
  width: 100%;
  padding: 12px;
  margin-top: 6px;
  border-radius: 10px;
  border: 1px solid #dfe6e9;
  font-size: 14px;
  background: #fafbfc;
}

textarea {
  resize: vertical;
}

/* Tombol */
button {
  width: 100%;
  padding: 14px;
  border-radius: 12px;
  border: none;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  margin-top: 12px;
}

/* Warna tombol */
button:not(.secondary):not(.print) {
  background: linear-gradient(135deg, #2ecc71, #27ae60);
  color: white;
}

button.secondary {
  background: linear-gradient(135deg, #3498db, #2980b9);
  color: white;
}

button.print {
  background: linear-gradient(135deg, #9b59b6, #8e44ad);
  color: white;
}

/* Rekap button */
.rekap {
  display: flex;
  gap: 10px;
}

.rekap button {
  flex: 1;
}

/* Divider */
hr {
  border: none;
  border-top: 1px solid #ecf0f1;
  margin: 20px 0;
}
</style>
</style>
</head>

<body>
<main>
<div class="container">

    <div class="header">
        <img src="https://i.ibb.co.com/9kXbzMyb/Chat-GPT-Image-Jun-29-2026-02-37-41-PM.png" alt="Logo SIMRS">
        <h2>Laporan Kerja IT SIMRS</h2>
    </div>

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
  window.open(SCRIPT_URL + "https://docs.google.com/spreadsheets/d/1R3o6cmbhg0iYAvP9a5UokS_QaOFhIXRoPuRTniF1xNU/edit?gid=0#gid=0", "_blank");
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
</html>
