~~~js
<el-upload ref="uploadInstance">
</el-upload>

import { ref } from 'vue'

const uploadInstance = ref()
const manualUploadFile = () => {
  uploadInstance.value.$el.querySelector('input').click()
}
~~~
