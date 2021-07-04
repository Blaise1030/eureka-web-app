<template> 
    <div>
        This is the group forming page
    </div>
</template>

<script>
import {onMounted} from "vue";
export default {
  name: "Incubator",  
  setup() {
    onMounted(() => {
      console.log("This is the group forming page")
    });
  },
};
</script>

<style lang="scss" scoped></style>
