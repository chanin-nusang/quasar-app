<template>
  <q-page class="bg-grey-3 coloumn">

    <q-dialog v-model="dialog" :position="position" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Add Contact</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input 
            class="col" 
            square
            bg-color="white"
            v-model="name" 
            placeholder="Name" 
            dense>
          </q-input>
          <q-input 
            class="col" 
            square
            bg-color="white"
            v-model="phone" 
            placeholder="Phone Number" 
            dense>
          </q-input>
          <q-input 
            class="col" 
            square
            bg-color="white"
            v-model="email" 
            placeholder="E-Mail" 
            dense>
          </q-input>
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Add contact" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <div class="row q-pa-sm">
      <q-space />
      <q-btn 
      @click.stop="open('bottom')"
      dense color="primary" icon="add" label="Add contact" />
      <!-- <q-input 
     class="col" 
     square
     filled 
     bg-color="white"
     @keyup.enter="addTask"
      v-model="newTask" 
      placeholder="Add contact" 
      dense>
        <template v-slot:append>
          <q-btn 
          @click="addTask" 
          round 
          dense 
          flat 
          icon="add" />
        </template>
      </q-input> -->
    </div>
    <q-list 
    class="bg-white"
    separator
    bordered
    >
      <q-item 
      v-for="(contact, index) in contacts"
      :key="contact.name"
      clickable
      v-ripple>
        <q-item-section avatar>
          <q-btn 
          @click.stop="dial(contact.phone)"
          flat round dense color="primary" icon="call"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ contact.name }}</q-item-label>
        </q-item-section>
        <q-item-section side
         >
          <q-btn 
          @click.stop="deleteContact(index)"
          flat round dense color="primary" icon="delete"/>
        </q-item-section>
      </q-item>
      
    </q-list>
    
    <div v-if="!contacts.length"
    class="no-tasks absolute-center">
      <q-icon name="clear"
      size="100px"
      color="primary"/>
      <div class="text-h5 text-primary text-center">No contact</div>
    </div>

    </q-page>

</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
  setup () {
    const dialog = ref(false)
    const position = ref('top')

    return {
      dialog,
      position,

      open (pos) {
        position.value = pos
        dialog.value = true
        
      }
    }
  },
  data() {
    return {
      name: '',
      phone: '',
      email: '',
      contacts: [
        {
          name: 'Chanin Nusang',
          phone: '0619823990',
          email: 'realchanin@gmail.com'
        }
      ]
    }
  },
  methods: {
    addContact() {
      this.contacts.push({
        name: this.name,
        phone: this.phone,
        email: this.email
      })
      this.name = ''
      this.phone = ''
      this.email = ''
    },
    deleteContact(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
       this.contacts.splice(index, 1)
       this.$q.notify('Contact deleted')
      })
    },
    dial(number) {
      window.location ='tel:'+number;
    }
  }
})
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-task {
    opacity: 0.5;
  }
</style>