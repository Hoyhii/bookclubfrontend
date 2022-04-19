<template>
        <form>
            <div>
                <div v-if="vanHiba" class="alert alert-danger" role="alert">
                    {{ this.message }}
                </div>
                <div>
                    <label for="membername" class="form-label">Név</label>
                    <input v-model="member.name" type="text" class="form-control" id="membername">
                </div>
                <div>
                    <label for="membergender" class="form-label">Neme</label>
                    <input v-model="member.gender" type="text" class="form-control" id="membergender">
                </div>
                <div>
                    <label for="memberbirth" class="form-label">Születésnap</label>
                    <input v-model="member.birth_date" type="date" class="form-control" id="memberbirth">
                </div>
                <button @click="newBook" class="btn btn-primary">Submit</button>
            </div>
        </form>
</template>

<script>
export default {
    name: 'MemberForm',
    data() {
        return {
            vanHiba: false,
            message: '',
            member: {
                name: '',
                gender: '',
                birth_date: ''
            }
        }
    },
    methods: {
        async newBook() {
            let Response = await fetch('https://127.0.0.1:8000/api/members', {
                method: "POST",
                headers: {
                    'Content-type': 'application/json',
                    'Accept': 'application/json'
                },
                body: JSON.stringify(this.member)
            })
            let body =  await Response.json()
            if (Response.status == 201) {
                this.vanHiba = false
                this.member = {
                name: '',
                gender: '',
                birth_date: ''
                }
                this.$emit("newmember")
                return 
            }
            this.vanHiba = true,
            this.message = body.message
            
        }
    },
    
}
</script>

<style>

</style>
