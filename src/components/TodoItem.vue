<template>
  <div>
    <el-col :span="12">
      <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
        <el-row :gutter="12">
          <el-col :span="21">{{ todo }}</el-col>
          <el-col :span="3">
            <el-button @click="removeTodo" type="success" icon="el-icon-check" circle></el-button>
          </el-col>
        </el-row>
      </el-card>
    </el-col>
  </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: String, // Define la propiedad 'todo' como una cadena
    index: Number // Define la propiedad 'index' como un número
  },
  methods: {
    removeTodo() {
      // Emitir un evento para notificar al componente padre que se debe eliminar este elemento
      this.$emit('remove', this.index);
    }
  }
}
</script>
