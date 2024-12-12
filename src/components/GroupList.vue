<template>
  <div>
    <h2 class="text-lg font-bold mb-4">Groups</h2>
    <ul>
      <li v-for="group in groups" :key="group.id" class="mb-4">
        <div class="bg-white p-4 rounded shadow">
          <h3 class="text-xl font-bold">{{ group.name }}</h3>
          <ul class="mt-2">
            <li v-for="member in group.members" :key="member.id" class="mb-2 flex justify-between">
              <span>{{ member.name }} ({{ member.age }}, {{ member.fitnessLevel }})</span>
              <div>
                <button @click="editMember(member)" class="btn-primary">Edit</button>
                <button @click="removeMember(group.id, member.id)" class="btn-danger">Hapus</button>
              </div>
            </li>
          </ul>
          <button @click="removeGroup(group.id)" class="btn-danger mt-2">Hapus Grup</button>
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
    },
    removeMember(groupId, memberId) {
      this.$emit('removeMember', groupId, memberId)
    },
    editMember(member) {
      this.$emit('editMember', member)
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
