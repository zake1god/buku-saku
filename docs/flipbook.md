# 📖 Buku Saku Interaktif (HD)

<div id="flipbook-container">
    <div id="flipbook"></div>
</div>

<script>
document.addEventListener("DOMContentLoaded", async function () {

    const pdfUrl = "/assets/buku-saku.pdf";

    const pdf = await pdfjsLib.getDocument(pdfUrl).promise;
    const pages = [];

    for (let i = 1; i <= pdf.numPages; i++) {
        const page = await pdf.getPage(i);
        const viewport = page.getViewport({ scale: 2 }); // HD scale

        const canvas = document.createElement("canvas");
        const context = canvas.getContext("2d");

        canvas.width = viewport.width;
        canvas.height = viewport.height;

        await page.render({
            canvasContext: context,
            viewport: viewport
        }).promise;

        pages.push(canvas);
    }

    const pageFlip = new St.PageFlip(
        document.getElementById("flipbook"),
        {
            width: 600,
            height: 800,
            size: "stretch",
            showCover: true,
            maxShadowOpacity: 0.5,
            mobileScrollSupport: false,
        }
    );

    pageFlip.loadFromHTML(pages);

});
</script>
