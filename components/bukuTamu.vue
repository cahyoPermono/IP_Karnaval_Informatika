<template>
    <div class="surface-card px-4 md:px-6 py-2 shadow-6 border-round w-full">
        <Toast />
            <h1 class="text-3xl text-white">Subscribe untuk melihat lowongan kami</h1>
        <hr>
        <Form>
            <div class="card">
                <div class="mb-2">
                    <label for="nama" class="block text-900 font-medium mb-2">Nama<span
                            style="color:red;">*</span></label>
                    <Field class="w-full mb-3" id="nama" name="nama" :rules="isRequired" v-model="data.nama" />
                    <ErrorMessage name="nama"><small style="color:red;">Nama harus diisi</small></ErrorMessage>
                </div>
                <div class="mb-2">
                    <label for="nohp" class="block text-900 font-medium mb-2">No. HP<span
                            style="color:red;">*</span></label>
                    <Field class="w-full mb-3" id="nohp" name="nohp" :rules="isRequired" v-model="data.noHp" />
                    <ErrorMessage name="nohp"><small style="color:red;">No HP harus diisi</small></ErrorMessage>
                </div>
                <div class="mb-2">
                    <label for="email" class="block text-900 font-medium mb-2">Email<span
                            style="color:red;">*</span></label>
                    <Field class="w-full mb-3" id="email" name="email" :rules="validateEmail" v-model="data.email" />
                    <small style="color:red;"><ErrorMessage name="email" /></small>
                </div>
                <div class="mb-2">
                    <div class="formgrid grid">
                        <div class="field col">
                            <label for="jurusan" class="block text-900 font-medium mb-2">Jurusan<span
                                    style="color:red;">*</span></label>
                            <Field class="w-full mb-3" id="jurusan" name="jurusan" :rules="isRequired"
                                v-model="data.jurusan" />
                            <ErrorMessage name="jurusan"><small style="color:red;">Jurusan harus diisi</small>
                            </ErrorMessage>
                        </div>
                        <div class="field col">
                            <label for="Angkatan" class="block text-900 font-medium mb-2">Angkatan<span
                                    style="color:red;">*</span></label>
                            <Field class="w-full mb-3" id="Angkatan" name="Angkatan" :rules="isRequired"
                                v-model="data.angkatan" />
                            <ErrorMessage name="Angkatan"><small style="color:red;">Angkatan harus diisi</small>
                            </ErrorMessage>
                        </div>
                    </div>
                </div>
                <Button label="Masuk" icon="pi pi-user" class="p-button-warning mb-3" @click="masuk" />
            </div>
        </Form>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useToast } from "primevue/usetoast";

const toast = useToast();

const data = reactive({
    nama: "",
    noHp: "",
    jurusan: "",
    email: "",
    angkatan: ""
});

const isRequired = (value) => {
    if (!value) {
        return "This field is required";
    }
    return true;
};

const validateEmail = (value) => {
    isRequired(value)
    // if the field is not a valid email
    const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
    if (!regex.test(value)) {
        return 'Alamat email harus valid';
    }
    // All is good
    return true;
}


const masuk = () => {
    try {
        axios.post("http://localhost:3000/api/landing/guests", {
            name: data.nama,
            phone: data.noHp,
            major: data.jurusan,
            generation: data.angkatan,
            email: data.email,
        }).then(() => {
            toast.add({ severity: "info", summary: "Enjoy the Carnaval" });
            location.href = "/jobs"
        })
    } catch (err) {
        console.log(err);
    }
};

</script>

<style>
.mb-3 {
    height: 35px;
}
</style>