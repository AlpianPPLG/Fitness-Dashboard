<template>
  <div
    :class="[
      'min-h-screen p-8',
      isDarkTheme ? 'bg-gray-900 text-white' : 'bg-gray-100 text-gray-900'
    ]"
  >
    <h1 class="text-4xl font-bold mb-6 text-center">Fitness Dashboard</h1>
    <div class="flex justify-between mb-6">
      <button
        @click="toggleTheme"
        :class="[
          'btn-primary',
          isDarkTheme ? 'bg-gray-200 text-gray-900' : 'bg-blue-500 text-white'
        ]"
      >
        {{ isDarkTheme ? 'Tema Terang' : 'Tema Gelap' }}
      </button>
      <input v-model="search" placeholder="Cari Anggota" class="input-field" />
    </div>
    <StatisticsMember :groups="groups" />
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <AddMember
        :groups="groups"
        @memberAdded="addMemberToList"
        @updateMember="updateMember"
        :memberToEdit="selectedMember"
      />
      <AddGroup @groupAdded="addGroupToList" />
      <GroupList
        :groups="filteredGroups"
        @removeGroup="removeGroup"
        @removeMember="removeMember"
        @editMember="setMemberToEdit"
      />
    </div>
    <CalendarGym :events="events" @addEvent="addEvent" />

    <footer class="mt-10 text-center">
      <button @click="showPolicyModal = true" class="btn-policy">Lihat Kebijakan</button>
    </footer>

    <ModalPopUp v-if="showPolicyModal" @close="showPolicyModal = false">
      <template #header>
        <h2 class="text-xl font-bold">Kebijakan</h2>
      </template>
      <template #body>
        <p class="text-gray-700">Ini adalah konten kebijakan. Silakan baca dengan seksama.</p>
      </template>
      <template #footer>
        <button @click="showPolicyModal = false" class="btn-primary">Tutup</button>
      </template>
    </ModalPopUp>
  </div>
</template>

<script>
import AddMember from './components/AddMember.vue'
import AddGroup from './components/AddGroup.vue'
import GroupList from './components/GroupList.vue'
import StatisticsMember from './components/StatisticsMember.vue'
import CalendarGym from './components/CalendarGym.vue'
import ModalPopUp from './components/ModalPopUp.vue'

export default {
  components: { AddMember, AddGroup, GroupList, StatisticsMember, CalendarGym, ModalPopUp },
  data() {
    return {
      groups: [],
      events: [],
      nextGroupId: 1,
      nextMemberId: 1,
      search: '',
      isDarkTheme: false,
      selectedMember: null,
      showPolicyModal: false // Kontrol untuk menampilkan modal kebijakan
    }
  },
  computed: {
    filteredGroups() {
      return this.groups.map((group) => ({
        ...group,
        members: group.members.filter((member) =>
          member.name.toLowerCase().includes(this.search.toLowerCase())
        )
      }))
    }
  },
  methods: {
    toggleTheme() {
      this.isDarkTheme = !this.isDarkTheme
      this.saveToLocalStorage()
    },
    addMemberToList(member) {
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
      this.saveToLocalStorage()
    },
    addGroupToList(group) {
      this.groups.push({
        id: this.nextGroupId++,
        name: group.name,
        members: []
      })
      this.saveToLocalStorage()
    },
    removeGroup(groupId) {
      if (confirm('Apakah Anda yakin ingin menghapus grup ini?')) {
        this.groups = this.groups.filter((group) => group.id !== groupId)
        this.saveToLocalStorage()
      }
    },
    removeMember(groupId, memberId) {
      if (confirm('Apakah Anda yakin ingin menghapus anggota ini?')) {
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
        this.saveToLocalStorage()
      }
    },
    setMemberToEdit(member) {
      this.selectedMember = member
    },
    updateMember(member) {
      const updatedGroups = this.groups.map((group) => {
        if (group.id === member.groupId) {
          return {
            ...group,
            members: group.members.map((m) => (m.id === member.id ? member : m))
          }
        }
        return group
      })
      this.groups = updatedGroups
      this.selectedMember = null
      this.saveToLocalStorage()
    },
    addEvent(event) {
      this.events.push(event)
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('groups', JSON.stringify(this.groups))
      localStorage.setItem('events', JSON.stringify(this.events))
      localStorage.setItem('nextGroupId', this.nextGroupId)
      localStorage.setItem('nextMemberId', this.nextMemberId)
      localStorage.setItem('isDarkTheme', this.isDarkTheme)
    },
    loadFromLocalStorage() {
      const storedGroups = JSON.parse(localStorage.getItem('groups'))
      const storedEvents = JSON.parse(localStorage.getItem('events'))
      const storedNextGroupId = localStorage.getItem('nextGroupId')
      const storedNextMemberId = localStorage.getItem('nextMemberId')
      const storedTheme = localStorage.getItem('isDarkTheme')

      if (storedGroups) this.groups = storedGroups
      if (storedEvents) this.events = storedEvents
      if (storedNextGroupId) this.nextGroupId = parseInt(storedNextGroupId)
      if (storedNextMemberId) this.nextMemberId = parseInt(storedNextMemberId)
      if (storedTheme !== null) this.isDarkTheme = storedTheme === 'true'
    }
  },
  mounted() {
    this.loadFromLocalStorage()
  }
}
</script>

<style scoped>
.input-field {
  @apply border border-gray-300 p-3 mb-4 rounded-lg transition duration-200 focus:outline-none focus:ring-2 focus:ring-blue-500;
}

.btn-primary {
  @apply p-3 rounded-lg shadow-md hover:bg-blue-600 transition duration-200;
}

.btn-policy {
  @apply mt-4 text-blue-700 underline hover:text-blue-500 transition duration-200;
}

.group-card {
  @apply bg-white p-4 rounded-lg shadow-lg transition duration-200 hover:shadow-xl;
}

.group-name {
  @apply font-bold text-lg mb-2;
}
</style>
