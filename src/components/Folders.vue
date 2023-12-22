<template>

    <div @click="iconClk" id="foldersIcon">
        <i class="fa-solid fa-folder-open" style="color: #183153;"></i>
    </div>

    <Folder @updateFolderFn="updateFolderFn" @editFolder="editFolder" @closeFolder="closeFolder" @deleteFolder="deleteFolder" :mails="mails" :updateFolder="updateFolder" :showFolder="showFolder" :curFolder="curFolder"></Folder>

    <div id="folderList"  >

        <div id="AddBtn">
           
            <p @click="addFolder">Add Folder  <i class="fa-solid fa-plus" style="color: #183153;"></i></p>
            
        </div>

        <div @click="openFolder(folder)" id="folderItem" v-for="(folder,index) in folders" :key="index">
            <span id="folderIcon">
                <i class="fa-solid fa-folder-open" style="color: #183153;"></i>
            </span>

            {{ folder }}
        </div>
    </div>


</template>

<script>

import Folder from './Folder.vue';

export default {
    name: 'Folders',
    data() {
        return {
            updateFolder: false,
            curFolder: "",
            showFolder: false
        }
    },
    props: {
        mails: Array,
        folders: Array
    },
    methods: {
        iconClk() {
            let folderList = document.getElementById("folderList");
            let currentDisplay = folderList.style.display;
            folderList.style.display = currentDisplay === "none" ? "block" : "none";
        },
        openFolder(folder) {
            this.iconClk();
            this.curFolder = folder;

            this.showFolder = true
            this.updateFolder = false
        },
        updateFolderFn(newFolder) {
            this.curFolder = newFolder
            this.showFolder = false
            this.updateFolder = false
            return this.$emit("updateFolderFn", newFolder)

        },
        addFolder(){

            this.curFolder = ""

            this.showFolder = true
            this.updateFolder = true

            this.iconClk();
        },
        closeFolder(){
            this.showFolder = false
            this.updateFolder = false
        },  
        editFolder() {
            this.showFolder = false
            this.updateFolder = true
            this.showFolder = true
        },
        deleteFolder(newFolder) {
            this.showFolder = false
            this.updateFolder = false
            return this.$emit('deleteFolder', newFolder);
        },
    },
    components: {
        Folder
    }
}

</script>

<style>

#folderIcon {
    width: 5%;
    height: auto;
    position: absolute;
    left: 8%;
    top: 50%;
    transform: translateY(-50%);
}


#foldersIcon {
    color: wheat;
    position: absolute;
    left: 5%;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    height: auto;
}

#folderList {
    opacity: 1;
    background: lightgrey;
    height: auto;
    width: 22%;
    position: absolute;
    left: 10%;
    top: 50%;
    transform: translateY(-50%);
    border-radius: 5%;
}

#AddBtn{
    margin-bottom: 2%;
    margin-top: 2%;
    display: flex;
    justify-content: space-around;
    align-content: space-around;
    cursor: pointer;
}

#folderItem{
    margin-top: 5%;
    margin-bottom: 5%;
    padding: 5%;
    position: relative;
    border-radius: 5%;
    cursor: pointer;
}

#folderItem:hover{
    padding: 5%;
    position: relative;
    background: grey;
}


#AddBtn p:hover{
    background: grey;
}
#AddBtn p{
    padding: 1%;
    border-radius: 10%;
}
</style>