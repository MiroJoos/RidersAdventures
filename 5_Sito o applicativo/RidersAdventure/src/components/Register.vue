<template>
    <div id="register">
        <input type="text" placeholder="NomeUtente" v-model="nomeUtente" required>
        <br>
        <br>
        <input type="email" placeholder="E-Mail" v-model="email" required>
        <br>
        <br>
        <input type="password" placeholder="Password" v-model="password" required>
        <br>
        <br>
        <p id="error" v-if="errorMsg"> {{ errorMsg }} </p>
        <br>
        <button @click="register">ISCRIVITI</button>
        <br>
        <p>Hai già un account?&nbsp;<a href="/login">Accedi</a></p>
    </div>
</template>

<script>
    import { getAuth, createUserWithEmailAndPassword } from "firebase/auth"

    export default {
        data() {
            return {
                nomeUtente:"",
                email: "",
                password: "",
                errorMsg: ""
            }
        },
        methods: {
            /** 
             * Password complexity:
             * 1. Length greather than 8 characters
             * 2. At least 1 uppercase character, 
             * 3. At least 1 lowercase character
             * 4. At least 1 special character (numbers are special)
             */ 
            checkPasswordComplexity() {
                var lowers = 0
                var uppers = 0
                var specials = 0

                if (this.password.length >= 8) {
                    for (var i = 0; i < this.password.length; i++) {
                        var character = this.password.charAt(i);

                        if (character >= '!' && character <= '~') {
                            if (character >= 'A' && character <= 'Z') {
                                uppers++
                            } else if (character >= 'a' && character <= 'z') {
                                lowers++
                            } else {
                                specials++
                            }
                        }
                    }

                    return lowers >= 1 && uppers >= 1 && specials >= 1
                } else {
                    return false
                }
            },
            register() {
                if (this.checkPasswordComplexity()) {
                    createUserWithEmailAndPassword(getAuth(), this.email, this.password)
                    .then((data) => {
                        console.log(data)
                    })
                    .catch((error) => {
                        switch (error.code) {
                            case "auth/email-already-in-use":
                                this.errorMsg = "L'utente esiste già.&nbsp<a href='/login'>Accedi</a>"
                                break
                        }
                    })
                } else {
                    this.errorMsg = "La password non soddisfa i requisiti minimi"
                }

                function registerNewUser() {
                    var email = document.getElementById("email").value;
                    var password = document.getElementById("password").value;
                    var nickname = document.getElementById("nickname").value;
                    var uid;

                    // creare nuovo account
                    auth.createUserWithEmailAndPassword(email, password)
                        .then((userCredential) => {
                            var uid = userCredential.user.uid;
                            db.ref("users/" + uid).set({
                                uid,
                                email,
                                nickname,
                                ban: false,
                                admin: false,
                            });
                        })
                        .catch((error) => {
                            const errorCode = error.code;
                            const errorMessage = error.message;
                            document.getElementById('login_error').innerHTML = error.message;
                        });
                }

            }
        }
    }
</script>

<style>
    #register {
        margin: auto;
        width: fit-content;
        padding: 100px;
        border: 2px solid black;
        border-radius: 25px;
    }

    #register input{
        width: 100%;
        height: 3vh;
    }

    #register label {
        text-align: left;
    }

    #register button {
        margin-bottom: 20px;
        height: 4vh;
        width: 100%;
        background: rgba(255, 68, 0, 0.906);
        color: white;
    }

    #error {
        color: red;
    }
</style>