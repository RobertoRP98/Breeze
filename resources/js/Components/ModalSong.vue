<script setup>
import {useForm} from '@inertiajs/vue3';
import TextInput from '@/Components/TextInput.vue';



const props = defineProps({
    song:{type:Object, default:() => ({})},
    modal:String, title:String, op:String
});

const form = useForm({
    id:props.song.id,
    title:props.song.title,
    author:props.song.author,
    album:props.song.album
});

const save = () => {
    form.post(route('songs.store'),{
        onSuccess:() => cerrar()
    });

}

const update = () =>{
    var id=document.getElementById('id2').value;
    form.put(route('songs.update',id),{
        onSuccess:() => cerrar()
    });
}

const cerrar = () =>{
    form.reset();
    document.querySelector('#cerrar'+props.op).click();
};



</script>

<template>

<div class="modal fade" :id="modal" tabindex="5" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <label class="h5">{{ title }}</label>
                <button class="btn-close" type="button" data-bs-dismiss="modal"
                aria-label="Close"></button>
            </div>
        <div class="modal-body">
            <form @submit.prevent="(op==='1' ? save() : update())">
                <TextInput :id="'id'+op" type="hidden" name="id"
                v-model="form.id"></TextInput>
                <div class="input-group mb-3">
                    <span class="input-group-text">
                        <i class="fa-solid fa-music"></i> </span>
                        <TextInput :id="'title'+op" class="form-control" type="text"
                        name="title" v-model="form.title" maxlength="120" placeholder="Cancion"
                        required></TextInput>
                </div>
                <div v-if="form.errors.title" class="text-sm text-danger">
                    {{ form.errors.title}}
                </div>
                <!-- INICIA OTRO-->
                <div class="input-group mb-3">
                    <span class="input-group-text">
                        <i class="fa-solid fa-user"></i></span>
                        <TextInput :id="'author'+op" class="form-control" type="text"
                        name="author" v-model="form.author" maxlength="120" placeholder="Author"
                        required></TextInput>
                </div>
                <div v-if="form.errors.author" class="text-sm text-danger">
                    {{ form.errors.author}}
                </div>
                <!-- INICIA OTRO-->
                <div class="input-group mb-3">
                    <span class="input-group-text">
                        <i class="fa-solid fa-compact-disc"></i></span>
                        <TextInput :id="'album'+op" class="form-control" type="text"
                        name="album" v-model="form.album" maxlength="120" placeholder="Album"
                        required></TextInput>
                </div>
                <div v-if="form.errors.album" class="text-sm text-danger">
                    {{ form.errors.album}}
                </div>

                <div class="d-grid mx-auto">
                    <button class="btn btn-success" :disabled="form.processing">
                    <i class="fa-solid fa-floppy-disk"></i> Guardar
                    </button>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-dark" type="submit" :id="'cerrar'+op"
            data-bs-dismiss="modal">Cerrar</button>
        </div>
    </div>

    </div>
</div>


</template>