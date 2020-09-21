<template>
    <div class="InputOfNotes">
        <form @submit="submitNote">
             <div class="menu">
                <button type="button" @click="delNote" class="bg-danger btn btn-delete">Hapus</button>
                <button type="submit" class="bg-success btn">Save</button>
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
        propSaveNote : {
            type: Function
        },
        propEditNote : {
            type : Function
        },
        propDelNote : {
            type : Function
        }
        
    },
    data: function () {
        return {
            id : 0,
            judul : '',
            deskripsi : ''
        }
    },
    methods : {
        submitNote(e){
            e.preventDefault();
            if (this.id === 0) {
                this.propSaveNote(this.judul, this.deskripsi);    
            }else{
                this.propEditNote(this.id, this.judul, this.deskripsi);
            }
            
        },
        delNote(){
            this.propDelNote(this.id);
            this.emptyNote();
        },
        emptyNote(){
            this.id = 0;
            this.judul = '';
            this.deskripsi = '';
        }
    },
    mounted(){
        this.$root.$on('emitNote', note => {
            this.id = note.idNote;
            this.judul = note.namaNote;
            this.deskripsi = note.descNote;

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