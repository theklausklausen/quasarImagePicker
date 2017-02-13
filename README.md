# quasarImagePicker

> An image-upload component for vue.js

![An image-upload component for vue.js preview](https://raw.githubusercontent.com/mysoundsf/quasarImagePicker/preview.gif)

# Installation
    
    cordova plugin add cordova-plugin-camera

# Usage
That's a basic example of a component:

    <template>
      <div id="app">
        <quasarImagePicker destinationURL="https://www.yourDestinationURL.com" actionSheetHeader="Add a photo" galleryEntryText="Add photo from gallery" cameraEntryText="Add photo from gallery"></quasarImagePicker>
      </div>
    </template>

    <script>
    import { quasarImagePicker} from 'quasarImagePicker'

    export default {
      name: 'app',
      components: {
        quasarImagePicker
      }
    }
    </script>

# Custom destination URL

To set the destination of the upload, simply specify it on the component:

    <quasarImagePicker destinationURL="https://www.yourDestinationURL.com"></quasarImagePicker>

# Custom gallery entry text and camera entry text

To set the gallery entry text and camera entry text, simply specify it on the component:

    <quasarImagePicker galleryEntryText="Add photo from gallery" cameraEntryText="Add photo from gallery"></quasarImagePicker>

# Custom actionSheetHeader text

To set the actionSheetHeader text, simply specify it on the component:

    <quasarImagePicker cameraEntryText="Add photo from gallery"></quasarImagePicker>
