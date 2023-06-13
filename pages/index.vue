<template>
  <div>
    value: {{ refVal }}
    <button @click="clickPlus">Plus</button>
  </div>
</template>
<script>
export default {
  name: "Index",
  setup() {
    let refVal = ref(0);
    let val1 = ref(1);
    let val2 = ref(1);
    refVal.value = val1.value + val2.value;
    const clickPlus = () => {
      val1.value = 2;
    };
    return {
      refVal,
      clickPlus,
    };
  },
};
</script>
<style></style>
