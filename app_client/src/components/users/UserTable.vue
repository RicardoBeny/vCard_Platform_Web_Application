<script setup>
import { inject } from "vue";
import avatarNoneUrl from '@/assets/avatar-none.png'
import { useUserStore } from '../../stores/user.js'

const userStore = useUserStore()
const serverBaseUrl = inject("serverBaseUrl");

const props = defineProps({
  users: {
    type: Array,
    default: () => [],
  },
  showId: {
    type: Boolean,
    default: true,
  },
  showEmail: {
    type: Boolean,
    default: true,
  },
  showTypeOfUser: {
    type: Boolean,
    default: true,
  },
  showPhoto: {
    type: Boolean,
    default: true,
  },
  showEditButton: {
    type: Boolean,
    default: true,
  },
  showDeleteButton: {
    type: Boolean,
    default: true,
  },
});

const emit = defineEmits(["edit" , "delete"]);

const photoFullUrl = (user) => {
  return user.photo_url
    ? serverBaseUrl + "/storage/fotos/" + user.photo_url
    : avatarNoneUrl;
};

const editClick = (user) => {
  emit("edit", user);
};

const deleteClick = (user) => {
  emit("delete", user);
};

</script>

<template>
  <table class="table">
    <thead>
      <tr>
        <th v-if="showId" class="align-middle">#</th>
        <th v-if="showPhoto" class="align-middle">Photo</th>
        <th class="align-middle">Name</th>
        <th v-if="showEmail" class="align-middle">Email</th>
        <th v-if="showTypeOfUser" class="align-middle">Type of User</th>
        <th v-if="showEditButton || showDeleteButton"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users" :key="user.id" v-show="user.id != userStore.user.id">
        <td v-if="showId" class="align-middle">{{ user.id }}</td>
        <td v-if="showPhoto" class="align-middle">
          <img :src="photoFullUrl(user)" class="rounded-circle img_photo" />
        </td>
        <td class="align-middle">{{ user.name }}</td>
        <td v-if="showEmail" class="align-middle">{{ user.email }}</td>
        <td v-if="showTypeOfUser" class="align-middle">{{ user.user_type == 'V' ? 'Vcard' : 'Admin'}}</td>
        <td class="text-end align-middle" v-if="showEditButton || showDeleteButton">
          <div class="d-flex justify-content-end">
            <button
              class="btn btn-xs btn-light"
              @click="editClick(user)"
              v-if="showEditButton"
            >
              <i class="bi bi-xs bi-pencil"></i>
            </button>
            <button
              class="btn btn-xs btn-light"
              @click="deleteClick(user)"
              v-if="user.user_type == 'A'"
            ><i class="bi bi-xs bi-x-square-fill"></i>
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
button {
  margin-left: 3px;
  margin-right: 3px;
}

.img_photo {
  width: 3.2rem;
  height: 3.2rem;
}
</style>
