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
          id: Date.now(), // Unique ID for the new group
          name: this.groupName,
          members: [] // Initialize with an empty array for members
        }
        this.$emit('groupAdded', newGroup)
        this.groupName = '' // Reset the input field
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
