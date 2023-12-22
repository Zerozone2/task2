<template>
    <span @click="iconClk" id="contactsIcon">
        <i class="fa-solid fa-address-book" style="color: #172154;"></i>
    </span>

    <div>
        <ContactInfo @deleteContact="deleteContact" @editContact="editContact" @closeContact="closeContact" @updateContactFn="updateContactFn"  :contactInfo="curContact" :updateContact="updateContact" :showContact="showContact"></ContactInfo>
    </div>



    <div id="contactList"  >
        <div>
            <input type="text" v-model="search" id="searchBtn" @keyup.enter="searchContact"/>
            
        </div>
        <div id="sortAddBtn">
            <p @click="changeSortMethod">Sort <i class="fa-solid fa-arrow-down-a-z" style="color: #183153;" v-show="sortAsc"></i>
                <i class="fa-solid fa-arrow-down-z-a" style="color: #183153;" v-show="!sortAsc"></i>
            </p>
           
            <p @click="addContact">Add Contact <i class="fa-solid fa-plus" style="color: #183153;"></i></p>
            
        </div>
        <div id="addBtn">
            
        </div>
        <div @click="openContact(contact)" id="contactItem" v-for="(contact,name) in contacts" :key="name">
            <span id="contactIcon">
                <i class="fa-solid fa-user" style="color: #26395a;"></i>
            </span>

            {{ contact.name }}
        </div>
    </div>
</template>

<script>

import ContactInfo from './ContactInfo.vue';

export default {
    name: 'Contacts',
    data() {
        return {
            curContact: { name: "", email: [] },
            search: '',
            sortAsc: true,
            showContact: false,
            updateContact: false
        };
    },
    props: {
        contacts: Array
    },
    compements: {
        ContactInfo,
    },
    methods: {
        iconClk() {
            let contactList = document.getElementById("contactList");
            let currentDisplay = contactList.style.display;
            contactList.style.display = currentDisplay === "none" ? "block" : "none";
        },
        openContact(contact) {
            this.iconClk();
            this.curContact = contact;
            this.showContact = true
            this.updateContact = false
        },
        updateContactFn(newContact) {
            
            this.showContact = false
            this.updateContact = false
            return this.$emit("updateContactFn", newContact)

        },
        searchContact(){

            if(this.search !== ''){
                const newContact = {
                    name: this.search,
                    email: [this.search]
                }
                this.search = '';
                return this.$emit('searchContact', newContact);
            }

        },
        addContact(){

            this.curContact = { name: "", email: [] }
            
            this.showContact = true
            this.updateContact = true

            this.iconClk();
        },
        deleteContact(newContact) {
            this.showContact = false
            this.updateContact = false
            return this.$emit('deleteContact', newContact);
        },
        changeSortMethod() {
            this.sortAsc = !this.sortAsc
            return this.$emit('changeSortMethod');
        },
        closeContact(){
            this.showContact = false
            this.updateContact = false
        },  
        editContact() {
            this.showContact = false
            this.updateContact = true
            this.showContact = true
        },
    },
    components: { ContactInfo }
}

</script>

<style scoped>
p{
    display: inline;
}

#contactsIcon {
    color: wheat;
    position: absolute;
    right: 5%;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    height: auto;
}

#searchBtn{
    margin: 2% 0;
    border: 0px solid;
    text-overflow: clip;
}

#searchBtn:focus{
    outline: none;
    border: 0px solid;
    

}

#contactList{
    opacity: 1;
    background: lightgrey;
    height: auto;
    width: 22%;
    position: absolute;
    right: 10%;
    top: 50%;
    transform: translateY(-50%);
    border-radius: 5%;
}

#contactItem{
    margin-top: 5%;
    margin-bottom: 5%;
    padding: 5%;
    position: relative;
    border-radius: 5%;
    
}

#contactItem:hover{
    padding: 5%;
    position: relative;
    background: grey;
}

#sortAddBtn{
    margin-bottom: 2%;
    margin-top: 2%;
    display: flex;
    justify-content: space-around;
    align-content: space-around;
    
}

#sortAddBtn p:hover{
    background: grey;
}
#sortAddBtn p{
    padding: 1%;
    border-radius: 10%;
}

#contactIcon {
    width: 5%;
    height: auto;
    position: absolute;
    left: 8%;
    top: 50%;
    transform: translateY(-50%);
}

.contact {
    width: 80%;
    height: 80%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateY(-50%);
    transform: translateX(-50%);
}


</style>