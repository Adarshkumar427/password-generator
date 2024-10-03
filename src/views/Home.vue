<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from '../../src/components/Header.vue'
import { ref, watch } from 'vue';
const password = ref('')
const passwordLength = ref(12)

const includeUpperCase = ref(true)
const includeLowerCase = ref(true)
const includeNumbers = ref(true)
const includeSymbols = ref(true)
const showSnackbar = ref(false)
const snackbarMessage = ref('')

const uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
const lowercase = 'abcdefghijklmnopqrstuvwxyz'
const digits = '0123456789'
const symbols = '!@#$%^&*()_+{}[]|:;<>,.?/~`'

function copyPassword() {
    const textarea = document.createElement("textarea");
    textarea.value = password.value;
    document.body.appendChild(textarea);
    textarea.select();
    document.execCommand("copy");
    document.body.removeChild(textarea);
    snackbarMessage.value = 'Password copied!'
    showSnackbar.value = true


}

function generatePassword() {
    let sets = ''
    if (includeUpperCase.value) {
        sets += uppercase
    }
    if (includeLowerCase.value) {
        sets += lowercase
    }
    if (includeSymbols.value) {
        sets += symbols
    }
    if (includeNumbers.value) {
        sets += digits
    }

    if (sets.length === 0) {
        password.value = ''
        return
    }

    let newPassword = ''
    for (let i = 1; i <= passwordLength.value; i++) {
        const randomIndex = Math.floor(Math.random() * sets.length)
        newPassword += sets[randomIndex]
    }
    password.value = newPassword
}

watch([passwordLength, includeUpperCase, includeLowerCase, includeNumbers, includeSymbols], generatePassword)

generatePassword()

</script>

<template>
    <Header />

    <v-app>
        <v-container class="d-flex justify-center mt-16 flex-column">
            <v-card class="pa-10 ">
                <p class="password ">{{ password }}</p>

                <v-divider color="green bold" class="mt-1 mb-3"></v-divider>
                <div class="d-flex">

                    <v-btn @click="copyPassword" icon="mdi-content-copy" size="x-small" color="green"
                        title="Copy Password"></v-btn>
                    <v-btn @click="generatePassword" class="ms-3" icon="mdi-refresh" size="x-small" color="red"
                        title="Reset Password"></v-btn>
                </div>
            </v-card>

            <v-card class="pa-10 my-4 ">
                <v-row>
                    <v-col style="font-size: 20px;">Customize your password</v-col>
                </v-row>
                <v-divider></v-divider>

                <div>
                    <div>
                        <v-row>
                            <v-col class="mt-3">
                                <span>password length</span>

                                <v-col>
                                    <v-row>
                                        <v-text-field density="compact" style="width: 5px;" v-model="passwordLength"
                                            type="number" />
                                        <v-col style="width:300px">
                                            <v-slider v-model="passwordLength" min="1" max="50" color="red" step="1" />
                                        </v-col>
                                    </v-row>
                                </v-col>
                            </v-col>
                        </v-row>
                    </div>
                    <v-spacer></v-spacer>
                    <div>
                        <v-row>
                            <v-col>
                                <v-checkbox v-model="includeUpperCase" label="uppercase" color="red" hide-details />
                                <v-checkbox v-model=includeLowerCase label="lowercase" color="red" hide-details />
                                <v-checkbox v-model="includeNumbers" label="numbers" color="red" hide-details />
                                <v-checkbox v-model="includeSymbols" label="symbols" color="red" hide-details />
                            </v-col>
                        </v-row>
                    </div>
                </div>
            </v-card>

            <v-snackbar color="green" v-model="showSnackbar" :timeout="1000">
                {{ snackbarMessage }}
            </v-snackbar>
        </v-container>
    </v-app>
</template>

<style scoped>
Header {
    position: fixed;
}

.password {
    font-size: 20px;

}
</style>
