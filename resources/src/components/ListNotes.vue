<template>
    <div class="listOfNotes">
        <ul>
            <li v-for="(row, index) in notes" :key="index" @click="noteClick(row.idNote)" >
                <button class="btn-note">
                    <label>{{ row.namaNote }} </label>
                    <span>{{ row.descNote }} </span>
                </button>
            </li>
            
        </ul>
    </div>
</template>

<script>
export default {
    name: 'listOfNotes',
    data: function () {
        return {
            notes : [ 
                 {idNote: 1, namaNote: 'SuperAdmin', descNote: 'Superadmin can do anything include system'},
                 {idNote: 2, namaNote: 'Admin', descNote: 'Admin can do anything without system '},
                 {idNote: 3, namaNote: 'User', descNote: 'User works under admin conditions'}
            ]
        }
    },
    
    methods: {
        noteClick(id){
            let showNote = this.notes.find( note => note.idNote === id );
            showNote.mode = 'update';
            this.$root.$emit('emitNote', showNote);
        },
        makeNewId(){
            let newId = 0;
            if (this.notes.length === 0) {
                newId = 1
            } else {
                newId = this.notes[this.notes.length - 1].idNote + 1;
            }
            return newId;
        }
    },
    mounted(){
        this.$root.$on('emitDelNote', dataNote => {
            // console.log(dataNote);
            if (dataNote.id === 0) {
                alert('Silahkan pilih data yang akan dihapus');
            } else {
                let indexNote = this.notes.findIndex ( note => note.idNote === dataNote.id );
                this.notes.splice(indexNote, 1);
            }
        });
        this.$root.$on('emitUpdNote', dataNote => {
            // console.log(dataNote);
            let indexNote = this.notes.findIndex ( note => note.idNote === dataNote.idNote );
            this.notes[indexNote].namaNote = dataNote.namaNote;
            this.notes[indexNote].descNote = dataNote.descNote;
        });
        this.$root.$on('emitSavedNote', dataNote =>{
            // console.log(dataNote);
            let newId = this.makeNewId();
            let newNote = {idNote:newId, namaNote: dataNote.namaNote, descNote: dataNote.descNote}
            this.notes.push(newNote);
            this.noteClick(newNote.idNote);
        });
    }
}
</script>

<style>
ul {
     list-style-type: none;
     padding: 0;
     margin:0px;
}
.btn-note {
     text-align: left;
     border: none;
     border-bottom: 1px solid gainsboro;
     padding: 0px 15px;
     cursor: pointer;
     outline: none;
     background: #f7f7f7;
     height: 150px;
     width: 100%;
}
.btn-note:hover {
     background:#eaeaea;
}
.btn-note label{
     display:block;
     color: #444444;
     font-size: 1.5em;
     margin-bottom: 15px;
}
.btn-note span{
    color: #545454;
}
</style>