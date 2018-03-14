<template>
  <chart v-if="showLine" :data="lineData" :options="options" height="128px" ></chart>
</template>

<script>
  import chart from '~/components/chart'
  export default {
    components:
      {
        chart
      },
    data () {
      return {
        showLine: false
      }
    },
    mounted () {
      this.showLine = true // showLine will only be set to true on the client. This keeps the DOM-tree in sync.
    },
    asyncData () {
      const lineData = {
        labels: ['10', '20', '30', '40', '50','60','70','80','90','100'],
        datasets: [{
          tension: 0,
          label: 'Asistencia',
          backgroundColor: '#f87979',
          data: [0, 1, 1,0]
        }, {
          tension: 0,
          label: 'Examen',
          backgroundColor: '#127979',
          data: [0, 0, 0,1,0]
        }
        ]
      }

      const options = {}
      return { lineData, options }
    }
  }
</script>
