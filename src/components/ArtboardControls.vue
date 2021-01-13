<template>
  <div class="controls">

    <div class="controls-inner">
        
        <div class="form-group">
            <label for="color">
                <input type="color"
                       id="color"
                       v-model="color">
            </label>
        </div>

        <div class="form-group">
            <label for="draw">
                <input type="radio"
                       id="draw"
                       v-model="tool"
                       value="draw">
                Draw
            </label>
        </div>

        <div class="form-group">
            <label for="eraser">
                <input type="radio"
                       id="eraser"
                       v-model="tool"
                       value="eraser">
                Erase
            </label>
        </div>

        <div class="form-group">

        </div>

    </div>

  </div>
</template>

<style lang="scss" scoped>
.controls {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    height: 80%;
    background-color: #4a4a4a;
    border: 2px solid darken(#4a4a4a, 10%);
    border-radius: 2px;
}
.controls-inner {
    padding: 8px 10px;
    color: #f2f2f2;
}
.text-center {
    text-align: center;
}
.form-group {
    margin-bottom: 15px;
}
input {
    &[type=color] {
        display: block;
        width: 25px;
        height: 25px;
        padding: 0;
        margin: 0 auto;
        border: none;
    }
}
</style>

<script>
export default {
    data() {
        return {
            color: '#000',
            tool: 'draw',
        }
    },
}
</script>