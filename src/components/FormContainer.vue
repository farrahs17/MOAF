<template>
  <div class="form">
    <b-field label="Name">
        <b-input v-model="formData.name"></b-input>
    </b-field>

    <div class="intents" v-for="(intent,k) in formData.intents" :key="k">
      <b-field label="Action">
            <b-select placeholder="Action" v-model="intent.action">
                <option value="set_state">set_state</option>
                <option value="set_entity">set_entity</option>
                <option value="say">say</option>
                <option value="goto_intent">goto_intent</option>
            </b-select>    
      </b-field>
      <b-field label="Args">
        <b-input v-model="intent.arg"></b-input>
      </b-field>
      <div>

        <b-button size="is-small" rounded type="is-primary" @click="add(intent.action, intent.arg)" v-show="k == formData.intents.length-1">
          <b-icon pack="fas" icon="plus"></b-icon>
        </b-button>
        <b-button size="is-small" rounded type="is-danger" @click="remove(k)" v-show="k || ( !k && formData.intents.length > 1)">
          <b-icon pack="fas" icon="minus"></b-icon>
        </b-button>
      </div>
      
    </div>

    <div class="patterns">
      <b-field label="Patterns">
        <b-input maxlength="200" type="textarea"></b-input>
      </b-field>

    </div>

    <b-button type="is-primary" @click.prevent="submit()">Submit</b-button>

    
  </div>
</template>

<script>
export default {
  name: 'FormContainer',
  props: {
    msg: String
  },
  data () {
    return {
      formData: {
        name: '',
        intents: [{
          action: '',
          arg: ''
        }],
        patterns: ''
      }
    }
  },
  mounted() {
    console.log(this.formData);
  },
  methods: {
    add(action, arg) {
      this.formData.intents.push({
        action, arg
      })
    },
    remove (index) {
      this.formData.intents.splice(index, 1)
    },
    submit() {
      const data =JSON.parse(JSON.stringify(this.formData))
      console.log(data)
      this.saveToFile(data, 'data')

    },
    saveToFile(jsonData, filename) {
      const data = JSON.stringify(jsonData)
      let blob = new Blob([data], { type: 'text/plain;charset=utf-8;' })
      if (navigator.msSaveBlob) { // IE 10+
        navigator.msSaveBlob(blob, filename)
      } else {
        let link = document.createElement('a')
        if (link.download !== undefined) { // feature detection
          // Browsers that support HTML5 download attribute
          let url = URL.createObjectURL(blob)
          link.setAttribute('href', url)
          link.setAttribute('download', filename)
          link.style.visibility = 'hidden'
          document.body.appendChild(link)
          link.click()
          document.body.removeChild(link)
        }
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  .form {
    width: 80%;
    margin: 0 auto;
  }
  h3 {
    margin: 40px 0 0;
  }

  .intents {
    display: flex;
    align-items: center;
    justify-content: space-between;

    button {
      margin-top: 20px;
    }
    .field {
      width: 40%;
      margin: 0;
    }
  }
  

  .button .is-rounded {
    padding: 0;
  }
</style>
