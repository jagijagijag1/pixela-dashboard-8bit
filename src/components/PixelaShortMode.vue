<template>
  <div
    class="pixela column is-narrow">
    <div class="container with-title is-dark" style="width: 290px;">
      <label class="title">{{ graphName }}</label>

        <!--<p class="title is-5">{{ graphName }}</p>-->
        <div
          class="has-text-centered"
          style="margin-bottom: 15px;">
          <img
            v-if="loaded"
            :src="graphUrl + shortModeSuffix"
            width="220px" />
        </div>

        <div
          class="columns is-vcentered is-marginless is-centered"
          style="padding-bottom: 5px;">
          <div class="column is-narrow is-paddingless">
            <label
              class="label has-text-white is-marginless"
              style="width: 60px;">日付</label>
          </div>
          <div class="column is-narrow is-paddingless">
            <input 
              v-model="date" 
              type="text" 
              style="width: 100px;"
              class="input is-small">
          </div>
        </div>

        <div
          class="columns is-vcentered is-marginless is-centered"
          style="padding-bottom: 5px;">
          <div class="column is-narrow is-paddingless">
            <label
              class="label has-text-white is-marginless"
              style="width: 60px;">値</label>
          </div>
          <div class="column is-narrow is-paddingless">
            <input 
              v-model="quantity" 
              type="text" 
              style="width: 100px;"
              class="input is-small">
          </div>
        </div>

        <div class="columns is-centered is-marginless">
          <div class="column is-narrow">
              <button 
                :class="{ 'is-loading': isRecording }"
                class="btn is-primary is-small" 
                @click="recordData">記録</button>
          </div>
          <div class="column is-narrow ">
              <button 
                :class="{ 'is-loading': isUpdating }"
                class="btn is-success is-small" 
                @click="updateData">更新</button>
          </div>
        </div>
        <p 
          v-if="isSuccess"
          class="help is-success is-center">{{ msg }}</p>
        <p 
          v-if="!isSuccess" 
          class="help is-danger is-center">{{ msg }}</p>

      </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    user: {
      type: String,
      required: true
    },
    token: {
      type: String,
      required: true
    },
    graphName: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      loaded: true,
      graphUrl: '',
      date: '', 
      quantity: '0',
      msg: '',
      isSuccess: null,
      isRecording: false,
      isUpdating: false,
      shortModeSuffix: '?mode=short'
    }
  },
  created() {
    // set graph URL
    this.graphUrl = "https://pixe.la/v1/users/" + this.user + "/graphs/" + this.graphName

    // set today's date string
    var dateObj = new Date()
    var yyyy = dateObj.getFullYear()
    var MM = ('0' + (dateObj.getMonth() + 1)).slice(-2)
    var dd = ('0' + dateObj.getDate()).slice(-2)
    this.date = yyyy + MM + dd
  },
  methods: {
    recordData() {
      // set target URL
      var postUrl = this.graphUrl
      this.initForm('record')

      if (this.token === '' || this.date === '' | this.quantity === '') {
        this.updateForm('record', 'fail')
        this.msg = 'fill all input (USER-TOKEN, date, quantity)'
        return
      }

      // POST this.date & this. quantity to this.graphName
      axios({
        method: 'post',
        url: postUrl,
        headers: { 'X-USER-TOKEN': this.token },
        data: { date: this.date, quantity: this.quantity }
      })
      .then(response => {
        console.log("record " + response.data.message);
        this.updateForm('record', 'success')
      }).catch(error => {
        console.log(error);
        this.updateForm('record', 'fail')
      });
    },
    updateData() {
      // set target URL
      var putUrl = this.graphUrl + '/' + this.date
      this.initForm('update')

      if (this.token === '' || this.date === '' | this.quantity === '') {
        this.updateForm('record', 'fail')
        this.msg = 'fill all input (USER-TOKEN, date, quantity)'
        return
      }

      // PUT (update) quantity of this.date to this. quantity on this.graphName
      axios({
        method: 'put',
        url: putUrl,
        headers: { 'X-USER-TOKEN': this.token },
        data: { quantity: this.quantity }
      })
      .then(response => {
        console.log("update " + response.data.message);
        this.updateForm('update', 'success')
      }).catch(error => {
        console.log(error);
        this.updateForm('update', 'fail')
      });
    },
    initForm(action) {
      // initialize record/update-related variables
      this.loaded = false
      this.msg = ''
      if (action === 'record') {
        this.isRecording = true
      } else if (action === 'update') {
        this.isUpdating = true
      }
    },
    updateForm(action, isSuccess) {
      // update record/update-related variables
      this.loaded = true
      this.isRecording = false
      this.isUpdating = false

      if (isSuccess === 'success') {
        // if action sucessed
        this.isSuccess = true

        if (action === 'record') {
          this.msg = 'Successfully recorded'
        } else if (action === 'update') {
          this.msg = 'Successfully updated'
        }
      } else {
        // if action failed
        this.isSuccess = false
        this.msg = 'Error: something wrong'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
// NES.css
@import "../../node_modules/nes.css/scss/nes.scss";
</style>
