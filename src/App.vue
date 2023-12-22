<template>
  <Contacts @deleteContact="deleteContact" @updateContactFn="updateContactFn" @searchContact="searchContact" @changeSortMethod="changeSortMethod" :contacts="this.contacts" />
  <Attachments />
  
  <Folders @deleteFolder="deleteFolder" @updateFolderFn="updateFolderFn" :folders="folders"/>
</template>

<script>

import Contacts from './components/Contacts.vue';
import Attachments from './components/Attachments.vue';
import Folders from './components/Folders.vue';

export default {
  name: 'App',
  data() {
    return {
      contacts: [],
      folders: [],
    }
  },
  components: {
    Contacts,
    Attachments,
    Folders
  },
  created() {
    this.contacts = [
        {
          name: "ali",
          email: [
            "ali@mail.com",
            "fry@ok.com",
            "hacker@mail.com"
          ]
        },
        {
          name: "samer",
          email: [
            "samer@mail.com"
          ]
        },
        {
          name: "ahmed",
          email: [
            "hacker@mail.com"
          ]
        },
      ],
    this.folders = [
      "parent",
      "safety"
    ]
  },
  watch: {

  },
  methods: {
    getContacts() {
      this.contacts
    },
    async searchContact(newContact) {
      
      const reuslt = await fetch('http://localhost:8080/contact/search')

      this.contacts = await reuslt.json()

    },
    changeSortMethod() {
      this.contacts.reverse()
    },
    async updateContactFn(curContact) {
      // PUT can be used with POST
      await fetch('http://localhost:8080/contact/add', {
        method: 'post',
        headers: {
              'Content-type': "application/json; charset=UTF-8"
            },
        body: JSON.stringify(curContact),
      })
      console.log(curContact)
      this.contacts = [ ...this.contacts,  curContact]

    },
    async deleteContact(newContact) {
      await fetch('http://localhost:8080/contact/delete', {
        method: 'delete',
        headers: {
              'Content-type': "application/json; charset=UTF-8"
            },
        body: JSON.stringify(newContact),
      })

      this.contacts = this.contacts.filter(item => item.name != newContact.name)
    },
    async updateFolderFn(newFolder) {
      // PUT can be used with POST
      await fetch('http://localhost:8080/folder/add', {
        method: 'post',
        headers: {
              'Content-type': "application/json; charset=UTF-8"
            },
        body: JSON.stringify(newFolder),
      })
      console.log(newFolder)
      this.folders = [ ...this.folders,  newFolder]
    },
    async deleteFolder(newFolder) {
      await fetch('http://localhost:8080/folder/delete', {
        method: 'delete',
        headers: {
              'Content-type': "application/json; charset=UTF-8"
            },
        body: JSON.stringify(newFolder),
      })

      this.folders = this.folders.filter(item => item != newFolder)
    }
  }
}
</script>

<style>
* {
  transition: all 0.5s;
  margin: 0;
}

#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100%;
  width: 100%;
}

</style>
