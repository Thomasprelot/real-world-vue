<template>
  <div>
    <h1>Create an Event</h1>
    <form @submit.prevent="createEvent">
      <BaseSelect
        label="Select a category"
        v-model="event.category"
        :options="categories"
        :class="{ error: $v.event.category.$error }"
        @blur="$v.event.category.$touch()"
      />
      <template v-if="$v.event.category.$error">
        <p v-if="!$v.event.category.required">Category is required</p>
      </template>

      <h3>Name & describe your event</h3>
      <BaseInput
        label="Title"
        placeholder="Title"
        class="field"
        v-model="event.title"
        :class="{ error: $v.event.title.$error }"
        @blur="$v.event.title.$touch()"
      />
      <template v-if="$v.event.title.$error">
        <p v-if="!$v.event.title.required">Title is required</p>
      </template>

      <BaseInput
        label="Description"
        placeholder="Description"
        class="field"
        v-model="event.description"
        :class="{ error: $v.event.description.$error }"
        @blur="$v.event.description.$touch()"
      />
      <template v-if="$v.event.description.$error">
        <p v-if="!$v.event.description.required">Description is required</p>
      </template>

      <h3>Where is your event?</h3>
      <BaseInput
        label="Location"
        placeholder="Location"
        class="field"
        v-model="event.location"
        :class="{ error: $v.event.location.$error }"
        @blur="$v.event.location.$touch()"
      />
      <template v-if="$v.event.location.$error">
        <p v-if="!$v.event.location.required">Location is required</p>
      </template>

      <h3>When is your event?</h3>

      <div class="field">
        <label>Date</label>
        <datepicker
          v-model="event.date"
          placeholder="Select a date"
          :input-class="{ error: $v.event.date.$error }"
          @closed="$v.event.date.$touch()"
        ></datepicker>
      </div>
      <template v-if="$v.event.date.$error">
        <p v-if="!$v.event.date.required">Date is required</p>
      </template>

      <BaseSelect
        label="Select a time"
        :options="times"
        v-model="event.time"
        @blur="$v.event.time.$touch()"
        class="field"
        :class="{ error: $v.event.time.$error }"
      />
      <template v-if="$v.event.time.$error">
        <p v-if="!$v.event.time.required">Time is required</p>
      </template>

      <BaseButton
        type="submit"
        buttonClass="button-fill-gradient"
        :disabled="$v.$anyerror"
        >Submit</BaseButton
      >
      <p v-if="$v.$anyError" class="errorMesage">
        Please fill out the required field(s).
      </p>
    </form>
  </div>
</template>

<script>
import Datepicker from '@hokify/vuejs-datepicker'
import NProgress from 'nprogress'
import { required } from 'vuelidate/lib/validators'

export default {
  components: { Datepicker },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  validations: {
    event: {
      category: { required },
      organizer: { required },
      title: { required },
      description: { required },
      location: { required },
      date: { required },
      time: { required }
    }
  },
  methods: {
    createEvent() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        NProgress.start()
        this.$store
          .dispatch('event/createEvent', this.event)
          .then(() => {
            this.$router.push({
              name: 'event-show',
              params: { id: this.event.id }
            })
            this.event = this.createFreshEventObject()
          })
          .catch(() => {
            NProgress.done()
          })
      }
    },
    createFreshEventObject() {
      const user = this.$store.state.user.user
      const id = Math.floor(Math.random() * 10000000)

      return {
        id: id,
        user: user,
        category: '',
        organizer: user,
        title: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  }
}
</script>

<style scoped>
.field {
  margin-bottom: 24px;
}
</style>
