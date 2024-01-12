<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import Swal from 'sweetalert2';
import { Head } from '@inertiajs/vue3';
import {useForm} from '@inertiajs/vue3'; 
import ModalSong from '@/Components/ModalSong.vue';



const form = useForm({});
const props = defineProps({
    songs:{type:Object}
});


const eliminar=(id,name) =>{
const swalWithBoostrapBootons = Swal.mixin({
    buttonsStyling:true
})
swalWithBoostrapBootons.fire({
    title:'Seguro de eliminar la canción' +name,
    text:'Se perdera la canción',
    icon:'question',
    showCancelButton:true,
    confirmButtonText:'<i class="fa-solid fa-check"></i> Si, eliminar',
    cancelButtonText:'<i class="fa-solid fa-ban"> </i> Cancelar'
}).then((result) => {
    if(result.isConfirmed){
        form.delete(route('songs.destroy',id));
    }
})
};

const openModal = (song) => {
    document.getElementById('id2').value = song.id;
    document.getElementById('title2').value = song.title;
    document.getElementById('author2').value = song.author;
    document.getElementById('album2').value = song.album;

};
</script>

<template>
<Head title="Songs"></Head>
<AuthenticatedLayout>
    <template #header>
        <div class="container-fluid mt-3 bg-white">
            <div class="row mt-3">
                <div class="col-md-4 offset-md-4">
                    <div class="d-grid mx-auto">
                        <button class="btn btn-dark" data-bs-toggle="modal"
                        data-bs-target="#modalCreate">
                        <i class="fa-solid fa-circle-plus"></i>
                        Añadir
                    </button>
                    </div>
                </div>
            </div>
            <div class="row mt-3">
                <div class="col-md-10 offset-md-1">
                    <div class="table-responsive">
                        <table class="table table-stripeted table-bordered">
                            <thead>
                                <tr>
                                    <th>#</th>
                                    <th>NOMBRE</th>
                                    <th>ARTISTA</th>
                                    <th>ALBUM</th>
                                    <th>EDITAR</th>
                                    <th>BORRAR</th>
                                </tr>
                            </thead>
                        <tbody>
                            <tr v-for="song, i in songs" :key="song.id">
                                <td>{{ i+1 }}</td>
                                <td>{{ song.title }}</td>
                                <td>{{ song.author }}</td>
                                <td>{{ song.album }}</td>

                                <td> 
                                <button class="btn btn-warning"
                                 data-bs-toggle="modal"
                                data-bs-target="#modalEdit" @click="openModal(song)">
                                <i class="fa-solid fa-edit"></i>
                                </button>
                                 </td>

                                 <td>
                                    <button class="btn btn-danger"
                                    @click="eliminar(song.id,song.title)">
                                    <i class="fa-solid fa-trash"></i>
                                    </button>
                                </td>

                            </tr>
                        </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
        <ModalSong :modal="'modalCreate'" :title="'Añadir Cancion'" :op="'1'"></ModalSong>
        <ModalSong :modal="'modalEdit'" :title="'Editar Cancion'" :op="'2'"></ModalSong>

    </template>
</AuthenticatedLayout>

</template>