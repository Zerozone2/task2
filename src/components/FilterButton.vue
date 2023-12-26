<template>

    <div id="filterButton">
        <input @change="filterMials" type="text" id="filterInput" />
        <button @click="addMialsFiltered">filter</button>
        <select id="filterBy">
            <option value="1">by</option>
            <option value="2">sender</option>
            <option value="3">subject</option>
        </select>
    </div>

    <div id="mailList"  class="toggle">

        <div id="folderItem" v-for="(mail,index) in mailsFiltered" :key="index">

            {{ mail }}

        </div>

    </div>

    <div id="folderList"  class="toggle">

        <div @click="addMailsToFolder(folder)" id="folderItem" v-for="(folder,index) in folders" :key="index">
            <span id="folderIcon">
                <i class="fa-solid fa-folder-open" style="color: #183153;"></i>
            </span>

            {{ folder }}
        </div>

    </div>

</template>

<script>

export default {
    name: 'FilterButton',
    data() {
        mailsFiltered: this.mails
    },
    props: {
        mails: Array,
        folders: Array
    },
    methods: {
        filterMials() {

            var filterInput = document.getElementById('filterInput');
            var textFilterInput = filterInput.value;

            const filterBy = document.getElementById('filterBy');
            const filterByType = filterBy.value;
            const selectedIndex = filterBy.selectedIndex;

            if(selectedIndex !== -1 && selectedIndex !== 0) {
                this.mailsFiltered = this.mails.filter( mail => mail[filterByType] == textFilterInput )
            }

        },
        addMialsFiltered() {

            // check box mechansiam

            var folderList = document.getElementById('folderList');
            folderList.style.display = folderList.style.display === "block" ? "none" : "block";

        },
        addMailsToFolder(folder) {
            
            this.mailsFiltered.forEach(element => {
                element["filter"].push(folder)
            });

        }
    }
}

</script>

<style scoped>
#filterButton {
    position: relative;
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
    display: none;
}

#folderIcon {
    width: 5%;
    height: auto;
    position: absolute;
    left: 8%;
    top: 50%;
    transform: translateY(-50%);
}
</style>