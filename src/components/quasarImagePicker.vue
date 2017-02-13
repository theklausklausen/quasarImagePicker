<template>
  <div class="border">
      <i @click="showActionSheetWithIcons">add_a_photo</i>
      <q-modal v-if="this.isActive" ref="minimizedModal" class="minimized" :content-css="{padding: '50px'}">
      <h4>{{this.modalHeader}}</h4>
      <i>{{this.activityIcon}}</i>
      <p>{{this.modalBody}}</p>
      <button class="red" @click="$refs.minimizedModal.close()">Ok</button>
    </q-modal>
  </div>
</template>

<script>
import { ActionSheet } from 'quasar'
export default {

  data: function() {
    return {
      isActive: false,
      modalHeader: 'Bild Upload',
      modalBody: 'Bild wird hochgeladen...',
      activityIcon: null
    }
  },
  props: ['destinationURL', 'actionSheetHeader', 'galleryEntryText', 'cameraEntryText'],
  methods: {
    send(baseString) {
      this.$http.post(this.destinationURL, {image: baseString}).then(response => {
        this.isActiveLoading = true
        this.activityIcon = 'file_upload'
        if (response.status === 200) {
          this.modalBody = 'Ein Entwickler wird das bild überprüfen.'
          this.activityIcon = 'done'
        }
        else {
          this.modalHeader = 'Bild Upload Fehler'
          this.modalBody = 'UUPS, da ist wohl etwas schiefgelaufen..\nVersuche es einfach noch einmal!'
          this.activityIcon = 'error'
        };
        response.statusText
        response.headers.get('Expires')
        this.someData = response.body
      }, response => {
      })
    },
    openCamera() {
      let srcType = window.Camera.PictureSourceType.CAMERA
      let options = this.setOptions(srcType)
      navigator.camera.getPicture((imageData) => {
        this.send(imageData)
      }, (error) => {
        console.debug('Unable to obtain picture: ' + error, 'app')
      }, options)
    },
    setOptions(srcType) {
      var options = {
        quality: 100,
        destinationType: window.Camera.DestinationType.DATA_URL,
        sourceType: srcType,
        encodingType: window.Camera.EncodingType.JPEG,
        mediaType: window.Camera.MediaType.PICTURE,
        allowEdit: false,
        correctOrientation: false,
        targetWidth: 150,
        targetHight: 150
      }
      return options
    },
    createNewFileEntry(imgUri) {
      window.resolveLocalFileSystemURL(cordova.file.cacheDirectory, function success(dirEntry) {
        dirEntry.getFile('tempFile.jpeg', { create: true, exclusive: false }, function (fileEntry) {
          console.log('got file: ' + fileEntry.fullPath)
        }, function() {
          console.log('Error Create File')
        })
      }, function() {
        console.log('ErrorResolveUrl')
      })
    },
    openFilePicker(selection) {
      let srcType = window.Camera.PictureSourceType.SAVEDPHOTOALBUM
      let options = this.setOptions(srcType)
      navigator.camera.getPicture((imageData) => {
        this.send(imageData)
      }, function cameraError(error) {
        console.debug('Unable to obtain picture: ' + error, 'app')
      }, options)
    },
    showActionSheetWithIcons(/* gallery */) {
      let that = this
      ActionSheet.create({
        title: this.actionSheetHeader,
        // gallery: gallery,
        actions: [
          {
            label: this.galleryEntryText,
            icon: 'photo_album',
            handler () {
              that.openFilePicker()
            }
          },
          {
            label: this.cameraEntryText,
            icon: 'camera_alt',
            handler () {
              that.openCamera()
            }
          }
        ]
      })
    }
  }
}
</script>

<style>
</style>
