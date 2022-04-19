<template>
        <article class="card">
            <img :src="picture" class="card-img-top" alt="...">
            <div class="card-body">
                <h5 class="card-title">{{ member.name }}</h5>
                <p class="card-text">Született: {{ member.birth_date }}</p>
                <p class="card-text">Csatlakozott: {{ member.created_at }}</p>
                <a @click="Payment" class="btn btn-primary">Tagdíj befizetés</a>
                <div v-if="vanHiba" class="alert alert-danger" role="alert">
                    {{ message }}
                </div>
                <div v-if="sikeresBefizetes" class="alert alert-primary" role="alert">
                    Sikeres befizetés!
                </div>
            </div>
        </article>
</template>

<script>
export default {
    name: 'memberItem',
    props: [
        "member"
        ],
    data() {
        return {
            vanHiba: false,
            message: '',
            sikeresBefizetes: false
        }
    },
    computed: {
        picture() {
            if (this.member.gender == 'M') {
                return "/kepek/male.png"
            }else if (this.member.gender == 'F'){
                return "/kepek/female.png"
            }
            return "/kepek/other.png"
        }
    },
    methods: {
        async Payment() {
            let Response = await fetch(`https://127.0.0.1:8000/api/members/${this.member.id}/pay`, {
                method: "POST",
                headers: {
                    'Content-type': 'application/json',
                    'Accept': 'application/json'
                },
            })
            let body = await Response.json()
            if (Response.status == 200) {
                this.sikeresBefizetes = true
                this.vanHiba = false
                return
            }
            this.message = body.message
            this.vanHiba = true
            this.sikeresBefizetes = false
        }
    },
    
}
</script>

<style>

</style>
