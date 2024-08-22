<template>
  <div>
    <h2 class="text-lg font-bold mb-4">Add Member</h2>
    <form @submit.prevent="addMember">
      <input v-model="name" placeholder="Name" class="input-field" />
      <input v-model="age" type="number" placeholder="Age" class="input-field" />
      <select v-model="fitnessLevel" class="input-field">
        <option>Beginner</option>
        <option>Intermediate</option>
        <option>Advanced</option>
      </select>
      <select v-model="selectedGroup" class="input-field">
        <option v-for="group in groups" :key="group.id" :value="group.id">{{ group.name }}</option>
      </select>
      <button type="submit" class="btn-primary">Add Member</button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    groups: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      name: '',
      age: null,
      fitnessLevel: 'Beginner',
      selectedGroup: null
    }
  },
  methods: {
    addMember() {
      if (this.name && this.age && this.selectedGroup !== null) {
        this.$emit('memberAdded', {
          name: this.name,
          age: this.age,
          fitnessLevel: this.fitnessLevel,
          groupId: this.selectedGroup
        })
        this.name = ''
        this.age = null
        this.fitnessLevel = 'Beginner'
        this.selectedGroup = null
      } else {
        alert('Please fill in all fields')
      }
    }
  }
}
</script>

<style scoped>
.input-field {
  display: block;
  width: 100%;
  padding: 8px;
  margin-bottom: 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
}
.btn-primary {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
}
</style>
