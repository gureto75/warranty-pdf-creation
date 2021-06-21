<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="modal" :class="{ sale: theme === 'sale' }">
      <div class="actions">
        <slot name="links"></slot>
      </div>
      <h1>{{ modalTitle }}</h1>
      <p>{{ modalContent }}</p>
      <slot>...</slot>
      <div>
        <button class="download" @click="createPdfMake">Save PDF (acode)</button>
      </div>
    </div>
  </div>
</template>

<script>
  import pdfMake from 'pdfmake/build/pdfmake'
  import pdfFonts from '../assets/vfs_fonts.js'
  pdfMake.vfs = pdfFonts.pdfMake.vfs
  export default {
    name: 'Modal',
    props: [
      'model',
      'brand',
      'color',
      'serial',
      'clientName',
      'modalTitle',
      'modalContent',
      'supplier',
      'theme',
    ],
    computed: {
      pdfName() {
        return `${this.clientName}_Warranty`
      },
    },
    methods: {
      closeModal() {
        this.$emit('close')
      },
      createPdfMake() {
        const date = new Date()
        const expiration = new Date()
        expiration.setDate(date.getDate() + 183)
        const buyDate = new Intl.DateTimeFormat('el-GR', {
          dateStyle: 'short',
        }).format(date)
        const expDate = new Intl.DateTimeFormat('el-GR', {
          dateStyle: 'short',
        }).format(expiration)
        const dd = {
          info: {
            title: `${this.pdfName}_ends_in_${expDate}`,
            author: 'atmosfaira',
            subject: 'Warranty',
            keywords: 'Warranty',
          },
          styles: {
            header: {
              fontSize: 18,
              bold: true,
              decoration: 'underline',
            },
            tableRegular: {
              margin: [0, 20, 0, 0],
            },
            tableHeader: {
              bold: true,
              fontSize: 13,
              color: 'black',
            },
          },
          content: [
            {
              image: 'atmosfaira-logo.jpg',
              width: 150,
            },
            {
              text: 'ΕΓΓΥΗΣΗ ΚΑΛΗΣ ΛΕΙΤΟΥΡΓΙΑΣ ΣΥΣΚΕΥΗΣ',
              margin: [0, 10],
              style: 'header',
              alignment: 'center',
            },
            {
              columns: [
                { width: '*', text: '' },
                {
                  width: 'auto',
                  style: 'tableRegular',
                  table: {
                    headerRows: 1,
                    // dontBreakRows: true,
                    // keepWithHeaderRows: 1,
                    body: [
                      [
                        { text: 'ΣΥΣΚΕΥΗ', style: 'tableHeader' },
                        { text: 'ΧΡΩΜΑ', style: 'tableHeader' },
                        { text: '#Serial', style: 'tableHeader' },
                      ],
                      [
                        `${this.brand} ${this.model}`,
                        this.color,
                        `${this.serial}-${this.supplier}`,
                      ],
                    ],
                  },
                },
                ,
                { width: 'auto', text: '' },
              ],
            },
            {
              columns: [
                { width: '*', text: '' },
                {
                  width: 'auto',
                  style: 'tableRegular',
                  table: {
                    headerRows: 1,
                    // dontBreakRows: true,
                    // keepWithHeaderRows: 1,
                    body: [
                      [
                        { text: 'ΗΜ/ΝΙΑ ΑΓΟΡΑΣ', style: 'tableHeader' },
                        {
                          text: 'ΙΣΧΥΣ ΕΓΓΥΗΣΗΣ ΕΩΣ',
                          style: 'tableHeader',
                        },
                      ],
                      [
                        { text: buyDate, alignment: 'center' },
                        { text: expDate, alignment: 'center' },
                      ],
                    ],
                  },
                },
                ,
                { width: 'auto', text: '' },
              ],
            },
            {
              image: 'signature.png',
              width: 240,
              alignment: 'right',
              margin: [0, 20, 15, 0],
            },
          ],
          defaultStyle: { font: 'Jura' },
        }

        pdfMake.fonts = {
          Jura: {
            normal: 'Jura-Regular.ttf',
            bold: 'Jura-Bold.ttf',
          },
        }

        pdfMake.createPdf(dd).download(`${this.pdfName}_ends_in_${expDate}`)
        pdfMake.createPdf(dd).open()
        // pdfMake.createPdf(dd).print()
      },
    },
  }
</script>

<style>
  .modal {
    width: 400px;
    padding: 20px;
    margin: 100px auto;
    background: white;
    border-radius: 10px;
  }

  .backdrop {
    top: 0;
    position: fixed;
    background: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 100%;
  }
  .modal h1 {
    color: #03cfb4;
  }

  .modal .actions {
    text-align: center;
    margin: 30px 0 10px 0;
  }

  .modal .actions a,
  .download {
    color: #333;
    padding: 8px;
    border: 1px solid #eee;
    border-radius: 4px;
    text-decoration: none;
    margin: 10px;
  }

  .modal.sale {
    background: crimson;
    color: white;
  }

  .modal.sale h1 {
    color: white;
  }

  .modal.sale .actions {
    color: white;
  }

  .modal.sale .actions a {
    color: white;
  }
</style>
