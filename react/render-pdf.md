# Handle a PDF in React

```javascript
const handleDownloadPdf = async () => {
  if (divRef.current) {
    const canvas = await html2canvas(divRef.current, { useCORS: true });
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF("p", "mm", "legal");
    const imgProps = pdf.getImageProperties(imgData);
    const pdfWidth = pdf.internal.pageSize.getWidth();
    const imgHeight = (canvas.height * pdfWidth) / canvas.width;
    const pdfHeight = imgProps.height < imgHeight ? imgProps.height : imgHeight;
    pdf.addImage(imgData, "PNG", 0, 0, pdfWidth, pdfHeight);
    pdf.save(`Invoice_${invoice.invoiceNumber}.pdf`);
  }
};
```

[source](https://x.com/aaronbesson/status/1850968524119793809)
