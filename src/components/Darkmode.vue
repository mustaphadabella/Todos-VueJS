<template>
    <div class="parent-dark">
        <h3>Dark Mode</h3>
        <div class="container-dark">
            <input class="dark" type="checkbox" name="dark" id="dark" v-model="dark">
        </div>
    </div>
</template>
<script>
export default {
  data () {
    return {
      dark: false
    }
  },
  watch: {
    dark: function () {
      if (this.dark) {
        document.querySelector('html').style.filter = 'invert(1)'
      } else {
        document.querySelector('html').style.filter = 'invert(0)'
      }
    }
  }
}
</script>

<style scoped>
.container-dark{
    display: flex;
    flex-direction: column;
}
#dark{
width: 80px;
height: 40px;
border-radius: 40px;
appearance: initial;
background-color: #c4c4c475;
transition: background-color 0.6s linear;
outline: none;
margin-bottom: 20px;
position: relative;
}
#dark:checked{
    background-color: #00bcd4;
}
#dark::before{
    position: absolute;
    content: "OFF";
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: 700;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    top:2px;
    left: 2px;
    width:36px;
    height: 36px;
    border-radius: 40px;
    transition: left 0.4s linear;
    background-color: white;
}
#dark:checked::before{
    top:2px;
    left: 42px;
    content: "ON";
}
.parent-dark{
    display: flex;
    flex-direction: row;
    justify-content: center;
    margin-top: 15px;
}
</style>
