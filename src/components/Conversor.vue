<template>
  <div class="section">
    <div class="column">
      <h1 class="title is-1">Conversor archivo CSV</h1>
    </div>

    <div class="columns container">
      <div class="file has-name is-boxed is-centered"
           v-bind:class="{'is-info': !loadedFile, 'is-primary': loadedFile}">
        <label class="file-label">
          <input class="file-input" type="file"
                 id="first"
                 @change="uploadFirst">
          <span class="file-cta">
            <span class="file-icon">
              <i class="fas fa-upload"></i>
            </span>
            <span class="file-label">
              Select 1st language file...
            </span>
          </span>
          <span class="file-name" id="file-first" v-show="loadedFile"></span>
        </label>
      </div>
    </div>

    <div class="columns container">
      <div class="file has-name is-boxed is-centered"
           v-bind:class="{'is-warning': !loadedFile, 'is-primary': loadedFile}">
        <label class="file-label">
          <input class="file-input" type="file"
                 id="second"
                 @change="uploadSecond">
          <span class="file-cta">
            <span class="file-icon">
              <i class="fas fa-upload"></i>
            </span>
            <span class="file-label">
              Select 2nd language file...
            </span>
          </span>
          <span class="file-name" id="file-second" v-show="loadedFile"></span>
        </label>
      </div>
    </div>

    <div class="column is-half">
      <div class="control">
      <textarea
        class="textarea"
        v-bind:class="{'is-warning': isEditable}"
        rows="17"
        readonly
        placeholder="FIRST LANGUAGE IS THE MAIN LANGUAGE - E.G. EN.JSON"
        v-model='secondFile'>
      </textarea>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'conversor',
    data () {
      return {
        firstFile: null,
        secondFile: null,
        loadedFile: false,
        isEditable: false
      }
    },
    methods: {
      uploadFirst (e) {
        const fileToLoad = event.target.files[0]
        const reader = new FileReader()
        document.getElementById('file-first').innerHTML = event.target.files[0].name ? event.target.files[0].name : 'Select 1st language file'
        reader.onload = fileLoadedEvent => {
          this.firstFile = JSON.parse(fileLoadedEvent.target.result)
        }
        reader.readAsText(fileToLoad)
      },
      uploadSecond (e) {
        const fileToLoad = event.target.files[0]
        const reader = new FileReader()
        document.getElementById('file-second').innerHTML = event.target.files[0].name ? event.target.files[0].name : 'Select 2nd language file'
        this.loadedFile = true
        reader.onload = fileLoadedEvent => {
          let data = JSON.parse(fileLoadedEvent.target.result)
          Object.keys(data).forEach((secondFileKey) => {
            Object.keys(this.firstFile).forEach((firstFileKey) => {
              if (secondFileKey === firstFileKey && this.firstFile[firstFileKey] === data[secondFileKey]) {
                delete data[secondFileKey]
              }
            })
          })
          this.secondFile = JSON.stringify(data)
        }
        reader.readAsText(fileToLoad)
      }
    }
  }
</script>


