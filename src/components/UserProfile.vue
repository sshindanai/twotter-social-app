<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{user.username}}</h1>
            <br>
             <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <br>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{followers}}
                <hr>
            </div>
            <form class="user-profile__create-twoot" @submit.prevent>
                <label for="newTwoot"><strong>New Twoot</strong></label>
                <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{option.name}}
                        </option>
                    </select>
                </div>
                <button id="createTwootButton" @click="createNewTwoot()">Twoot!</button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" 
                :username="user.username" 
                :twoot="twoot" 
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
import TwootItem from "./TwootItem"
export default {
    name: "UserProfile",
    data() {
        return {
            newTwootContent:'',
            selectedTwootType:'instant',
            followers: 0,
            user: {
                id: 1,
                username: '_John_Doe_',
                firstName: 'John',
                lastName: 'Doe',
                email: 'johndoe123@shmail.com',
                isAdmin: true,
                twoots: [
                    {id:1, content:'Twotter is amazing!'},
                    {id:2, content: "Don't forget to subscribe"},
                    {id:3, content:'Clubhouse is good'},
                    {id:4, content:'I love Canada! :D'},
                ]
            },
            twootTypes: [
                {value: 'draft', name:'Draft'},
                {value: 'instant', name:'Instant'},
            ],
        }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    toggleFavourite(id) {
        console.log(`Favourite tweet #${id}`)
    },
    createNewTwoot() {
        if (this.newTwootContent && this.selectedTwootType !== 'draft') {
            this.user.twoots.unshift({
                id: this.user.twoots.length + 1,
                content: this.newTwootContent
            })
        }
    }
  },
  mounted() {
    this.followerUser()
  },
  components: {
    TwootItem
  }
}
</script>

<style scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}

.user-profile__admin-badge {
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
    margin-bottom: 20px;
}

h1 {
    margin: 0;
}

#createTwootButton {
    width: 10rem;
    height: 2.5rem;
    background-color: #F48328;
    color: white;
    font-weight: bold;
    font-size: 1.2em;
    border-radius: 7.5px;
    border: 1px solid #DFE3E8;
}

#createTwootButton:focus {
    border: 1px solid #F48328;
}

.user-profile__create-twoot {
    padding-top: 20px;
    display: flex;
    flex-direction: column;
}

.user-profile__create-twoot-type {
    margin: 30px 0;
}
</style>