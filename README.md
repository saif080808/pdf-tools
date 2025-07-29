<script src="https://unpkg.com/pdf-lib"></script>
<input type="file" id="f1" multiple>
<button onclick="merge()">Merge</button>
<script>
async function merge(){
  const pdfDoc = await PDFLib.PDFDocument.create();
  const files = document.getElementById('f1').files;
  for(let f of files){
    const data = await f.arrayBuffer();
    const donor = await PDFLib.PDFDocument.load(data);
    const pages = await pdfDoc.copyPages(donor, donor.getPageIndices());
    pages.forEach(p => pdfDoc.addPage(p));
  }
  const bytes = await pdfDoc.save();
  const blob = new Blob([bytes], {type: 'application/pdf'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'merged.pdf';
  a.textContent = 'Download merged PDF';
  document.body.appendChild(a);
}
</script>
