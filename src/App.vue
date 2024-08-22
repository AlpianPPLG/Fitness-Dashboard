<template>
  <div class="min-h-screen bg-gray-100 p-8">
    <h1 class="text-3xl font-bold mb-8 text-center">Fitness Dashboard</h1>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <!-- Add Member Component -->
      <AddMember :groups="groups" @memberAdded="addMemberToList" />

      <!-- Add Group Component -->
      <AddGroup @groupAdded="addGroupToList" />

      <!-- Group List Component -->
      <GroupList :groups="groups" @removeGroup="removeGroup" @removeMember="removeMember" />
    </div>
  </div>
</template>

<script>
import AddMember from './components/AddMember.vue'
import AddGroup from './components/AddGroup.vue'
import GroupList from './components/GroupList.vue'

export default {
  components: { AddMember, AddGroup, GroupList },
  data() {
    return {
      groups: [], // Array to hold groups and members
      nextGroupId: 1, // Unique ID for each new group
      nextMemberId: 1 // Unique ID for each new member
    }
  },
  methods: {
    addMemberToList(member) {
      // Logic to add member to the list
      const updatedGroups = this.groups.map((group) => {
        if (group.id === member.groupId) {
          return {
            ...group,
            members: [...group.members, { ...member, id: this.nextMemberId++ }]
          }
        }
        return group
      })
      this.groups = updatedGroups
    },
    addGroupToList(group) {
      // Add group to the list logic
      this.groups.push({
        id: this.nextGroupId++,
        name: group.name,
        members: []
      })
    },
    removeGroup(groupId) {
      // Remove group logic
      this.groups = this.groups.filter((group) => group.id !== groupId)
    },
    removeMember(groupId, memberId) {
      // Remove member logic
      const updatedGroups = this.groups.map((group) => {
        if (group.id === groupId) {
          return {
            ...group,
            members: group.members.filter((member) => member.id !== memberId)
          }
        }
        return group
      })
      this.groups = updatedGroups
    }
  }
}
</script>

<style scoped>
.input-field {
  @apply border p-2 mb-2 w-full;
}

.btn-primary {
  @apply bg-blue-500 text-white p-2 rounded;
}

.btn-danger {
  @apply bg-red-500 text-white p-1 rounded ml-2;
}

.group-card {
  @apply bg-gray-100 p-4 rounded-lg mb-4;
}

.group-name {
  @apply font-bold text-lg mb-2;
}
</style>
