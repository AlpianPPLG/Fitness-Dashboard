<template>
  <div>
    <h2 class="text-lg font-bold mb-4">{{ isEditMode ? 'Edit Member' : 'Add Member' }}</h2>
    <form @submit.prevent="submitForm">
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
      <button type="submit" class="btn-primary">
        {{ isEditMode ? 'Update Member' : 'Add Member' }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    groups: {
      type: Array,
      required: true
    },
    memberToEdit: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      name: '',
      age: null,
      fitnessLevel: 'Beginner',
      selectedGroup: null,
      isEditMode: false
    }
  },
  watch: {
    memberToEdit: {
      immediate: true,
      handler(newVal) {
        if (newVal) {
          this.isEditMode = true
          this.name = newVal.name
          this.age = newVal.age
          this.fitnessLevel = newVal.fitnessLevel
          this.selectedGroup = newVal.groupId
        } else {
          this.resetForm()
        }
      }
    }
  },
  methods: {
    submitForm() {
      if (this.name && this.age && this.selectedGroup !== null) {
        const memberData = {
          name: this.name,
          age: this.age,
          fitnessLevel: this.fitnessLevel,
          groupId: this.selectedGroup,
          id: this.isEditMode ? this.memberToEdit.id : null // Tambahkan ID jika dalam mode edit
        }
        this.$emit(this.isEditMode ? 'updateMember' : 'memberAdded', memberData)
        this.resetForm()
      } else {
        alert('Please fill in all fields')
      }
    },
    resetForm() {
      this.name = ''
      this.age = null
      this.fitnessLevel = 'Beginner'
      this.selectedGroup = null
      this.isEditMode = false // Reset mode edit
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
