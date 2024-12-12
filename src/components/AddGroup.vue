<template>
  <div>
    <h2 class="text-lg font-bold mb-4">Add Group</h2>
    <form @submit.prevent="addGroup">
      <input v-model="groupName" placeholder="Group Name" class="input-field" />
      <button type="submit" class="btn-primary">Create Group</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      groupName: ''
    }
  },
  methods: {
    addGroup() {
      if (this.groupName.trim() !== '') {
        const newGroup = {
          id: Date.now(),
          name: this.groupName,
          members: []
        }
        this.$emit('groupAdded', newGroup)
        this.groupName = ''
      } else {
        alert('Please enter a group name')
      }
    }
  }
}
</script>

<style scoped>
.input-field {
  display: block;
  width: 100%;
  padding: 12px;
  margin-bottom: 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
  transition: border 0.3s;
}
.input-field:focus {
  border-color: #4caf50;
}
.btn-primary {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
  transition: background-color 0.3s;
}
.btn-primary:hover {
  background-color: #45a049;
}
</style>
