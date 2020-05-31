<template>
   <div>
      <h1 style="marginBottom : 0.5em">Thai Chana Let's Go</h1>
      <div v-for="item of LIST" v-bind:key="item.name" class="card-container">
         <a-card
            size="small"
            class="checkin-card"
            :bordered="true"
            :hoverable="true"
         >
            <a-icon
               type="delete"
               v-if="item.type === 'new'"
               slot="extra"
               v-on:click="removeNewItem(item)"
            />
            <div class="card-content">
               <div>
                  <img
                     style="borderRadius:5px"
                     width="70"
                     loading="lazy"
                     alt="card_cover"
                     v-bind:src="item.image"
                  />
               </div>
               <h3>
                  {{ item.name }}
               </h3>
            </div>
            <a-button
               type="primary"
               v-on:click="handleCheckIn(item)"
               style="width:100%;marginTop : 1em;borderRadius : 10px"
            >
               Check In
            </a-button>
         </a-card>
      </div>
      <a-button
         v-if="showAddNew === false"
         type="dashed"
         icon="plus"
         v-on:click="openAddNew()"
      >
         Add New
      </a-button>
      <div v-if="showAddNew === true">
         <h4>Add new</h4>
         <a-input
            v-model="newName"
            placeholder="Input name"
            style="width:350px;marginBottom:1em"
         />
         <a-input
            v-model="newUrl"
            placeholder="Input link"
            style="width:350px"
         />
      </div>
      <a-button
         :disabled="newName == '' || newUrl === '' ? true : false"
         v-if="showAddNew === true"
         type="primary"
         icon="save"
         style="marginTop:1em"
         v-on:click="saveNew()"
      >
         Save
      </a-button>
   </div>
</template>

<script>
export default {
   name: 'ThaiChana',
   data() {
      return {
         showAddNew: false,
         newName: '',
         newUrl: '',
         LIST: [],
      };
   },
   // beforeCreate() {
   //    const localData = JSON.parse(localStorage.getItem('checkin-list'));
   //    console.log(localData);
   //    localStorage.setItem('checkin-list', JSON.stringify(localData));
   // },
   created() {
      const localData = JSON.parse(localStorage.getItem('checkin-list'));
      if (localData === null) {
         this.LIST = [
            {
               id: 1,
               url: 'https://qr.thaichana.com/?appId=0001&shopId=S0000051771',
               name: 'Chamchuri Square',
               image:
                  'https://media-cdn.tripadvisor.com/media/photo-s/0c/4e/cf/06/caption.jpg',
               score: 0,
            },
            {
               id: 2,
               url: 'https://qr.thaichana.com/?appId=0001&shopId=S0000005524',
               name: '7/11 Chamchuri Square',
               image:
                  'https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/7-11TH.svg/1200px-7-11TH.svg.png',
               score: 0,
            },
            {
               id: 3,
               url: 'https://qr.thaichana.com/?appId=0001&shopId=S0000050571',
               name: 'Samyan Mitrtown',
               image:
                  'https://static.bangkokpost.com/media/content/20190919/c1_1753839.jpg',
               score: 0,
            },
         ];
         localStorage.setItem('checkin-list', JSON.stringify(this.LIST));
      } else {
         localData.sort(function(a, b) {
            return b.score - a.score;
         });
         this.LIST = localData;
      }
   },
   methods: {
      handleCheckIn(item) {
         const localData = JSON.parse(localStorage.getItem('checkin-list'));
         const addScore = localData.map((list) => {
            if (list.id === item.id) {
               list.score += 1;
            }
            return list;
         });
         localStorage.setItem('checkin-list', JSON.stringify(addScore));
         window.location.assign(item.url);
      },
      openAddNew() {
         this.showAddNew = true;
      },
      saveNew() {
         const localData = JSON.parse(localStorage.getItem('checkin-list'));
         localData.push({
            id: Date.now(),
            type: 'new',
            score: 0,
            url: this.newUrl,
            name: this.newName,
            image:
               'https://scontent.fbkk22-3.fna.fbcdn.net/v/t1.0-9/99010878_112528737130108_1364544554849533952_n.jpg?_nc_cat=103&_nc_sid=85a577&_nc_eui2=AeHkJSOO_anhF8UcH5-bLJwBre4wJl-zQ7-t7jAmX7NDv0jvdNnTfiIdX6rl1giXTfsrxCEX_ORrsJ-zRWsbpEQu&_nc_ohc=wCEVLMz7NhQAX8F9l7Z&_nc_ht=scontent.fbkk22-3.fna&oh=a9f0ff33abe065230c49fa837c7a9ad5&oe=5EF414ED',
         });
         localStorage.setItem('checkin-list', JSON.stringify(localData));
         this.LIST = localData;
         this.showAddNew = false;
      },
      removeNewItem(item) {
         const localData = JSON.parse(localStorage.getItem('checkin-list'));
         const newData = localData.filter((list) => list.id !== item.id);
         localStorage.setItem('checkin-list', JSON.stringify(newData));
         this.LIST = newData;
      },
   },
};
</script>

<style scoped>
.checkin-card {
   width: 350px;
   margin: 0 auto;
   border-radius: 15px;
}
.card-content {
   display: flex !important;
   flex-direction: row;
   justify-content: space-evenly;
   align-items: center;
}

.card-container {
   margin-bottom: 1.5em;
}
</style>
