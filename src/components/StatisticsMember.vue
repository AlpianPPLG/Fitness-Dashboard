<template>
  <div class="bg-white p-4 rounded-lg shadow mb-4">
    <h2 class="text-xl font-bold">Statistik Anggota</h2>
    <p>Total Anggota: {{ totalMembers }}</p>
    <p>Total Grup: {{ totalGroups }}</p>
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
  computed: {
    totalMembers() {
      return this.groups.reduce((acc, group) => acc + group.members.length, 0)
    },
    totalGroups() {
      return this.groups.length
    }
  }
}
</script>

<style scoped></style>
