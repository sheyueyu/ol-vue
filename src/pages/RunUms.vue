<template>
    <div>
        <!-- <button @click="runBackendCode">Run Backend Code</button> -->
        <router-view>
        </router-view>
      </div>

  </template>
  
  <script>
  import axios from 'axios';
  export default {
    name: 'RunUms',
    data() {
      return {
        form: {
          email: '',
          name: '',
          food: null,
          checked: []
        },
        foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true
      }
    },

    methods: {
        runBackendCode() {
            axios.post('http://localhost:5000/run_test')
            .then(response => {
                console.log(response.data);  // 打印返回的数据
                // 在这里处理返回的数据...
            });
        },
        onSubmit(event) {
            event.preventDefault()
            alert(JSON.stringify(this.form))
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }

    },
    components: {
    }
}
</script>
  