<template>
  <div class="bg-white p-4 rounded-lg shadow mb-4">
    <h2 class="text-lg font-bold mb-4">Kalender Kegiatan</h2>
    <div class="mb-4">
      <input v-model="eventTitle" placeholder="Judul Kegiatan" class="input-field" />
      <input v-model="eventDate" type="date" class="input-field" />
      <input v-model="eventTime" type="time" class="input-field" />
      <button @click="addNewEvent" class="btn-primary">Tambah Kegiatan</button>
    </div>
    <ul>
      <li v-for="event in events" :key="event.id" class="mb-2">
        {{ event.title }} - {{ formatDateTime(event) }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    events: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      eventTitle: '',
      eventDate: '',
      eventTime: ''
    }
  },
  methods: {
    addNewEvent() {
      if (this.eventTitle && this.eventDate && this.eventTime) {
        const newEvent = {
          id: Date.now(),
          title: this.eventTitle,
          date: this.eventDate,
          time: this.eventTime
        }
        this.$emit('addEvent', newEvent)
        this.resetForm()
      } else {
        alert('Harap isi semua kolom')
      }
    },
    resetForm() {
      this.eventTitle = ''
      this.eventDate = ''
      this.eventTime = ''
    },
    formatDateTime(event) {
      return `${event.date} ${event.time}`
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
