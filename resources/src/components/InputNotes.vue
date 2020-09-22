<template>
    <div class="InputOfNotes">
        <form @submit="submitNote">
             <div class="menu">
                <button type="button" @click="delNote" class="bg-danger btn btn-delete">Hapus</button>
                <button type="submit" class="bg-success btn" v-if="mode == 'save'">Save</button>
                <button type="button" @click="upNote" class="bg-success btn" v-if="mode == 'update'">Update</button>
            </div>
            <div class="content-input">
                <input type="text" class="text" placeholder="ID" v-model="id">
                <input type="text" class="text" placeholder="Judul" v-model="judul">
                <textarea class="text textarea" placeholder="Tulis apa yang kamupikirkan" v-model="deskripsi"></textarea>
            </div>
        </form>
    </div> 
</template>

<script>
export default {
    name: 'InputOfNotes',
    props: {
        propEditNote : {
            type : Function
        }
        
    },
    data: function () {
        return {
            id : 0,
            judul : '',
            deskripsi : '',
            mode : 'save'
        }
    },
    methods : {
        dataNote(){
            let dataNote = {
                idNote : this.id,
                namaNote : this.judul,
                descNote : this.deskripsi
            }
            return dataNote;
        },
        submitNote(e){
            e.preventDefault(); 
            let dataNote = this.dataNote();
            this.$root.$emit('emitSavedNote', dataNote);            
        },
        upNote(){
            let dataNote = this.dataNote();
            this.$root.$emit('emitUpdNote', dataNote);
        },
        delNote(){
            let dataNote = {id : this.id}
            this.$root.$emit('emitDelNote', dataNote);
            this.emptyNote();
        },
        emptyNote(){
            this.id = 0;
            this.judul = '';
            this.deskripsi = '';
        }
    },
    mounted(){
        this.$root.$on('emitNote', showNote => {
            this.id = showNote.idNote;
            this.judul = showNote.namaNote;
            this.deskripsi = showNote.descNote;
            this.mode = showNote.mode;

        });
        this.$root.$on('emitEmptyNote', showEmptyNote => {
            this.id = showEmptyNote.idNote;
            this.judul = showEmptyNote.namaNote;
            this.deskripsi = showEmptyNote.descNote;
            this.mode = showEmptyNote.mode;

        });
    }
}
</script>

<style>
.menu{
     background: #f7f7f7;
     padding:10px 25px;
     margin-bottom: 25px;
     text-align:right;
     border-bottom: 1px solid #e8e6e6;
}
.btn-delete{ margin-right:10px; }

.content-input{
     padding: 0px 25px;
}
.text{
     display: block;
     width: 100%;
     padding: 0px;
     font-size: 20px;
     font-weight: bold;
     color: #2c3e50;
     border: none;
     margin-bottom: 10px;
     box-sizing: border-box;
     outline: none;
}
.textarea{
     min-height: 350px;
     font-size: 15px;
     font-weight: lighter;
     line-height: 30px;
}
.loader{
     vertical-align: middle;
}
</style>