<template>
    <v-container>
      <v-row>
        <v-col md="6" lg="4" v-for="ad in ads" :key="ad.id">
          <v-card class="ad-card" :elevation="6">
            <v-img :src="ad.image" class="ad-image" />
            <v-card-title>{{ ad.title }}</v-card-title>
            <v-card-text>{{ ad.description }}</v-card-text>
            <v-card-actions>
              <v-btn text> {{ ad.price }} Lei</v-btn>
              <v-spacer />
              <v-btn color="primary" @click="buy(ad.id)">Buy</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <v-form @submit.prevent="postAd">
        <v-text-field v-model="title" label="Title" required></v-text-field>
        <v-textarea v-model="description" label="Description" required></v-textarea>
        <v-text-field v-model="price" label="Price" required></v-text-field>
        <v-file-input label="Image" v-model="image"></v-file-input>
        <v-btn type="submit" color="primary">Post Ad</v-btn>
      </v-form>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        ads: [
          {
            id: 1,
            title: 'Casti Airpods',
            description: 'Casti Airpods Pro',
            price: 500,
            image: require('@/assets/item1.jpeg')
          },
          {
            id: 2,
            title: 'Mouse Logitech',
            description: 'Mouse Logitech Mx Master 3',
            price: 300,
            image: require('@/assets/item2.jpeg')
          },
          {
            id: 3,
            title: 'Laptom Microsoft',
            description: 'A high-end mountain bike with full suspension',
            price: 1500,
            image: require('@/assets/item3.jpeg')
          }
        ],
        title: '',
        description: '',
        price: '',
        image: null
      };
    },
    methods: {
      buy(adId) {
        alert('You bought ad #' + adId);
      },
      postAd() {
        const newAd = {
          id: this.ads.length + 1,
          title: this.title,
          description: this.description,
          price: this.price,
          image: null
        };
        if (this.image) {
          // Read the file as a base64 string and set it as the ad image
          const reader = new FileReader();
          reader.onload = () => {
            newAd.image = reader.result;
            this.ads.push(newAd);
          };
          reader.readAsDataURL(this.image);
        } else {
          this.ads.push(newAd);
        }
        this.title = '';
        this.description = '';
        this.price = '';
        this.image = null;
      }
    }
  };
  </script>
  
  