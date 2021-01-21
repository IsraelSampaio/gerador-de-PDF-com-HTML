<template>
  <main class="report" @click="canvas">
    <div class="report-view report-view--hero">
      <div class="report-view__wrapper-logo" />
    </div>
    <div class="report-view">
      <div class="report-view__wrapper-logo">
        <img src="@/assets/logo.png" class="report-view__logo" />
      </div>
    </div>
  </main>
</template>

<script>
import { jsPDF } from "jspdf";

import { toPng } from "html-to-image";

export default {
  name: "App",
  methods: {
    async canvas() {
      const filename = "ThisIsYourPDFFilename.pdf";

      const listViewElement = this.$el.querySelectorAll(".report-view");

      let pdf = new jsPDF("l", "mm", "a4", true);

      for (let viewEntry of Object.entries(listViewElement)) {
        const [key, view] = viewEntry;

        const dataUrl = await toPng(view);

        if (key > 0) pdf.addPage("a4", "l");

        pdf.addImage(dataUrl, "PNG", 0, 0, 298, 211);
      }

      pdf.save(filename);

      console.log(pdf.output("datauristring", filename));
    }
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 24px;
  display: flex;
  justify-content: center;
  background: #8a8a8a;
}

@page {
  margin: 0cm;
  size: landscape;
}

p {
  widows: 3;
  orphans: 3;
}

.report {
  height: 21cm;
  width: 29.7cm;

  &-view {
    height: 100%;
    width: 100%;
    background: #fff;
    margin-bottom: 24px;

    &__wrapper-logo {
      width: 200px;
      background: rgba(248, 185, 72, 1);
      height: 100%;

      display: flex;
      justify-content: center;
      align-items: flex-start;
    }

    &__logo {
      width: 150px;
      margin-top: 16px;
    }

    page-break-after: always;
  }
}

@media print {
  body {
    margin: 0;
    padding: 0;
    display: block;
    background: #fff;
  }

  .report {
    height: 100vh;
    width: 100vw;

    &-view {
      margin-bottom: 0;

      width: 100vw;
      height: 100vh;
    }
  }
}
</style>
