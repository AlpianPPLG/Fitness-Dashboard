<template>
  <div>
    <h2 class="text-lg font-bold mb-4">Groups</h2>
    <ul>
      <li v-for="group in groups" :key="group.id" class="mb-4">
        <div class="bg-white p-4 rounded shadow">
          <h3 class="text-xl font-bold">{{ group.name }}</h3>
          <ul class="mt-2">
            <li v-for="member in group.members" :key="member.id" class="mb-2">
              {{ member.name }} ({{ member.age }}, {{ member.fitnessLevel }})
            </li>
          </ul>
          <button @click="removeGroup(group.id)" class="btn-danger mt-2">Remove Group</button>
        </div>
      </li>
    </ul>
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
  methods: {
    removeGroup(groupId) {
      this.$emit('removeGroup', groupId)
    }
  }
}
</script>

<style scoped>
.btn-danger {
  background-color: #f44336;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
